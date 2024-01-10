```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.SocialPlatforms.Impl;

public class RoBotStatus : MonoBehaviour
{
    [Header("HP")]
    [SerializeField]
    private float MinHp;
    public float minHp
    {
        get
        {
            return MinHp;
        }
        set
        {
            MinHp = value;
        }
    }
    [SerializeField]
    private float MaxHp;
    public float maxHp
    {
        get
        {
            return MaxHp;
        }
        set
        {
            MaxHp = value;
        }
    }

    [Header("Info")]
    [SerializeField]
    private GameObject Player;
    public GameObject player
    {
        get
        {
            return Player;
        }
    }
    [SerializeField]
    protected float EnemyScoreAdd;   // 몬스터의 점수량
    [SerializeField]
    private GameObject EnemySpawner; // 몬스터 스포너 오브젝트
    [SerializeField]
    private float AttackTime;        // 공격 딜레이 시간
    public float attackTime          // 몬스터의 공격 딜레이 시간
    {
        get { return AttackTime; }
    }
    [SerializeField]
    ParticleSystem particleObject; // 로봇의 피가 0일 때 죽는 파티클
    [SerializeField]
    private float Exp;
    public float exp
    {
        get { return Exp; }
        set { Exp = value; }
    }

    [Header("Sound")]
    private AudioSource audioSource;
    public AudioClip MoveSound;
    public AudioClip AttackSound;
    public AudioClip DeadSound;

    void OnEnable()
    {
        audioSource = GetComponent<AudioSource>();
        MinHp = MaxHp;
    }

    public void Update()
    {
        DesEnemy();
    }

    private void DesEnemy()
    {
        // 오브젝트의 MinHp가 0이하라면
        if (MinHp <= 0)
        {
            player.GetComponent<Status>().Experience += Exp;                    // 플레이어의 Experience를 현재 로봇의 Exp만큼 더해진다.
            EnemySpawner.GetComponent<ScoreUI>().EnemyScore += EnemyScoreAdd;   // 플레이어의 EnemyScore 현재 로봇의 EnemyScoreAdd만큼 더해진다.
            particleObject.transform.position = transform.position;
            particleObject.Play();
            EnemySpawner.GetComponent<Spawner>().EnemyCount -= 1.0f;            // 몬스터 현재 총 개체수를 줄임
            if (this.gameObject.tag == "RoBot1") // 태그가 RoBot1인 경우는 비활성화만 시킴
            {
                this.gameObject.SetActive(false);
            }
            else
            {
                // 죽으면 사운드클립이 사라지므로 AudioSource.PlayClipAtPoint를 사용하여 죽어도 사운드가 들리게 함
                AudioSource.PlayClipAtPoint(DeadSound, transform.position);
                // Scout봇이 죽는다면 Spawner에서 ScoutUpCheck를 다시 false 시킴
                if (this.gameObject.name == "Robot_Scout(Clone)")
                {
                    EnemySpawner.GetComponent<Spawner>().ScoutUpCheck = false;
                }
                Destroy(gameObject);
            }
        }
    }

    // 먼저 충돌 처리
    private void OnTriggerEnter(Collider other)
    {
        // 충돌체의 태그가 Player이면서 현재 오브젝트의 이름이 robotSphere(Clone)이라면
        if (other.gameObject.tag == "Player" && transform.gameObject.name == "robotSphere(Clone)")
        {
            Attack(1.0f);
            Destroy(this.gameObject);
        }
        // 충돌체의 태그가 Player이면서 현재 오브젝트의 태그가 RoBot1이라면
        if (other.gameObject.tag == "Player" && this.gameObject.tag == "RoBot1")
        {
            Attack(0.5f);
        }

        // 충돌체의 Layer값이 Wall이면서 현재 오브젝트의 태그가 RoBot1이라면
        if (other.gameObject.layer == LayerMask.NameToLayer("Wall") && this.gameObject.tag =="RoBot1")
        {
            particleObject.transform.position = transform.position;
            particleObject.Play();
            this.gameObject.SetActive(false);
        }
    }

    // 지속적인 충돌 처리
    private void OnTriggerStay(Collider other)
    {
        // 충돌체의 태그가 Player이면서 현재 오브젝트의 태그가 RoBot1이라면
        if (other.gameObject.tag == "Player" && this.gameObject.tag == "RoBot1")
        {
            Attack(0.5f);
        }

        // 충돌체의 Layer값이 Wall이면서 현재 오브젝트의 태그가 RoBot1이라면
        if (other.gameObject.layer == LayerMask.NameToLayer("Wall") && this.gameObject.tag == "RoBot1")
        {
            particleObject.transform.position = transform.position;
            particleObject.Play();
            this.gameObject.SetActive(false);
        }
    }

    // 플레이어를 공격할 때 사용하는 함수
    public void Attack(float Damage)
    {
        // 플레이어의 체력이 공격량보다 적다면
        if (player.GetComponent<Status>().MinHp < Damage)
        {
            EnemySpawner.GetComponent<Spawner>().enabled = false;
            DontDestroyOnLoad(EnemySpawner);
            Cursor.visible = true; // 마우스 커서 보이게 설정
            Cursor.lockState = CursorLockMode.None; // 마우스 위치를 고정 해제
            SceneManager.LoadScene("EndScene");
            if (Damage == 0.5f)
            {
                this.gameObject.SetActive(false);
            }
            else
            {
                Destroy(this.gameObject);
            }
        }
        else // 플레이어의 체력이 공격량보다 많다면
        {
            player.GetComponent<Status>().MinHp -= Damage;
            player.GetComponent<Status>().HitCheck = true;
        }
    }
}


```
