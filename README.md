# CityGun

## 목차
### 게임 설명
###



## 게임 제작 설명
### 개발 툴: Unity 3D
### 사용한 언어: C#
### 플랫폼: PC
### 장르: FPS
### 서브 장르
#### 1) 디펜스 : 시간의 흐름에 따라 적의 종류와 개체 수가 증가함 <br/>2) 보스 : 여러 패턴을 가지고 있는 강한 적<br/>3) 어빌리티 : 플레이어를 강하게 만들어주는 시스템

## 게임 흐름도
![게임흐름도 drawio](https://github.com/ACEDIA2567/CityGun/assets/101154683/716ce524-23e0-443a-b2fe-d1fc8fb1e7c4)

## 게임 설명
### 게임 스토리
#### 먼 미래 인간들은 혁신적인 기술로 로봇들을 개발해 생활을 하고 있었지만 로봇에게
#### 인공지능 기술을 넣어준 후 로봇들이 반란을 일으켜 도시를 침략했고
#### 플레이어가 로봇들의 침략을 막는 이야기

## 게임 특징
### 1. FPS장르를 가졌지만 경험치와 레벨을 만들어 플레이어를 점점 강해지는 특징을 가진다.
### 2. PVE로서 유저가 아닌 적을 상대하는 특징을 가진다. 
### 3. 시간이 지남에 따라서 적의 개체수가 늘어나서 하드코어 해진다.

## 플레이 방법    
### 1. [조작 설명](System/조작%20설명.md)
### 2. [UI 설명](System/UI%20설명.md)

## 시스템 소개    
### 1. [레벨](PlayerInfo/플레이어%20레벨.md)
### 2. [어빌리티](PlayerInfo/플레이어%20어빌리티.md)

## 몬스터 소개
### 1. [볼 봇](MonsterInfo/BallBot.md)
### 2. [스폰 봇](MonsterInfo/SpawnBot.md)
### 3. [스카우트 봇](MonsterInfo/ScoutBot.md)
### 4. [인베이더 봇](MonsterInfo/InvaderBot.md)
### 5. [로키 봇](MonsterInfo/Rockie.md)

## 보스 소개
### 1. [가디언 봇](MonsterInfo/GuardianBot.md)

## 그래픽    
### [맵](System/맵%20설명.md)
### 파티클 가져온 사이트: <br/>https://assetstore.unity.com/packages/vfx/shaders/ultimate-10-shaders-168611 / https://assetstore.unity.com/packages/vfx/particles/legacy-particle-pack-73777

## 사운드    
### 일반 몬스터: 움직이는 동적의 몬스터의 몬스터이므로 3D사운드를 설정
### 보스 몬스터: 움직이지 않는 정적의 몬스터이므로 2D사운드를 설정
### 사운드 가져온 사이트: https://pixabay.com/ko/sound-effects/



## 개발 일정
### 23년 8월 ~ 23년 12월
![20240115_172928](https://github.com/ACEDIA2567/CityGun/assets/101154683/35cc1dca-505e-477b-a0cf-4d974dcacaa9)

## 개발 세부 정리
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

### 23.10.23 | 8주차
#### ● 대학교 중간 고사로 인하여 미실시

### 23.10.30 | [9주차](주차%20정리/9주차.md)
#### ● 일반 몬스터 스포너 구현 및 주기 설정
#### ● 일반 몬스터(Invader Bot) 추가

### 23.11.06 | [10주차](주차%20정리/10주차.md)
#### ● 일반 몬스터 강화 스탯 부여
#### ● 탄 충돌 처리 변경

### 23.11.13 | [11주차](주차%20정리/11주차.md)
#### ● 어빌리티 창 UI
#### ● 플레이어의 경험치와 체력의 정보와 UI

### 23.11.20 | [12주차](주차%20정리/12주차.md)
#### ● 레벨업과 어빌리티 연결
#### ● 플레이어 피격 후 화면

### 23.11.27 | [13주차](주차%20정리/13주차.md)
#### ● 보스 몬스터 (생성, 공격, 피격, UI)

### 23.12.04 | [14주차](주차%20정리/14주차.md)
#### ● 모든 사운드 추가
#### ● 보스 처치 시 씬 이동
#### ● 보스 처치 여부에 따른 텍스트 변경

### 23.12.11 | 15주차
#### ● 대학교 기말 고사로 인하여 미실시

### 23.12.18 | [16주차](주차%20정리/16주차.md)
#### ● Start씬에서 조작 키와 UI 설명을 해주는 버튼 추가
#### ● 버튼 클릭시 해당 버튼 비활성화


