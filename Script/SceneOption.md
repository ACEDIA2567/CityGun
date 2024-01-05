```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

public class SceneOption : MonoBehaviour
{
    public Text ScoreText;
    public Text ClearText;
    public Text TimeText;
    private GameObject ScoreUI;
    private float Enemyscore;

    private void Start()
    {
        ScoreUI = GameObject.Find("EnemySpawner");
        if (ScoreUI != null)
        {
            Enemyscore = ScoreUI.GetComponent<ScoreUI>().EnemyScore;
        }
    }

    private void Update() // 텍스트 정보 갱신
    {
        ScoreText.text = "Your Score:" + Enemyscore;
        ClearText.text = ScoreUI.GetComponent<ScoreUI>().BoosDeath == true ? "Game Clear" : "Game Over";
        TimeText.text = ScoreUI.GetComponent<ScoreUI>().TimeMin.ToString("F0") + ":" + ScoreUI.GetComponent<ScoreUI>().TimeSec.ToString("F0");
    }

    public void GameStart() // 인게임씬으로 이동
    {
        SceneManager.LoadScene("InGameScene");
    }

    public void GameReStart() // 인트로씬으로 이동
    {
        SceneManager.LoadScene("StartScene");
    }

    public void GameEnd() // 게임 종료
    {
        Application.Quit();
    }
}

```
