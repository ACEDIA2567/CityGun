```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class BossRocket : MonoBehaviour
{
    private Status player;
    [SerializeField]
    private GameObject EnemySpawner;

    void Start()
    {
        player = GameObject.Find("Player").GetComponent<Status>();
    }

    private void OnParticleCollision(GameObject other)
    {
        // 충돌한 오브젝트의 이름이 Player라면 체력을 1 감소 시킨다.
        if (other.gameObject.name == "Player")
        {
            // 체력이 1보다 작다면 마우스관련 고정 보이게 설정 하고 게임 종료씬으로 이동시킴
            if (player.GetComponent<Status>().MinHp < 1)
            {
                EnemySpawner.GetComponent<Spawner>().enabled = false;
                DontDestroyOnLoad(EnemySpawner);
                Cursor.visible = true; // 마우스 커서 보이게 설정
                Cursor.lockState = CursorLockMode.None; // 마우스 위치를 고정해제
                SceneManager.LoadScene("EndScene");
            }
            else // 체력이 1보다 크다면 피격화면은 활성화시키고 체력을 1 감소시킨다.
            {
                player.GetComponent<Status>().MinHp -= 1;
                player.GetComponent<Status>().HitCheck = true;
            }
        }
    }
}

```
