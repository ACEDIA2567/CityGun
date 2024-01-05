```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[RequireComponent(typeof(MeshRenderer))]
public class BossMvement : MonoBehaviour
{
    private MeshRenderer meshRenderer;
    private BossStatus bossStatus;
    public float speed = .1f;
    private Material mat;

    private void Start()
    {
        bossStatus = GetComponent<BossStatus>();
        meshRenderer = GetComponent<MeshRenderer>();
        meshRenderer.material.SetFloat("_Cutoff", 1);
    }

    private void Update()
    {
        BossSpawn();
    }

    private void BossSpawn()
    {
        // 보스의 텍스처가 0.05f보다 크다면 텍스쳐를 비율을 올림
        if (meshRenderer.material.GetFloat("_Cutoff") > 0.05f)
        {
            // 메테리얼 변수 mat에 현재 메테리얼 값으로 초기화
            mat = meshRenderer.material;

            // Mathf.Clamp01으로 0과1사이로 만들어 시간에 따라서 _Cutoff의 값을 줄임
            mat.SetFloat("_Cutoff", Mathf.Clamp01(mat.GetFloat("_Cutoff") - Time.deltaTime * speed));
            meshRenderer.material = mat; // 메테리얼 값으로 초기화
        }
        else // 위의 조건을 만족하면 보스의 AttackStart즉 공격을 시작하게 한다.
        {
            bossStatus.AttackStart = true;
        }
    }


}
```
