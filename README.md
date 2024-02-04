# CityGun

## 목차
### ● [개발 툴 및 언어 설명](#개발-툴-및-언어-설명)
### ● [게임 다운로드 링크 및 플레이 영상](#게임-다운로드-링크-및-플레이-영상)
### ● [모티브로 한 게임](#모티브로-한-게임)
### ● [게임 흐름도](#게임-흐름도)
### ● [게임 설명](#게임-설명)
### ● [플레이 방법](#플레이-방법)
### ● [시스템 소개](#시스템-소개)
### ● [몬스터 소개](#몬스터-소개)
### ● [보스 소개](#보스-소개)
### ● [그래픽](#그래픽)
### ● [사운드](#사운드)
### ● [개발 일정](#개발-일정)
### ● [개발 후기](#개발-후기)

<br/>

## 개발 툴 및 언어 설명
### 개발 툴: Unity 3D
### 사용한 언어: C#
### [작성한 코드 폴더](Script)
### 플랫폼: PC

<br/>

## 게임 다운로드 링크 및 플레이 영상
### 다운로드 주소 : https://drive.google.com/file/d/1TCpMh7mbiDp-Ige4HLQx5D4PLLzp9Dni/view?usp=drive_link </br> (압축풀기 -> 폴더 안에서 CityGun.exe 실행)
### 플레이 영상 : https://youtu.be/5Kmw4vP7V-U

<br/>

## 모티브로 한 게임
### 1. 니어 오토 마타 (스퀘어에닉스 2017.03.17)
#### ● : 플레이어가 혼자서 여러 적을 쓰러트리고 나가는 솔로 플레이가 가능함
#### ● : 작은 로봇부터 큰 로봇까지 여러 로봇이 등장하고 로봇마다의 다른 공격 패턴을 가짐

### 2. 뱀파이어 서바이벌즈 (Poncle 2022.10.20)
#### ● : 로그라이크로서 계속 나오는 적을 처치하고 나오는 경험치로 레벨 업을 하여 플레이어를 강화 시킬 수 있음
#### ● : 강화를 할 때 랜덤으로 3가지를 선택해서 원하는 어빌리티를 선택할 수 있음

<br/>

