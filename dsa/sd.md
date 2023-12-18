```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Status : MonoBehaviour
{
    [Header("Walk, Run Speed")]
    [SerializeField]
    private float walkSpeed;
    [SerializeField]
    private float runSpeed;

    [Header("SpUse")]
    public float AbilitySpeed = 0;
    public float AbilitySp = 0;

    [Header("HP, SP")]
    public float MinSp = 0f;     // 최소 기력
    public float MaxSp = 100.0f; // 최대 기력
    public float MinHp = 0;      // 최소 체력
    public float MaxHp = 3.0f;   // 최대 체력
    public GameObject[] HpImage;
    public float SpRecovery = 2;
    public float HpRecovery = 0.1f;

    [Header("Level")]
    [SerializeField]
    private float Level = 1;                   // 플레이어 레벨
    public float Experience = 0;               // 현재 경험치
    [SerializeField]
    private float ExperienceToLevel = 1;       // 최대 경험치

    public bool ActiveCheck = true;

    public float WalkSpeed => walkSpeed + AbilitySpeed;
    public float RunSpeed => runSpeed + AbilitySpeed;

    [SerializeField]
    private GameObject Spbar;     // 기력바 이미지
    [SerializeField]
    private Image Exbar;          // 경험치바 이미지
    [SerializeField]
    private GameObject AbilityUI; // 플레이어가 레벨 업을 할 시에 활성화 시킬 오브젝트
    [SerializeField]
    private Image HitImage;       // 플레이어가 피격 당할 시의 피격 이미지
    [SerializeField]
    public bool HitCheck = false; // 플레이어의 피격 여부

    private void Start()
    {
        MinSp = MaxSp; // 게임 시작 시 현재 Sp를 최대 Sp로
        MinHp = MaxHp; // 게임 시작 시 현재 Hp를 최대 Hp로
    }

    private void Update()
    {
        // ActiveCheck가 false가 되면 Invoke로 5초후 ActiveCheck값이 true가 됨
        if (ActiveCheck == false) { Invoke("ActiveTrue", 5); }

        ExbarUpdate(Experience, ExperienceToLevel); // 경험치바 UI의 업데이트
        HpbarUpdate();                              // 체력바 UI의 업데이트

        if (Level < 15) // 플레이어의 레벨이 15이하일 때만 사용하게 함
        {
            LevelUp();
        }

        // 기력 상승
        if (MinSp < MaxSp)
        {
            MinSp += SpRecovery * Time.deltaTime;
        }
        // 체력 상승
        if (MinHp < MaxHp)
        {
            MinHp += HpRecovery * Time.deltaTime;
        }

        // 외부 오브젝트에서 사용하는 스크립트인 UIBarScript에서 UpdateValue의 함수를 사용함
        Spbar.GetComponent<UIBarScript>().UpdateValue(MinSp, MaxSp);

        if (HitCheck) // 플레이어가 피격을 당했을 때의 조건
        {
            StartCoroutine(AttackHit()); // AttackHit라는 코루틴을 시작함
        }
    }

    private void ActiveTrue() // Invoke로 인하여 5초뒤 실행할 함수
    {
        ActiveCheck = true;
    }

    private void LevelUp() // 레벨 업 관련 함수
    {
        if (Experience >= ExperienceToLevel) // 현재 경험치가 경험치량을 넘어가게 되면
        {
            AbilityUI.SetActive(true); // 레벨 업관련 오브젝트 활성화
            Cursor.visible = true;     // 마우스 커서 보이게 설정
            Cursor.lockState = CursorLockMode.None; // 마우스 위치를 고정해제
            Time.timeScale = 0;
            Level++;                         // 레벨 1 증가
            Experience -= ExperienceToLevel; // 경험치량을 최대 경험치로 빼서 초과한 경험치 들어오게 구현
            ExperienceToLevel += 5;          // 최대 경험치량을 5 증가시킴
        }
    }

    private void ExbarUpdate(float CurrentExp, float MaxExp)
    {
        float normalizedExp = Mathf.InverseLerp(0, MaxExp, CurrentExp);
        Exbar.fillAmount = normalizedExp;
    }

    private void HpbarUpdate()
    {
        int ReduceHp = (int)MinHp;
        float valueHp = MinHp - ReduceHp;
        // 최대 체력
        switch (MaxHp)
        {
            case 4:
                HpImage[3].transform.GetChild(0).gameObject.SetActive(true);
                HpImage[3].gameObject.SetActive(true);
                break;
            case 5:
                HpImage[4].transform.GetChild(0).gameObject.SetActive(true);
                HpImage[4].gameObject.SetActive(true);
                break;
        }

        switch ((int)MinHp)
        {
            case 0:
                HpImage[1].transform.GetChild(0).GetComponent<Image>().fillAmount = 0;
                HpImage[0].transform.GetChild(0).GetComponent<Image>().fillAmount = valueHp;
                break;
            case 1:
                HpImage[2].transform.GetChild(0).GetComponent<Image>().fillAmount = 0;
                HpImage[1].transform.GetChild(0).GetComponent<Image>().fillAmount = valueHp;
                HpImage[0].transform.GetChild(0).GetComponent<Image>().fillAmount = 1;
                break;
            case 2:
                HpImage[3].transform.GetChild(0).GetComponent<Image>().fillAmount = 0;
                HpImage[2].transform.GetChild(0).GetComponent<Image>().fillAmount = valueHp;
                HpImage[1].transform.GetChild(0).GetComponent<Image>().fillAmount = 1;
                break;
            case 3:
                HpImage[4].transform.GetChild(0).GetComponent<Image>().fillAmount = 0;
                HpImage[3].transform.GetChild(0).GetComponent<Image>().fillAmount = valueHp;
                HpImage[2].transform.GetChild(0).GetComponent<Image>().fillAmount = 1;
                break;
            case 4:
                HpImage[4].transform.GetChild(0).GetComponent<Image>().fillAmount = valueHp;
                HpImage[3].transform.GetChild(0).GetComponent<Image>().fillAmount = 1;
                break;
            case 5:
                HpImage[4].transform.GetChild(0).GetComponent<Image>().fillAmount = 1;
                break;
        }
    }

    public IEnumerator AttackHit()
    {
        HitImage.color = new Color(1, 1, 1, 0.8f);
        yield return new WaitForSeconds(0.1f);
        HitImage.color = Color.clear;
        HitCheck = false;
    }
}



```
![볼봇 drawio](https://github.com/ACEDIA2567/CityGun/assets/101154683/f154a522-b019-44a5-a194-aa291e05cb95)
