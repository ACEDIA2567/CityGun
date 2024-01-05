```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class AmmoEvent : UnityEngine.Events.UnityEvent<int, int> { }
public class Shoot : MonoBehaviour
{
    [HideInInspector]
    public AmmoEvent onAmmoEvent = new AmmoEvent();

    [Header("UI")]
    public GameObject Arms;
    public GameObject Gun;
    public GameObject GunPosition;
    public GameObject ZoomImg;
    public GameObject NoZoomImg1;
    public GameObject NoZoomImg2;

    [Header("Weapon Setting")]
    public WeaponSetting weaponSetting;

    [Header("Fire Effect")]
    [SerializeField]
    private GameObject GunEffect;

    [Header("Sound")]
    [SerializeField]
    private AudioClip ShootSound;
    [SerializeField]
    private AudioClip StartGunSound;
    [SerializeField]
    private AudioClip ReloadSound;
    [SerializeField]
    private AudioClip ZoomSound;

    private float lastAttackTime = 0; // 마지막 발사시간 체크용
    private bool isReload = false; // 재장전 여부

    private PlayerAnimator playerAnimator;
    private AudioSource audioSource;
    private CameraMove cameraMove;
    // 외부에서 필요한 정보를 열람하기 위해 정의한 Get Property's
    public WeaponName WeaponName => weaponSetting.weaponName;

    // raycast로 적을 피격할 것이기 때문에 Layer생성
    public LayerMask RobotLayer; // 로봇의 레이어
    public LayerMask BossLayer;  // 보스의 레이어

    private void Start()
    {
        // 총 첫 장전 애니메이션 플레이
        playerAnimator.Play("TakeOut", -1, 0);
    }

    private void Awake()
    {
        playerAnimator = GetComponent<PlayerAnimator>();
        cameraMove     = GetComponent<CameraMove>();
        audioSource    = GameObject.Find("arms_assault_rifle_01").GetComponent<AudioSource>(); 
        // 시작 시 탄약의 수를 최대로 설정
        weaponSetting.currentAmmo = weaponSetting.maxAmmo;
        audioSource.clip = StartGunSound;
        audioSource.Play();
    }

    private void OnEnable()
    {
        // 총 이팩트 비활성화
        GunEffect.SetActive(false);

        // 무기가 활성화될 때 해당 무기의 탄 수를 갱신한다.
        onAmmoEvent.Invoke(weaponSetting.currentAmmo, weaponSetting.maxAmmo);
    }

    void Update()
    {
        // 어빌리티를 선택할 시에 게임의 시간이 0이 되므로 총을 쏘지 못하게 함
        if (Time.timeScale > 0)
        {
            PlayerShoot();
        }
    }

    // 무기 첫 시작
    public void StartWeaponAction(int type = 0)
    {
        // 재장전 중일때는 무기 액션 금지
        if (isReload == true) return;

        if (type == 0)
        {
            if ( weaponSetting.isAutomaticAttack == true )
            {
                StartCoroutine("AttackLoop");
            }
            else
            {
                OnAttack();
            } 
        }
    }

    // 무기 정지
    public void StopWeaponAction(int type = 0)
    {
        if (type == 0)
        {
            StopCoroutine("AttackLoop");
        }
    }

    // 재장전 시작
    public void StartReload()
    {
        // 현재 재장전 중이면 재장전 불가능하게 함
        if (isReload == true || weaponSetting.currentAmmo == weaponSetting.maxAmmo) return;

        // 무기 액션 도중에 'R'키를 눌러 재장전을 시도하면 무기 액션 종료 후 재장전
        StopWeaponAction();

        StartCoroutine("OnReload");
    }

    private IEnumerator AttackLoop()
    {
        while (true)
        {
            OnAttack();
            yield return null;
        }
    }

    public void OnAttack()
    {
        // 현재 시간에 마지막으로 발사한 시간을 비교하여 발사 딜레이 비교함
        if (Time.time - lastAttackTime > weaponSetting.attackRate)
        {
            if ( playerAnimator.MoveSpeed > 0.5f)
            {
                return;
            }

            lastAttackTime = Time.time;

            // 현재 탄약의 개수가 0이하라면
            if ( weaponSetting.currentAmmo <= 0)
            {
                return;
            }

            // 현재 탄약의 개수를 줄임
            weaponSetting.currentAmmo--;
            onAmmoEvent.Invoke(weaponSetting.currentAmmo, weaponSetting.maxAmmo);

            // 총 발사 애니메이션 출력
            if (Gun.activeSelf)
            {
                playerAnimator.Play("Fire", -1, 0);
            }

            // 총 이팩트 생성 코루틴
            StartCoroutine("GunFireEffect");
            audioSource.volume = 0.05f;
            audioSource.Stop();
            audioSource.clip = ShootSound;
            audioSource.Play();

            // Raycast로 탄 발사 및 충돌 처리
            RaycastHit hit;
            if (Physics.Raycast(transform.position, transform.forward, out hit, weaponSetting.attackDistance, RobotLayer))
            {
                hit.collider.GetComponent<RoBotStatus>().minHp -= weaponSetting.AttackDamage;
            }
            if (Physics.Raycast(transform.position, transform.forward, out hit, weaponSetting.attackDistance, BossLayer))
            {
                hit.collider.GetComponent<BossStatus>().CurrentHp -= weaponSetting.AttackDamage;
            }
            // 줌이 아닐 때만 반동 구현
            if (!Input.GetMouseButton(1))
            {
                float x = Random.Range(-1f, 1f);
                float y = -0.5f;
                GameObject.Find("Player").GetComponent<CameraMove>().eulerAngleX += y;
                GameObject.Find("Player").GetComponent<CameraMove>().eulerAngleY += x;
            }
        }
    }

    // 탄 발사시 임팩트
    private IEnumerator GunFireEffect()
    {
        GunEffect.SetActive(true);
        yield return new WaitForSeconds(weaponSetting.attackRate * 0.3f);
        GunEffect.SetActive(false);
    }

    // 플레이어의 줌 상태 설정
    private void PlayerShoot()
    {
        // 우클릭을 누르고서 애니메이션이 reload_out_of_ammo@assault_rifle_01(재장전)이 아니라면
        if (Input.GetMouseButtonDown(1) && !playerAnimator.CurrentAnimationIs("reload_out_of_ammo@assault_rifle_01"))
        {
            audioSource.Stop();
            audioSource.volume = 0.5f;
            audioSource.clip = ZoomSound;
            audioSource.Play();
        }

        // 우클릭을 누르는 상태에서 애니메이션이 reload_out_of_ammo@assault_rifle_01(재장전)이 아니라면
        if (Input.GetMouseButton(1) && !playerAnimator.CurrentAnimationIs("reload_out_of_ammo@assault_rifle_01"))
        {
            StopWeaponAction();
            weaponSetting.isAutomaticAttack = false;
            // 줌모드가 되고 총을 가운데로 위치로 이동
            Gun.transform.position = Vector3.MoveTowards(Gun.transform.position, GunPosition.transform.position, Time.deltaTime * 0.5f);
        }
        // 우클릭을 뗀다면
        else if (Input.GetMouseButtonUp(1))
        {
            // 총의 오토 발사를 할 수 있게 하고 총을 원래 위치로 이동
            weaponSetting.isAutomaticAttack = true;
            Gun.transform.position = GameObject.Find("Player").transform.position;
        }

        // 총 위치가 가운데 도달시 에임 변경 후 카메라 확대
        if (Gun.transform.position == GunPosition.transform.position)
        {
            NoZoomImg1.SetActive(true);
            NoZoomImg2.SetActive(true);
            ZoomImg.SetActive(false);
            GameObject.Find("Main Camera").GetComponent<Camera>().fieldOfView = Mathf.Lerp(GameObject.Find("Main Camera").GetComponent<Camera>().fieldOfView, 20, Time.deltaTime * 5);
            Arms.GetComponent<SkinnedMeshRenderer>().enabled = false;
        }
        else
        {
            // 아닐시 원상태로 복구
            NoZoomImg1.SetActive(false);
            NoZoomImg2.SetActive(false);
            ZoomImg.SetActive(true);
            GameObject.Find("Main Camera").GetComponent<Camera>().fieldOfView = Mathf.Lerp(GameObject.Find("Main Camera").GetComponent<Camera>().fieldOfView, 60, Time.deltaTime * 5);
            Arms.GetComponent<SkinnedMeshRenderer>().enabled = true;
            weaponSetting.isAutomaticAttack = true;
        }
    }

    private IEnumerator OnReload()
    {
        // 재장전 애니메이션 재생
        playerAnimator.OnReload();
        audioSource.volume = 0.5f;
        audioSource.Stop();
        audioSource.clip = ReloadSound;
        audioSource.Play();

        while (true)
        {
            if (playerAnimator.CurrentAnimationIs("MoveMent"))
            {
                // 현재 탄 수를 최대로 설정하고, 바뀐 탄 수 정보를 Text UI에 업데이트
                weaponSetting.currentAmmo = weaponSetting.maxAmmo;
                onAmmoEvent.Invoke(weaponSetting.currentAmmo, weaponSetting.maxAmmo);

                yield break;
            }

            yield return null;
        }
    }
}

```
