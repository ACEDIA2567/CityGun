```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerAnimator : MonoBehaviour
{
    private Animator animator;

    private void Awake()
    {
        animator = GetComponentInChildren<Animator>(); // 자식 오브젝트에서 Animator라는 컴포넌트 정보를 가져옴
    }

    public float MoveSpeed // MoveMentSpeed 값을 MoveSpeed로 읽고 쓸수 있게 함
    {
        set => animator.SetFloat("MoveMentSpeed", value);
        get => animator.GetFloat("MoveMentSpeed");
    }

    public void OnReload() // Animator에서 OnReload라는 파라미터를 작동시킴
    {
        animator.SetTrigger("OnReload");
    }

    public void Play(string stateName, int layer, float normalizedTime) // Animator의 작동을 시켜주기 위한 함수
    {
        animator.Play(stateName, layer, normalizedTime);
    }

    public void StopAni() // 애니메이션을 중단
    {
        animator.speed = 0;
    }

    public void StartAni() // 애니메이션을 재생
    {
        animator.speed = 1.0f;
    }

    public bool CurrentAnimationIs(string name) // 현재 애니메이션이 맞는 지의 조건 여부를 확인하기 위한 함수
    {
        // 매개변수 name의 animator가 실행 중인지 확인하고 그 결과를 반영하는 함수
        return animator.GetCurrentAnimatorStateInfo(0).IsName(name);
    }
}

```
