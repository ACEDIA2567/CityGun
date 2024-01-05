```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class BlueVariant : MonoBehaviour
{
    Animator anim;
    private NavMeshAgent Agent;
    private RoBotStatus Status;
    private bool BoxCheck = false;    // 박스를 설치했는지의 여부
    public bool AttackCheck = false;  // 공격 여부

    [SerializeField]
    private GameObject Box;           // 봇이 갖고 있는 박스 오브젝트 (스카우트봇이 들어가 있는 박스)
    [SerializeField]
    private GameObject ScoutRoBot;    // Scout봇의 오브젝트
    [SerializeField]                  
    private GameObject SpawnPosition; // Scout봇의 스폰 위치
    [SerializeField]                  
    private GameObject AttackRange;   // Spawn봇의 공격 사거리

    private AudioSource audioSource;

    void Start()
    {
        AttackRange.SetActive(false);
        anim = gameObject.GetComponent<Animator>();
        Agent = GetComponent<NavMeshAgent>();
        Status = GetComponent<RoBotStatus>();
        audioSource = GetComponent<AudioSource>();
    }

    public void Update()
    {
        RoBotAnimation();
    }

    void RoBotAnimation()
    {
        // 플레이어를 쫓게함
        Agent.SetDestination(Status.player.transform.position); 
        // 플레이어와의 거리가 1.5이하이면서 현재 애니메이션이 BoxDown이 아니라면
        if (Agent.remainingDistance <= 1.5 && !anim.GetCurrentAnimatorStateInfo(0).IsName("BoxDown"))
        {
            // 걷는 애니메이션 중단 후 공격 애니메이션 작동
            anim.SetBool("Attack", true);
            anim.SetBool("Walking", false);
            Agent.speed = 0;
            audioSource.Stop();
        }

        // 걷는 애니메이션일 때 걷는 사운드 작동
        if (anim.GetCurrentAnimatorStateInfo(0).IsName("Walk"))
        {
            if (!audioSource.isPlaying)
            {
                audioSource.Play();
            }
        }

        // 현재 애니메이션이 Attack일 때
        if (anim.GetCurrentAnimatorStateInfo(0).IsName("Attack"))
        {
            if (anim.GetCurrentAnimatorStateInfo(0).normalizedTime >= 1.0f)
            {
                anim.SetBool("Attack", false);
                anim.SetBool("Walking", true);
                if (Agent.remainingDistance > 1.5f)
                {
                    Agent.speed = 3;
                    audioSource.Play();
                }
            }
            // 애니메이션시간이 0.7이상 0.8이하라면의 조건
            if (anim.GetCurrentAnimatorStateInfo(0).normalizedTime >= 0.7f && anim.GetCurrentAnimatorStateInfo(0).normalizedTime <= 0.8f)
            {
                audioSource.PlayOneShot(Status.AttackSound);
                if (!AttackCheck)
                {
                    AttackRange.SetActive(true);
                    AttackCheck = true;
                }
            }
            else
            {
                AttackCheck = false;
            }
        }

        // 현재 애니메이션이 BoxDown일 때
        if (anim.GetCurrentAnimatorStateInfo(0).IsName("BoxDown"))
        {
            audioSource.Stop();
            if (anim.GetCurrentAnimatorStateInfo(0).normalizedTime >= 0.95f)
            {
                anim.SetBool("BoxDown", false);
                anim.SetBool("Walking", true);
                Box.SetActive(false);
                Agent.speed = 3;
            }
            if (anim.GetCurrentAnimatorStateInfo(0).normalizedTime >= 0.8f)
            {
                if (Box.activeSelf)
                {
                    GameObject a = Instantiate(ScoutRoBot, SpawnPosition.transform);
                    a.SetActive(true);
                    a.transform.SetParent(null);
                }
                Box.SetActive(false);
            }
        }

        // 체력이 5가 되면 박스를 내려놓는다.
        if (Status.minHp < 5 && BoxCheck == false)
        {
            anim.SetBool("Attack", false);
            anim.SetBool("Walking", false);
            anim.SetBool("BoxDown", true);
            BoxCheck = true;
            Agent.speed = 0;
        }
    }

    public void Attack()
    {
        Status.Attack(1.0f);
    }
}

```
