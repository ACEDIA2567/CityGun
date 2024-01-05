```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class Invader : MonoBehaviour
{
    private RoBotStatus Status;
    private NavMeshAgent Agent;
    private Vector3 destination;

    [Header("Attack")]
    [SerializeField]
    private GameObject GunPosition; // 탄 생성 위치
    public  float      AttackTime;  // 공격 딜레이
    [SerializeField]
    private GameObject bullet;      // 탄 오브젝트
    private Vector3    Height = new Vector3(0, 1, 0);

    private AudioSource audioSource;

    void Start()
    {
        Status = GetComponent<RoBotStatus>();
        Agent = GetComponent<NavMeshAgent>();
        audioSource = GetComponent<AudioSource>();
        destination = new Vector3(Status.player.transform.position.x, 20, Status.player.transform.position.z);
        AttackTime = Status.attackTime;
    }

    void Update()
    {
        PlayerLook();
        Move();
    }

    private void Move()
    {
        UpdateDestination(Status.player.transform.position.x, Status.player.transform.position.z);
    }

    private void Attack()
    {
        if (AttackTime > 0)
        {
            AttackTime -= Time.deltaTime;
        }
        else
        {
            audioSource.Stop();
            audioSource.clip = Status.AttackSound;
            audioSource.Play();
            // 탄 생성 (회전값은 해당 적 오브젝트가 플레이어를 쳐다보기 때문에 따로 처리를 하지 않음
            GameObject a = Instantiate(bullet, GunPosition.transform.position, transform.rotation);
            AttackTime = Status.attackTime;
        }
    }

    // 플레이어를 쫓을때 Y축만 제거하여 추적하게 함
    void UpdateDestination(float newX, float newZ)
    {
        destination.Set(newX, destination.y, newZ);
        Agent.SetDestination(destination);
    }

    private void PlayerLook()
    {
        if (Vector3.Distance(transform.position, Status.player.transform.position) < 15)
        {
            transform.LookAt(Status.player.transform.position - Height);
            // 공격시작
            Attack();
        }
    }
}

```
