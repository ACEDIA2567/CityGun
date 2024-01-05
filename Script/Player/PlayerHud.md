```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using TMPro;
using UnityEngine.UI;

public class PlayerHud : MonoBehaviour
{
    [Header("Components")]
    [SerializeField]
    private Shoot           weapon;            // 현재 정보가 출력되는 무기

    [Header("Weapon Base")]
    [SerializeField]
    private TextMeshProUGUI WeaponTextName;    // 무기이름
    [SerializeField]
    private Image           WeaponImageIcon;   // 무기 아이콘
    [SerializeField]
    private Sprite[]        WeaponSpriteIcons; // 무기 아이콘 배열

    [Header("Ammo")]
    [SerializeField]
    private TextMeshProUGUI AmmoText;          // 탄 수

    

    private void Awake()
    {
        SetupWeapon();

        // 메소드가 등록되어 있는 이벤트 클래스(weapon.xx)의
        // Invoke() 메소드가 호출될 때 등록된 메소드(매개변수)가 실행된다.

        weapon.onAmmoEvent.AddListener(UpdateAmmoHUD);
    }

    private void SetupWeapon()
    {
        WeaponTextName.text = weapon.WeaponName.ToString();
        WeaponImageIcon.sprite = WeaponSpriteIcons[(int)weapon.WeaponName];
    }

    private void UpdateAmmoHUD(int currentAmmo, int maxAmmo)
    {
        AmmoText.text = $"<size=40>{currentAmmo}</size>/{maxAmmo}";
    }
}

```
