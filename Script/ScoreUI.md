```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class ScoreUI : MonoBehaviour
{
    public Text Score;             // 점수 UI 텍스트
    public float EnemyScore;       // 점수
    public bool BoosDeath = false; // 보스의 죽음 여부
    private Spawner spawner;
    public float TimeMin;          // 게임시간(분)
    public float TimeSec;          // 게임시간(초)

    void Start()
    {
        spawner = GetComponent<Spawner>();
        EnemyScore = 0.0f;
    }

    public void Update()
    {
        TimeMin = spawner.minPlayTime;
        TimeSec = spawner.secondPlayTime;
        if (Score != null)
        {
            Score.text = "Score:" + EnemyScore; // 점수를 획득 시 갱신
        }

        if (BoosDeath) // 보스가 죽으면 SceneMove함수를 3초 후 실행
        {
            Invoke("SceneMove", 3);
        }

        // 현재씬이 EndScene이라면
        if (SceneManager.GetActiveScene().name == "EndScene")
        {
            this.gameObject.SetActive(false);
        }
    }

    private void SceneMove()
    {
        Cursor.visible = true; // 마우스 커서 안보이게 설정
        Cursor.lockState = CursorLockMode.None; // 마우스 위치를 고정
        transform.GetComponent<Spawner>().enabled = false;
        DontDestroyOnLoad(this.gameObject);
        if (SceneManager.GetActiveScene().name != "EndScene")
        {
            SceneManager.LoadScene("EndScene");
        }
    }
}

```
