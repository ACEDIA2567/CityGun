```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Scout : MonoBehaviour
{
    [SerializeField]
    private GameObject Spawner;

    void Update()
    {
        ScoutUp();
    }

    private void ScoutUp()
    {
        if (transform.position.y < 20)
        {
            transform.position += Vector3.up * Time.deltaTime;
        }
        else
        {
            // 오브젝트가 Y축이 20보다 높아지면 스포너에 Invader를 소환하는 조건을 충족시키게 함
            Spawner.GetComponent<Spawner>().ScoutUpCheck = true;
        }
    }
}
```
