```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.AI;

public class RobotFreeAnim : MonoBehaviour 
{
	private RoBotStatus Status;
	private NavMeshAgent Agent;
	private AudioSource audioSource;

	Animator anim;

	[SerializeField]
	private GameObject Player;

	public int RobotHp;
	private CapsuleCollider capsuleCollider;

	private float WaitTime; // 생성되고 대기 시간
	private float DelayTime; // 공격받은 후 대기 시간
	public bool DelayCheck = false; // 공격 받았는지 여부

	void Awake()
	{
		audioSource = GetComponent<AudioSource>();
		Status = GetComponent<RoBotStatus>();
		Agent = GetComponent<NavMeshAgent>();
		anim = gameObject.GetComponent<Animator>();
		capsuleCollider = GetComponent<CapsuleCollider>();
		Player = GameObject.Find("Player");
		WaitTime = 3.5f;
		DelayTime = 1.5f;
		RobotHp = 10;
	}

	void Update()
	{
		RoBotAnimation();

		// 처음 생성 후 대기시간을 갖고 움직이게 구현
		if (WaitTime < 0)
        {
            Agent.SetDestination(Player.transform.position);
		}
		else if (WaitTime > 0)
		{
            WaitTime -= Time.deltaTime;
        }

		// 플레이어에게 공격을 받으면 대기시간을 갖고 더 빠르게 움직이게 구현
		if (DelayTime < 0 && DelayCheck)
        {
			if (audioSource.isPlaying)
			{
				audioSource.Stop();
				audioSource.clip = Status.MoveSound;
				audioSource.volume = 0.65f;
				audioSource.Play();
			}
			Agent.speed = 6.0f;
			DelayCheck = false;
		}
		else if (DelayTime > 0 && DelayCheck)
        {
			DelayTime -= Time.deltaTime;
		}
	}

	void RoBotAnimation()
	{
		// 현재 체력이 최대 체력보다 낮아지면 애니메이션 변경
		if (Status.minHp < Status.maxHp)
		{
			if (!anim.GetBool("Roll_Anim"))
			{
				DelayCheck = true;
				capsuleCollider.center = new Vector3(0, 0.05f, 0);
				anim.SetBool("Walk_Anim", false);
				anim.SetBool("Roll_Anim", true);
			}
		}
		else
        {
			anim.SetBool("Walk_Anim", true);
		}
	}
}
```
