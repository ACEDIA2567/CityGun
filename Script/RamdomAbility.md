```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RandomAbility : MonoBehaviour
{
    public  GameObject[] Abilitys;  // 모든 어빌리티 집합체
    private GameObject   Ability;   // 랜덤에서 선택된 어빌리티 오브젝트
    public  GameObject[] Before;    // 선택지 갯수
    private int          RandomNum; // 랜덤으로 선택할 어빌리티 배열 순서 값
    public  int          AbilNum;   // 선택한 어빌리티 수

    [Header("Sound")]
    [SerializeField]
    AudioClip LevelUpSound;
    private AudioSource audioSource;

    private void Awake()
    {
        audioSource = transform.parent.GetComponent<AudioSource>();
    }


    private void OnEnable()
    {
        audioSource.Play(); // 어빌리티
        Abilitys = RemoveNullElements(Abilitys); // Null인 어빌리티를 삭제하는 함수 실행
        switch (Abilitys.Length) // 처음 시작할 때 최대 3가지의 어빌리티 선택지의 갯수를 알아내기 위함
        {
            case 1:
                Before = new GameObject[1];
                break;
            case 2:
                Before = new GameObject[2];
                break;
            default:
                Before = new GameObject[3];
                break;
        }
        while (true)
        {
            RandomNum = Random.Range(0, Abilitys.Length); // 랜덤으로 어빌리티 집합체에서 1가지를 선택한다.
            if (Abilitys[RandomNum] != null)
            {
                Ability = Abilitys[RandomNum]; // 랜덤으로 뽑은 순서의 어빌리티를 Ability로 초기화
                break;
            }
        }
        AbilNum = 1; // 위에서 랜덤으로 어빌리티를 선택했기 때문에 1로 초기화
        Ability.SetActive(true); // 선택한 어빌리티 활성화
        Before[0] = Ability; // 랜덤으로 선택한 어빌리티 첫 번째 값으로 초기화

        while (true)
        {
            RandomNum = Random.Range(0, Abilitys.Length); // 랜덤 숫자를 변수로 초기화
            for (int i = 0; i < Before.Length; i++)
            {
                if (Before[i] != Abilitys[RandomNum]) // 선택된 어빌리티와 랜덤으로 정해진 어빌리티가 일치하지 않다면
                {
                    // 겹치는 숫자가 없음 다음 배열 숫자 비교해야함
                    if (i == Before.Length-1)
                    {
                        Ability = Abilitys[RandomNum]; // 랜덤으로 정해진 숫자로
                        Before[AbilNum] = Ability;
                        Ability.SetActive(true); // 선택한 어빌리티 활성화
                        AbilNum += 1; // 랜덤으로 어빌리티를 선택했기 때문에 1증가
                    }
                    else
                    {
                        continue; // 마지막까지 비교를 해야하기 때문에 다음 순서로 넘어감
                    }
                }
                else if (Before[i] == Abilitys[RandomNum])
                {
                    // 겹치는 숫자 생김
                    break;
                }
            }
            if (Before[Before.Length - 1] != null) // 선택지의 개수의 마지막 어빌리티가 정해졌다면 어빌리티 선택을 끝냄
            {
                break;
            }
        }
    }

    // 어벌리티 오브젝트에서 최대 레벨 삭제하는 함수
    GameObject[] RemoveNullElements(GameObject[] array)
    {
        // null이 아닌 요소들을 찾아 새 배열에 복사
        GameObject[] newArray = new GameObject[array.Length];
        int newIndex = 0; // 새로운 배열의 인덱스 값
        for (int i = 0; i < array.Length; i++) // 현재 배열의 수 만큼 반복
        {
            if (array[i] != null) // 어빌리티 배열에서 삭제된 어빌리티 오브젝트를 삭제 조건
            {
                newArray[newIndex] = array[i]; // Null이 아닌 배열을 새로운 배열로 초기화
                newIndex++; // 새로운 배열을 추가하였기 때문에 인덱스 값을 1올린다.
            }
        }

        // 삭제한 오브젝트가 있었기 때문에 새로운 배열의 인덱스 값으로 배열 크기 변경
        System.Array.Resize(ref newArray, newIndex);
        // 새 배열을 리턴
        return newArray;
    }
}


```
