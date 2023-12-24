```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Ability : MonoBehaviour
{
    public Text AbilLv; // 어빌리티 레벨
    public Text AbilExpl; // 어빌리티 설명

    [SerializeField]
    private int A = 1; // 해당 어빌리티 레벨
    private Status status;

    public  string  UpGradeText; // 업그레이드 텍스트
    public  float[] UpGrade;     // 업그레이트 수치
    private Shoot   shoot;
    [SerializeField]
    private GameObject AbilityUI;
    [SerializeField]
    private GameObject AbilityWin;

    public enum AbilityName { Maxsp, SpRecovery, MoveSpeed, SpUse, Maxhp, HpRecovery, AttackDamage, MaxAmmo }; // 해당 어빌리티를 이름을 저장
    [SerializeField]
    private AbilityName SelectName = AbilityName.Maxsp;

    void Start()
    {
        status = GameObject.Find("Player").GetComponent<Status>();
        shoot = GameObject.Find("Player").GetComponent<Shoot>();
    }

    void Update()
    {
        switch (SelectName) // 이름에 따른 텍스트 정보
        {
            case AbilityName.Maxsp:
                AbilExpl.text = UpGradeText + " " + status.MaxSp + " > " + UpGrade[A-1];
                break;
            case AbilityName.SpRecovery:
                AbilExpl.text = UpGradeText + " " + status.SpRecovery + " > " + UpGrade[A - 1];
                break;
            case AbilityName.MoveSpeed:
                AbilExpl.text = UpGradeText + " " + status.AbilitySpeed + " > " + UpGrade[A - 1];
                break;
            case AbilityName.SpUse:
                AbilExpl.text = "소모량\n점프: " + status.AbilitySp + " > " + UpGrade[A - 1] + "\n 대쉬: " + status.AbilitySp + " > " + UpGrade[A - 1] * 2;
                break;
            case AbilityName.Maxhp:
                AbilExpl.text = UpGradeText + " " + status.MaxHp + " > " + UpGrade[A - 1];
                break;
            case AbilityName.HpRecovery:
                AbilExpl.text = UpGradeText + " " + status.HpRecovery + " > " + UpGrade[A - 1];
                break;
            case AbilityName.AttackDamage:
                AbilExpl.text = UpGradeText + " " + shoot.weaponSetting.AttackDamage + " > " + UpGrade[A - 1];
                break;
            case AbilityName.MaxAmmo:
                AbilExpl.text = UpGradeText + " " + shoot.weaponSetting.maxAmmo + " > " + UpGrade[A - 1];
                break;
        }
        AbilLv.text = "LV. " + A; // 현재 레벨 텍스트 출력
    }

    public void AbilClick()
    {
        Cursor.visible               = false;                     // 마우스 커서 안보이게 설정
        Cursor.lockState             = CursorLockMode.Locked;     // 마우스 위치를 고정
        Time.timeScale               = 1.0f;                      // 시간을 되돌림
        shoot.Gun.transform.position = status.transform.position; // 총의 위치 원위치
        switch (SelectName) // 선택지에서 해당 어빌리티 선택 시 강화
        {
            case AbilityName.Maxsp:
                status.MaxSp = UpGrade[A - 1];
                break;
            case AbilityName.SpRecovery:
                status.SpRecovery = UpGrade[A - 1];
                break;
            case AbilityName.MoveSpeed:
                status.AbilitySpeed = UpGrade[A - 1];
                break;
            case AbilityName.SpUse:
                status.AbilitySp = UpGrade[A - 1];
                break;
            case AbilityName.Maxhp:
                status.MaxHp = UpGrade[A - 1];
                break;
            case AbilityName.HpRecovery:
                status.HpRecovery = UpGrade[A - 1];
                break;
            case AbilityName.AttackDamage:
                shoot.weaponSetting.AttackDamage = UpGrade[A - 1];
                break;
            case AbilityName.MaxAmmo:
                shoot.weaponSetting.maxAmmo = (int)UpGrade[A - 1];
                break;
        }
        A++; // 강화 성공 후 해당 어빌리티 레벨 1증가
        if (A - 1 == UpGrade.Length) // 해당 어빌리티가 최대 값이면 어빌리티 삭제
        {
            Debug.Log("강화 최대치 완료로 삭제");
            Destroy(this.gameObject);
        }
        foreach (Transform Child in AbilityWin.transform) // 현재 오브젝트들 비활성화
        {
            Child.gameObject.SetActive(false);
        }
        AbilityUI.SetActive(false); // 어빌리티 창 비활성화
    }
}

```
