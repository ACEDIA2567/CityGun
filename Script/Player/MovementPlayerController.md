```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[RequireComponent(typeof(CharacterController))] // CharacterController를 해당 오브젝트에 자동으로 컴포넌트 시킴
public class MovementPlayerController : MonoBehaviour
{
    [Header("Move")]
    [SerializeField]
    private float  moveSpeed;              // 플레이어의 이동속도
    public Vector3 MoveForce;              // 이동 힘
    public float   RunDelayTime;           // 달리다가 기력이 없을 때 다시 뛸 수 있는 딜레이 시간

    [Header("Jump")]
    [SerializeField]
    private float  JumpForce;              // 점프 크기
    [SerializeField]
    public float   gravity;                // 중력 수치

    [Header("Dash")]
    [SerializeField]
    public float   dashDistance   = 5.0f;  // 대쉬 거리
    [SerializeField]
    public float   dashDuration   = 0.2f;  // 대쉬 지속 시간
    public float   DashDelayTime  = 0.2f;  // 대쉬 입력 대기 시간
    public Vector3 dashDirection;          // 대쉬 방향
    public bool    isDashing      = false; // 대쉬 중인지 여부
    public float   dashTimer      = 0.0f;  // 대쉬 시간 측정
    public bool    canStartDash   = false; // 대쉬 시작 가능한지 여부

    public float WallDashTime  = 1.0f;     // 벽을 달리는 시간
    public bool WallCheck      = false;    // 벽에 붙었는지 여부
    public float MoveSpeed
    {
        set => moveSpeed = Mathf.Max(0, value);
        get => moveSpeed;
    }

    private CharacterController characterController; // 플레이어 이동 제어 스크립트

    void Start()
    {
        characterController = GetComponent<CharacterController>();
    }

    void Update()
    {
        if (!characterController.isGrounded) // 플레이어가 땅에 서 있지 않을 때
        {
            WallCheck = false;               // 플레이어는 벽에 붙지 않게 함
            if (Input.GetKey(KeyCode.Space)) // 스페이스를 누르고 있으면 
            {
                MoveForce.y += gravity * Time.deltaTime;         // 누르고 있으면 높은 점프
            }
            else
            {
                MoveForce.y += gravity * Time.deltaTime * 2.0f;  // 그렇지 않으면 낮은 점프
            }
        }
        else // 플레이어가 땅에  
        {
            WallCheck = true;  
            WallDashTime = 1.0f;
        }

        // 1초당 MoveForce만큼 속력으로 이동
        characterController.Move(MoveForce * Time.deltaTime);
    }

    public void MoveTo(Vector3 direction)
    {
        // 이동 방향 = 캐릭터의 회전 값 * 방향 값
        direction = transform.rotation * new Vector3(direction.x, 0, direction.z);

        // 이동 힘 = 이동방향 * 속도
        MoveForce = new Vector3(direction.x * MoveSpeed, MoveForce.y, direction.z * MoveSpeed);
    }

    public void Jump() // 플레이어 점프 키를 눌렀을 때의 함수
    {
        if (characterController.isGrounded) // 플레이어가 땅에 서 있다면
        {
            MoveForce.y = JumpForce;        // 플레이어의 컴포넌트의 CharacterController의 함수 Move의 y값을 증가시켜 점프를 하게함
        }
    }

    public void WallDash() // 플레이어가 벽에 붙어있을 때 점프 키를 누르면 발생하는 함수
    {
        if (!characterController.isGrounded) // 플레이어가 바닥에서 벽 달리기를 할 수 없게 조건문을 달음
        {
            if (WallDashTime > 0) // 벽 달리는 시간이 0이 되면 할 수 없게 조건문 달음
            {
                MoveForce.y = 0;  // 달리는 시간이 0초과라면 중력의 값을 
                gravity = 0;      // 중력 값을 0으로 고정함
                WallDashTime -= Time.deltaTime;      // 시간을 0이 될 때까지 감소 시킴
                if (Input.GetKeyDown(KeyCode.Space)) // 벽에서 달리다가 점프 키를 다시 누른다면 점프 정보 다시 설정
                {
                    MoveForce.y = JumpForce;  
                    gravity = -20;
                    WallDashTime = 0.0f;
                }
            }
            else // 시간이 0이하가 되면 중력을 다시 돌려놓음
            {
                gravity = -20;
            }
        }
    }
}

```
