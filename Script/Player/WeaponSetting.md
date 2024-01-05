```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public enum WeaponName { AssaultRifle = 0 }

[System.Serializable]
public struct WeaponSetting
{
    public WeaponName weaponName;        // 총기 이름
    public int        currentAmmo;       // 현재 탄약 수
    public int        maxAmmo;           // 최대 탄약 수
    public float      attackRate;        // 공격 속도
    public float      attackDistance;    // 공격 거리
    public bool       isAutomaticAttack; // 자동 공격 여부
    public float      AttackDamage;      // 공격 수치
}

```
