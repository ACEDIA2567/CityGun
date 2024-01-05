```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class BossStatus : MonoBehaviour
{
    private GameObject Player;
    private GameObject EnemySpawner;
    public Vector3 PlayerY;
    private AudioSource audioSource;

    [SerializeField]
    private Slider BossHpBar;              // 보스의 체력 UI
    [SerializeField]
    private GameObject BossName;           // 보스의 이름

    [Header("Status")]
    private float MaxHp = 1000.0f;         // 보스 최대 체력
    public float CurrentHp;                // 보스 현재 체력
    public float AttackTime = 10.0f;       // 보스 공격 시간
    public bool AttackStart = false;       // 보스 공격 시작
    public int AttackPattern;              // 보스 현재 패턴
    public bool PlayerAttack = false;
    [SerializeField]
    private ParticleSystem DesParticle;    // 보스의 죽음 파티클
    [SerializeField]
    private AudioClip DeathSound;          // 보스의 죽음 사운드

    private void Start()
    {
        Player = GameObject.Find("Player");
        EnemySpawner = GameObject.Find("EnemySpawner");
        audioSource = GetComponent<AudioSource>();
        CurrentHp = MaxHp; // 최대 체력을 현재 체력으로 초기화
    }

    void Update()
    {
        if (AttackStart)
        {
            BossHpBar.gameObject.SetActive(true);
            BossName.SetActive(true);
        }

        BossLook();
        BossDes();
        BossHpUpdate();
    }

    void BossHpUpdate()
    {
        // slider의 value값은 0과 1사이므로 Mathf.Clamp01를 사용함
        BossHpBar.value = Mathf.Clamp01(CurrentHp / MaxHp);
    }

    void BossDes()
    {
        // 보스가 죽었을때
        if (CurrentHp < 0)
        {
            audioSource.Stop();
            DesParticle.transform.position = transform.position + new Vector3(0, 30.0f, 0);
            DesParticle.transform.rotation = transform.rotation;
            DesParticle.Play();
            EnemySpawner.GetComponent<ScoreUI>().BoosDeath = true;
            if (!audioSource.isPlaying)
            {
                audioSource.volume = 0.5f;
                AudioSource.PlayClipAtPoint(DeathSound, transform.position);
                Destroy(this.gameObject);
            }
        }
    }

    void BossLook()
    {
        transform.LookAt(Player.transform.position - PlayerY);  // 플레이어를 쳐다본다.
        TimeRe();
    }

    void TimeRe() // 공격 딜레이
    {
        if (AttackTime > 0)
        {
            AttackTime -= Time.deltaTime;
        }
    }
}
```
