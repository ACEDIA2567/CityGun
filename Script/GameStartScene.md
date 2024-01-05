using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;


public class GameStartScene : MonoBehaviour
{
    public GameObject Operation; // 설명 사진

    public void GameStart()
    {
        SceneManager.LoadScene("InGameScene");
    }

    public void GameOperation()
    {
        Operation.SetActive(true);
        // 해당 하위 오브젝트들을 활성화 시킴
        for (int i = 0; i< 4; i++)
        {
            Operation.transform.GetChild(i).gameObject.SetActive(true);
        }
    }
}
