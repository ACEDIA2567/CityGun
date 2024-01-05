```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class Invader_Bullet : MonoBehaviour
{
    private GameObject EnemySpawner;

    private void Start()
    {
        EnemySpawner = GameObject.Find("EnemySpawner");
        Destroy(this.gameObject, 5.0f); // 5초뒤 탄 삭제
    }

    void Update()
    {
        transform.Translate(Vector3.forward * Time.deltaTime * 5); // 탄이 계속 앞으로 전진하게함
    }

    private void OnTriggerEnter(Collider other)
    {
        // 총알과 충돌한 오브젝트의 태그가 Player라면
        if (other.gameObject.tag == "Player")
        {
            // 플레이어의 체력이 1이하라면
            if (other.GetComponent<Status>().MinHp < 1)
            {
                EnemySpawner.GetComponent<Spawner>().enabled = false;
                DontDestroyOnLoad(EnemySpawner);
                Cursor.visible = true; // 마우스 커서 보이게 설정
                Cursor.lockState = CursorLockMode.None; // 마우스 위치를 고정 해제
                SceneManager.LoadScene("EndScene");
                Destroy(this.gameObject);
            }
            else // 플레이어의 체력이 1보다 크다면
            {
                other.GetComponent<Status>().MinHp -= 1;
            }
            Destroy(this.gameObject);
        }
    }
}

``
