```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraMove : MonoBehaviour
{
    [SerializeField]
    private float CameraXspeed = 5f; // 카메라 X축 속도
    [SerializeField]
    private float CameraYspeed = 3f; // 카메라 Y축 속도
    
    private float LimitMinX = -80f;  // 카메라 X축 최소 값
    private float LimitMaxX = 50f;   // 카메라 Y축 최대 값
    public float eulerAngleX;
    public float eulerAngleY;

    private void Start()
    {
        eulerAngleY = 0;
        eulerAngleX = 0;
    }

    // 마우스 위치로 플레이어의 카메라 회전값을 바꾸는 함수
    public void UpdateRotate(float MouseX, float MouseY, float WallAngle)
    {
        eulerAngleY += MouseX * CameraYspeed; // 마우스 좌우로 카메라 Y축 회전
        eulerAngleX -= MouseY * CameraXspeed; // 마우스 상하로 카메라 X축 회전

        eulerAngleX = ClmapAngle(eulerAngleX, LimitMinX, LimitMaxX);

        transform.rotation = Quaternion.Euler(eulerAngleX, eulerAngleY, WallAngle);
    }

    private float ClmapAngle(float angle, float min, float max)
    {   // angle의 값이 360에서 + 또는 -로 그 361이 되면 다시 0으로 바꾸는 함수
        if (angle < -360) angle += 360;
        if (angle > 360) angle -= 360;

        return Mathf.Clamp(angle, min, max);
    }
}

```
