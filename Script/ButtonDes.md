```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.EventSystems;

public class ButtonDes : MonoBehaviour, IPointerClickHandler
{
    public void OnPointerClick(PointerEventData eventData) // 클릭 했을 때 사용되는 이벤트 함수
    {
        // eventData를 이용하여 클릭한 오브젝트의 정보를 선언 및 초기화
        GameObject clickedObject = eventData.pointerPress;

        // 클릭한 오브젝트를 비활성화
        clickedObject.SetActive(false);
    }
}
```
