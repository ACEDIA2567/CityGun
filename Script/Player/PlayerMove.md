```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerMove : MonoBehaviour
{
    [Header("Input KeyCodes")]
    [SerializeField]
    private KeyCode keyCodeRun = KeyCode.LeftShift; // 달리는 키
    [SerializeField]
    private KeyCode keyCodeReload = KeyCode.R;      // 재장전 키
    [SerializeField]
    private KeyCode KeyCodeJump = KeyCode.Space;    // 점프 키

    [Header("Sound")]
    [SerializeField]
    private AudioClip WalkSound;   // 걷는 사운드
    [SerializeField]
    private AudioClip RunSound;    // 뛰는 사운드
    [SerializeField]
    private AudioClip JumpSound;   // 점프 사운드
    [SerializeField]               
    private AudioClip DashSound;   // 대쉬 사운드

    private CameraMove               cameraMove;
    private MovementPlayerController Movement;        
    private Status                   status; 
    private PlayerAnimator           playerAnimator;            
    private Shoot                    shoot; 
    public  LayerMask WallLayer;                      // 벽의 레이어
    private float     WallAngle = 0;                  // 플레이어의 화면 z값
    private CharacterController characterController;  // 캐릭터 움직임
    private AudioSource         audioSource;          // 사운드 소스

    void Start()
    {
        Cursor.visible = false;                       // 마우스 커서 안보이게 설정
        Cursor.lockState = CursorLockMode.Locked;     // 마우스 위치를 고정

        // 받아온 컴포넌트 정보를 변수로 초기화
        characterController = GetComponent<CharacterController>();
        cameraMove          = GetComponent<CameraMove>();
        Movement            = GetComponent<MovementPlayerController>();
        status              = GetComponent<Status>();
        playerAnimator      = GetComponent<PlayerAnimator>();
        shoot               = GetComponent<Shoot>();
        audioSource         = GetComponent<AudioSource>();
    }

    void Update()
    {
        if (Time.timeScale > 0) // 게임의 시간 흐름이 0일 때의 조건문
        {
            CameraRotation(); // 마우스 커서로 인한 카메라가 움직이는 함수
        }
        MoveUpdate();
        JumpUpdate();
        UpdateWeaponAction();
        DashUpdate();
        WallRunUpdate();
    }

    // 카메라 회전 함수
    private void CameraRotation()
    {
        float MouseX = Input.GetAxis("Mouse X"); // 마우스 커서의 x위치로 변수 선언 및 초기화
        float MouseY = Input.GetAxis("Mouse Y"); // 마우스 커서의 y위치로 변수 선언 및 초기화

        cameraMove.UpdateRotate(MouseX, MouseY, WallAngle); // cameraMove스크립트의 
    }

    // 플레이어의 이동 함수
    private void MoveUpdate()
    {
        if (Movement.RunDelayTime > 0)
        {
            Movement.RunDelayTime -= Time.deltaTime;
        }
        float x = Input.GetAxisRaw("Horizontal");
        float z = Input.GetAxisRaw("Vertical");

        if (Movement.MoveSpeed == status.RunSpeed && Movement.RunDelayTime <= 0.0f)
        {
            status.MinSp -= 7 * Time.deltaTime;
            if (status.MinSp < 0.0f)
            {
                Movement.RunDelayTime = 3.0f;
            }
        }
        if (x != 0 || z != 0)
        {
            bool RunCheck = false;

            if (z > 0 && Movement.RunDelayTime <= 0.0f) RunCheck = Input.GetKey(keyCodeRun);

            // 달리기와 걸을 때의 값 변경
            Movement.MoveSpeed       = RunCheck == true ? status.RunSpeed : status.WalkSpeed;
            playerAnimator.MoveSpeed = RunCheck == true ? 1 : 0.5f;
            audioSource.clip         = RunCheck == true ? RunSound : WalkSound;
            audioSource.pitch        = RunCheck == true ? 0.75f : 1.0f; // 사운드 속도

            if (!RunCheck)
            {
                audioSource.volume = 0.2f;
            }
            else
            {
                audioSource.volume = 0.5f;
            }

            if (audioSource.isPlaying == false && characterController.isGrounded) 
            // Update문에서 매프레임별로 재생이 되기 때문에
            // 재생이 안될 때만 재생하도록 함
            {
                audioSource.loop = true;
                audioSource.Play();
            }
        }
        else
        {
            Movement.MoveSpeed = 0;
            playerAnimator.MoveSpeed = 0;
            if (audioSource.isPlaying && characterController.isGrounded)
            // 움직이지 않을 때 재생이 되고 있으면 멈추게 함
            {
                audioSource.loop = false;
                audioSource.Stop();
            }
        }

        Movement.MoveTo(new Vector3(x, 0, z));
    }

    // 점프
    private void JumpUpdate()
    {
        // 점프 전에 플레이어가 바닥에 있는지 여부 확인
        if (characterController.isGrounded)
        {
            // 기력의 기본 소모값에 어빌리티 강화로 소모값까지해서 감소하게 함
            if (Input.GetKeyDown(KeyCodeJump) && status.MinSp >= 10.0f - status.AbilitySp)
            {
                Movement.Jump();
                status.MinSp -= 10.0f - status.AbilitySp;
                audioSource.Stop();
                audioSource.PlayOneShot(JumpSound);
            }
        }
    }

    // 대쉬
    private void DashUpdate()
    {
        if (Movement.DashDelayTime < 0)
        {
            Movement.canStartDash = false;
        }
        else if (Movement.DashDelayTime > 0)
        {
            Movement.DashDelayTime -= Time.deltaTime;
        }
        // 기력의 기본 소모값에 어빌리티 강화로 소모값까지해서 감소하게 함
        if (Input.GetKeyDown(keyCodeRun) && Movement.canStartDash && !Movement.isDashing && status.MinSp > 30.0f - status.AbilitySp)
        {
            float x = Input.GetAxisRaw("Horizontal");
            float z = Input.GetAxisRaw("Vertical");
            Movement.dashDirection = new Vector3(x, 0, z).normalized; // 플레이어의 현재 방향으로 대쉬
            Movement.isDashing     = true;
            Movement.canStartDash  = false;
            Movement.dashTimer     = 0.0f;
            status.MinSp          -= 30.0f - status.AbilitySp;
            audioSource.PlayOneShot(DashSound);
        }
        else
        {
            if (Input.GetKeyDown(keyCodeRun))
            {
                Movement.canStartDash = true;
                Movement.DashDelayTime = 0.3f;
            }
        }
        // 대쉬 중이라면
        if (Movement.isDashing)
        {
            // 대쉬 시간만큼 이동하게 함
            Movement.dashTimer += Time.deltaTime;
            if (Movement.dashTimer < Movement.dashDuration)
            {
                // 대쉬 동작을 이동으로 구현
                Movement.MoveTo(Movement.dashDirection * Movement.dashDistance);
            }
            else // 끝나면 대쉬 종료
            {
                Movement.isDashing = false;
            }
        }
    }
    
    // 벽 달리기
    private void WallRunUpdate()
    {
        RaycastHit hit;
        if (Physics.Raycast(transform.position, transform.right, out hit, 0.8f, WallLayer) || Physics.Raycast(transform.position, -transform.right, out hit, 0.8f, WallLayer))
        {
            if (Input.GetKey(KeyCodeJump) && Movement.MoveForce.y < 5.0f && Movement.MoveForce.y > -5.0f) // 바닥에 있지 않고 점프키를 계속 누르고 있다면
            {
                // 왼쪽과 오른쪽의 차이로 카메라 각도 변경
                if (Physics.Raycast(transform.position, transform.right, out hit, 0.8f, WallLayer) && Movement.WallDashTime > 0 && !Movement.WallCheck)
                {
                    WallAngle = 15;
                }
                else if (Physics.Raycast(transform.position, -transform.right, out hit, 0.8f, WallLayer) && Movement.WallDashTime > 0 && !Movement.WallCheck)
                {
                    WallAngle = -15;
                }
                else if (Movement.WallCheck)
                {
                    WallAngle = 0;
                }
                Movement.WallDash();
            }
        }
        else
        {
            WallAngle = 0;
            Movement.gravity = -20;
        }
    }

    // 총기 관련
    private void UpdateWeaponAction()
    {
        if (Input.GetMouseButtonDown(0) && status.ActiveCheck)
        {
            shoot.StartWeaponAction();
        }
        else if (Input.GetMouseButtonUp(0) || status.ActiveCheck == false)
        {
            shoot.StopWeaponAction();
        }

        if (Input.GetKeyDown(keyCodeReload))
        {
            shoot.StartReload();
        }
    }
}
```