## 게임 흐름도
![게임흐름도 drawio](https://github.com/ACEDIA2567/CityGun/assets/101154683/716ce524-23e0-443a-b2fe-d1fc8fb1e7c4)

<br/>

## 게임 설명
### 메인 장르: FPS
### 서브 장르
#### 1) 디펜스 : 시간의 흐름에 따라 적의 종류와 개체 수가 증가함
#### 2) 보스 : 여러 패턴을 가지고 있는 강한 적
#### 3) 어빌리티 : 플레이어를 강하게 만들어주는 시스템

### 게임 스토리
#### 먼 미래 인간들은 혁신적인 기술로 로봇들을 개발해 생활을 하고 있었지만 로봇에게
#### 인공지능 기술을 넣어준 후 로봇들이 반란을 일으켜 도시를 침략했고
#### 플레이어가 로봇들의 침략을 막는 이야기

### 게임 특징
#### 1. FPS장르를 가졌지만 경험치와 레벨을 만들어 플레이어를 점점 강해지는 특징을 가진다.
#### 2. PVE로서 유저가 아닌 적을 상대하는 특징을 가진다. 
#### 3. 시간이 지남에 따라서 적의 개체수가 늘어나서 하드코어 해진다.

<br/>

## 플레이 방법    
### 1. [조작 설명](System/조작%20설명.md)
### 2. [UI 설명](System/UI%20설명.md)

<br/>

## 시스템 소개    
### 1. [레벨](PlayerInfo/플레이어%20레벨.md)
### 2. [어빌리티](PlayerInfo/플레이어%20어빌리티.md)

<br/>

## 몬스터 소개
## 흐름도와 몬스터 정보
### 1. [볼 봇](MonsterInfo/BallBot.md)
### 2. [스폰 봇](MonsterInfo/SpawnBot.md)
### 3. [스카우트 봇](MonsterInfo/ScoutBot.md)
### 4. [인베이더 봇](MonsterInfo/InvaderBot.md)
### 5. [로키 봇](MonsterInfo/Rockie.md)

<br/>

## 보스 소개
## 흐름도와 보스 정보
### 1. [가디언 봇](MonsterInfo/GuardianBot.md)

<br/>

## 그래픽    
### [맵 설명](System/맵%20설명.md)
### 파티클 에셋 사이트:
#### 1. https://assetstore.unity.com/packages/vfx/shaders/ultimate-10-shaders-168611
#### 2. https://assetstore.unity.com/packages/vfx/particles/legacy-particle-pack-73777

## 사운드    
### 3D사운드 : 일반 몬스터
### 2D사운드 : BackGroundBGM, UI, 탄, 보스 몬스터
### 사운드 사이트: https://pixabay.com/ko/sound-effects/

<br/>

## 개발 일정
### 23년 8월 ~ 23년 12월
![20240115_172928](https://github.com/ACEDIA2567/CityGun/assets/101154683/35cc1dca-505e-477b-a0cf-4d974dcacaa9)

## 개발 세부 정리
## [개발 정리 폴더 이동](주차%20정리)
### 23.08.07 | 1주차 (프로토 타입)
#### ● 게임 개발 기획

### 23.08.14 | [2주차 (프로토 타입)](주차%20정리/2주차&#40;프로토%20타입&#41;.md)
#### ● 키보드로 플레이어 이동
#### ● 플레이어 달리기 추가
#### ● 마우스로 화면 회전
#### ● 화면 중심으로 크로스헤어

### 23.08.21 | [3주차 (프로토 타입)](주차%20정리/2주차&#40;프로토%20타입&#41;.md)
#### ● 플레이어 화면에 총 거치
#### ● 마우스로 좌(탄 발사), 우(줌)

### 23.08.28 | [4주차 (프로토 타입)](주차%20정리/2주차&#40;프로토%20타입&#41;.md)
#### ● 적 (추적, 생성)
#### ● 시작, 종료 씬 추가
#### ● 맵 추가
#### ● 화면 상단에 점수 UI 추가
#### ● 적 처치 시 점수 획득

### 23.09.04 | 1주차
#### ● 프로토 타입 이후의 개발 계획

### 23.09.11 | [2주차](주차%20정리/2주차&#40;프로토%20타입&#41;.md)
#### ● 재장전 (애니메이션, 탄 관련)
#### ● 탄 개수 UI

### 23.09.18 | [3주차](주차%20정리/3주차&#40;프로토%20타입&#41;.md)
#### ● 기본 적의 움직임 NavmeshAgent로 변경
#### ● 맵에 Navemesh Bake로 연결

### 23.09.25 | [4주차](주차%20정리/4주차&#40;프로토%20타입&#41;.md)
#### ● 플레이어 점프(높은, 낮은) 및 중력 구현
#### ● 맵 추가 수정
#### ● 적 AI(네비메시) 추가 변경

### 23.10.02 | [5주차](주차%20정리/5주차.md)
#### ● 사격 시 화면 반동
#### ● 맵 추가 수정

### 23.10.09 | [6주차](주차%20정리/6주차.md)
#### ● 플레이어 기력 UI 추가
#### ● 플레이어 행동시 기력 감소 및 증가추가
#### ● 일반 몬스터(Spawn Bot) 추가
#### ● 일반 몬스터(Scout Bot) 추가

### 23.10.16 | [7주차](주차%20정리/7주차.md)
#### ● 플레이어 대쉬
#### ● 플레이어 벽 뛰기
#### ● 플레이어 행동 후 기력 소모

### 23.10.30 | [8주차](주차%20정리/8주차.md)
#### ● 일반 몬스터 스포너 구현 및 주기 설정
#### ● 일반 몬스터(Invader Bot) 추가

### 23.11.06 | [9주차](주차%20정리/9주차.md)
#### ● 일반 몬스터 강화 스탯 부여
#### ● 탄 충돌 처리 변경

### 23.11.13 | [10주차](주차%20정리/10주차.md)
#### ● 어빌리티 창 UI
#### ● 플레이어의 경험치와 체력의 정보와 UI

### 23.11.20 | [11주차](주차%20정리/11주차.md)
#### ● 레벨업과 어빌리티 연결
#### ● 플레이어 피격 후 화면

### 23.11.27 | [12주차](주차%20정리/12주차.md)
#### ● 보스 몬스터 (생성, 공격, 피격, UI)

### 23.12.04 | [13주차](주차%20정리/13주차.md)
#### ● 모든 사운드 추가
#### ● 보스 처치 시 씬 이동
#### ● 보스 처치 여부에 따른 텍스트 변경

### 23.12.18 | [14주차](주차%20정리/14주차.md)
#### ● Start씬에서 조작 키와 UI 설명을 해주는 버튼 추가
#### ● 버튼 클릭시 해당 버튼 비활성화

</br>

## 개발 후기
### 프로젝트를 진행하면서 에셋(애니메이션, 사운드)을 받아서 사용하였는데 같은 제작사의 에셋이 아니다 보니 애니메이션에 맞춰서 사운드를 넣기가 아주 까다로웠고 또 유니티에서 AI기능인 NavmeshAgent를 사용하였는데 NavmeshAgent에 기능이 많다 보니 해당 기능이 있는 오브젝트가 많을수록 메모리를 많이 잡는다는 것을 알게 됨
### FPS장르다 보니 총을 견착하거나 줌을 할 때 위치를 정해줘야하는데 플레이어가 직접 쏘는 듯한 느낌을 줘야 하다 보니 다른 FPS게임을 플레이하면서 좋은 위치를 배워 사용하게 되었음
### 적의 스테이터스와 행동을 따로 하여 상속을 시켜서 사용하지 않고 스크립트를 컴포넌트화시켜서 사용하였는데 나중에는 상속과 컴포넌트화를 차별화하여서 알맞게 사용하여 공부를 더 해야겠음

### 요약
#### 1. NavmeshAgent는 기능이 많아 많은 오브젝트에 넣으면 메모리를 많이 잡아 효율적이지 않음
#### 2. 스크립트 컴포넌트화 말고 상속을 하여 사용해야겠음
#### 3. 오브젝트의 애니메이션과 사운드를 자연스럽게 하는 것은 힘듦
#### 4. 구현하고 싶은 것이 애매하다면 장르가 비슷한 게임을 많이 플레이하는 것이 좋음
