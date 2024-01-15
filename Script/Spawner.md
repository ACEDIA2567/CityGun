```cs
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class Spawner : MonoBehaviour
{
    [Header("EnemySpawnerInfo")]
    private float RandomSpawner; // 4군데의 랜덤 스폰 장소
    private Vector3 Spawner1;    // 스포너 위치1
    private Vector3 Spawner2;    // 스포너 위치2
    private Vector3 Spawner3;    // 스포너 위치3
    private Vector3 Spawner4;    // 스포너 위치4
    private float SpawnTime;
    private float SecondPlayTime = 0;
    public float secondPlayTime
    {
        get { return SecondPlayTime; }
    }
    private float MinPlayTime = 0;
    public float minPlayTime
    {
        get { return MinPlayTime; }
    }
    public bool ScoutUpCheck = false;
    [SerializeField]
    private GameObject FlySpawner1; // 공중 스포너 위치1
    [SerializeField]
    private GameObject FlySpawner2; // 공중 스포너 위치2
    [SerializeField]
    private GameObject FlySpawner3; // 공중 스포너 위치3
    [SerializeField]
    private GameObject FlySpawner4; // 공중 스포너 위치4
    public Text TimeText;

    [Header("EnemyInfo")]
    [SerializeField]
    private GameObject[] Enemys;
    [SerializeField]
    private bool HpUpCheck = false;
    public float EnemyCount = 0;
    [SerializeField]
    private GameObject BossObject;

    void Start()
    {
        Spawner1 = new Vector3(Random.Range(-1.5f, 5f), 1.5f, 65);
        Spawner2 = new Vector3(60, 1.5f, Random.Range(-5f, 1.5f));
        Spawner3 = new Vector3(Random.Range(-1.5f, 5f), 1.5f, -58);
        Spawner4 = new Vector3(-60, 1.5f, Random.Range(-5f, 1.5f));
        SpawnTime = 10.2f;
        StartCoroutine(BotSpawn(SpawnTime, Enemys[0])); // 처음 시작
        StartCoroutine(FlyBotSpawn(SpawnTime, Enemys[2]));
    }

    void Update()
    {
        PlayTimeCheck();
    }

    private void PlayTimeCheck()
    {
        SecondPlayTime += Time.deltaTime;
        TimeText.text = MinPlayTime + " : " + SecondPlayTime.ToString("F0");
        if (SecondPlayTime >= 60.0f)
        {
            MinPlayTime += 1;
            switch (MinPlayTime)
            {
                case 5:
                    StartCoroutine(BotSpawn(SpawnTime, Enemys[1])); // 처음 시작
                    break;
                case 10:
                    // 적들의 체력 증가
                    HpUpCheck = true;
                    break;
                case 15:
                    // 보스 소환
                    if (!BossObject.activeSelf)
                    {
                        BossObject.SetActive(true);
                    }
                    break;
            }
            SecondPlayTime = 0;
        }

        // 시간에 따라서 스테이지 진행
    }

    private void EnemyRandomSpawn(GameObject Enemy, Vector3 Spawner1, Vector3 Spawner2, Vector3 Spawner3, Vector3 Spawner4)
    {
        // 적 스폰
        if (EnemyCount < 60 || Enemy == Enemys[2]) // 60마리가 넘을 시에 적을 스폰하지 않음
        {
            EnemyCount += 1.0f;
            switch (RandomSpawner) // 매개변수에 있는 몬스터를 스폰하고 시간에 따라서 체력과 경험치량 증가
            {
                case 0:
                    GameObject a = Instantiate(Enemy, Spawner1, Quaternion.identity);
                    a.SetActive(true);
                    if (HpUpCheck)
                    {
                        a.GetComponent<RoBotStatus>().maxHp += 5.0f;
                        a.GetComponent<RoBotStatus>().exp += 1.0f;
                    }
                    break;

                case 1:
                    GameObject b = Instantiate(Enemy, Spawner2, Quaternion.identity);
                    b.SetActive(true);
                    if (HpUpCheck)
                    {
                        b.GetComponent<RoBotStatus>().maxHp += 5.0f;
                        b.GetComponent<RoBotStatus>().exp += 1.0f;
                    }
                    break;

                case 2:
                    GameObject c = Instantiate(Enemy, Spawner3, Quaternion.identity);
                    c.SetActive(true);
                    if (HpUpCheck)
                    {
                        c.GetComponent<RoBotStatus>().maxHp += 5.0f;
                        c.GetComponent<RoBotStatus>().exp += 1.0f;
                    }
                    break;

                case 3:
                    GameObject d = Instantiate(Enemy, Spawner4, Quaternion.identity);
                    d.SetActive(true);
                    if (HpUpCheck)
                    {
                        d.GetComponent<RoBotStatus>().maxHp += 5.0f;
                        d.GetComponent<RoBotStatus>().exp += 1.0f;
                    }
                    break;
            }
        }
    }

    IEnumerator FlyBotSpawn(float SpawnTime, GameObject Enemy)
    {
        while (true)
        {
            if (ScoutUpCheck) // 스카우트봇이 목표지점에 도달했는지 여부
            {
                RandomSpawner = Random.Range(0, 4);
                EnemyRandomSpawn(Enemy, FlySpawner1.transform.position, FlySpawner2.transform.position, FlySpawner3.transform.position, FlySpawner4.transform.position);
            }
            yield return new WaitForSeconds(SpawnTime);
        }
    }

    IEnumerator BotSpawn(float SpawnTime, GameObject Enemy) 
    {
        while (true)
        {
            RandomSpawner = Random.Range(0, 4);
            EnemyRandomSpawn(Enemy, Spawner1, Spawner2, Spawner3, Spawner4);
            if (SpawnTime > 3) SpawnTime -= 0.2f; // 실행 될때 마다 스폰 시간을 줄인다.
            yield return new WaitForSeconds(SpawnTime);
        }
    }

}


```
