```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BossAttack : MonoBehaviour
{
    private BossStatus bossStatus;       // 보스의 정보
    
    [Header("BossObject")]
    [SerializeField]
    private GameObject       EmpObjectRange; // 보스의 EMP범위
    public  GameObject[]     RocketPoss;     // 보스의 로켓 4군데의 위치
    public  ParticleSystem[] Rockets;        // 보스의 미사일 오브젝트
    public  GameObject[]     Scout_Robot;    // 보스의 스카우트들
    public  Transform        ScoutPos;       // 스카우트들의 생성되는 위치

    [Header("Sound")]
    [SerializeField]
    private AudioClip Attack1Sound; // 공격 사운드1
    [SerializeField]
    private AudioClip Attack2Sound; // 공격 사운드2
    [SerializeField]
    private AudioClip Attack3Sound; // 공격 사운드3

    private AudioSource audioSource;
    private Status      PlayerStatus;
    private GameObject  Player;

    void Start()
    {
        PlayerStatus = GameObject.Find("Player").GetComponent<Status>();
        Player       = GameObject.Find("Player");
        bossStatus   = GetComponent<BossStatus>();
        audioSource  = GetComponent<AudioSource>();
    }

    void Update()
    {
        if (bossStatus.AttackStart) // 보스의 공격이 가능하면 패턴을 설정하게 함
        {
            BossPattern();
        }
    }

    private void BossPattern()
    {
        if (bossStatus.AttackTime < 0)
        {
            bossStatus.AttackPattern = Random.Range(0, 3);   // 보스의 3가지 패턴 중 1개 선언
            bossStatus.PlayerAttack = false;                 // 패턴을 정했기 때문에 공격 선택을 멈춤
            switch (bossStatus.AttackPattern)
            {
                case 0:
                    Attack1();
                    break;

                case 1:
                    Attack2();
                    break;

                case 2:
                    Attack3();
                    break;
            }
        }
    }

    void Attack1() // 자폿 봇 스폰하여 공격
    {
        bossStatus.AttackTime = 30;
        StartCoroutine("ScoutAttackStart");
    }

    void Attack2() // 로켓 공격
    {
        bossStatus.AttackTime = 30;
        StartCoroutine("RocketStart");
    }

    void Attack3() // EMP 폭발로 플레이어 기능 상실 공격
    {
        float x = .5f;
        float y = .5f;
        float z = .5f;
        // 보스의 몸에 EMP를 터트림
        // 플레이어가 공격을 하지 못하게 됨
        bossStatus.AttackTime = 20;
        StartCoroutine(EMPStart(x, y, z));  // EMP 코루틴 발동
    }

    IEnumerator ScoutAttackStart()
    {
        foreach (var obj in Scout_Robot)
        {
            obj.transform.position = ScoutPos.position;
            obj.SetActive(true);
        }
        audioSource.Stop();               // 다른 공격 사운드 정지
        audioSource.clip = Attack1Sound;  
        audioSource.Play();               // 스카우트 스폰 사운드 시작
        yield return new WaitForSeconds(10.0f);
        bossStatus.PlayerAttack = true; // 10초후 스카우트 플레이어를 향해 돌진
    }

    IEnumerator RocketStart()
    {
        if (bossStatus.AttackPattern == 1)
        {
            // 보스의 로켓 발사대 4군데에서 차례대로 발사하게 함
            for (int i = 0; i < RocketPoss.Length; i++)
            {
                audioSource.Stop();              // 다른 공격 사운드 정지
                audioSource.clip = Attack2Sound; 
                audioSource.Play();              // 로켓 발사 사운드 시작
                RocketPoss[i].transform.LookAt(Player.transform);
                Rockets[i].transform.rotation = RocketPoss[i].transform.rotation;
                Rockets[i].transform.position = RocketPoss[i].transform.position;
                Rockets[i].Play();

                // 5초마다 다른 위치의 로켓으로 쏘게 함
                yield return new WaitForSeconds(5.0f);
            }
        }
    }

    IEnumerator EMPStart(float x, float y, float z)
    {
        EmpObjectRange.SetActive(true);  // EMP오브젝트 활성화
        audioSource.Stop();              // 다른 공격 사운드 정지
        audioSource.clip = Attack3Sound;
        audioSource.Play();              // EMP 공격 사운드 시작
        while (true)
        {
            float dleTime = Time.deltaTime; // 모든 Scale의 값을 늘려줌
            x += dleTime;
            y += dleTime;
            z += dleTime;
            EmpObjectRange.transform.localScale = new Vector3(x, y, z);    // EMP의 스케일 값을 계속 상승시킴
            if (EmpObjectRange.transform.localScale.x > 5)                 // EMP의 스케일 값이 4가 넘어가면 코루틴 중단
            {
                PlayerStatus.ActiveCheck = false;                          // 플레이어의 공격 5초 동안 멈춤
                break;
            }
            yield return null;
        }
        EmpObjectRange.SetActive(false);                                   // EMP 오브젝트 비활성화
    }
}
```
