```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BlueVariantAttack : MonoBehaviour
{
    [SerializeField]
    private GameObject Robot;

    private void OnEnable()
    {
        // 오브젝트가 활성화일때 공격처리를 하기 위해 OnEnable를 사용하고 해당 오브젝트에 매쉬를 뺌
        Robot.GetComponent<BlueVariant>().AttackCheck = true;
    }

    private void OnTriggerStay(Collider other)
    {
        // 충돌체의 태그가 Player라면
        if (other.gameObject.tag == "Player")
        {
            // 공격 시작
            if (Robot.GetComponent<BlueVariant>().AttackCheck)
            {
                Robot.GetComponent<BlueVariant>().Attack();
                // 공격을 시도 했다면 오브젝트를 다시 비활성화
                gameObject.SetActive(false);
            }
        }
    }
}

```
