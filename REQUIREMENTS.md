# Tokyo Railway Guide App - Requirements


> 한국인 관광객을 위한 도쿄 철도 정보 모바일 애플리케이션


---


## 1. 프로젝트 개요


### 프로젝트명


**Tokyo Railway Guide**


### 플랫폼


- iOS
- Android


### 기술 스택


- Expo
- React Native
- TypeScript
- Expo Router
- Backend API
- ODPT API
- Vercel
- 향후 PostgreSQL 등 DB 도입


### 개발 목적


기존에 개발한 JR East, Tokyo Metro, Toei Subway 웹 서비스를 기반으로
모바일 환경에 최적화된 도쿄 철도 정보 애플리케이션을 개발한다.


단순히 기존 웹사이트를 모바일 앱으로 옮기는 것이 아니라,
실제 도쿄를 여행하는 한국인 관광객이 사용할 수 있는 서비스를 목표로 한다.


---


# 2. 핵심 목표


Tokyo Railway Guide는 사용자가 다음 질문에 빠르게 답을 얻을 수 있어야 한다.


1. 이 역은 어떤 노선을 이용할 수 있는가?
2. 다음 열차는 언제 오는가?
3. 어느 방향 열차를 타야 하는가?
4. 어떤 노선으로 환승할 수 있는가?
5. 현재 열차가 정상 운행 중인가?
6. 내가 가진 교통패스로 이 노선을 이용할 수 있는가?
7. 원하는 역을 철도회사와 관계없이 검색할 수 있는가?


---


# 3. Target User


주요 사용자는 다음과 같다.


### Primary


- 도쿄를 여행하는 한국인 관광객
- 일본 철도 시스템에 익숙하지 않은 사용자
- JR / Tokyo Metro / Toei의 차이를 잘 모르는 사용자


### Secondary


- 도쿄 철도 정보를 빠르게 확인하고 싶은 사용자
- 특정 역이나 노선의 실시간 정보를 확인하려는 사용자


---


# 4. 지원 언어


## v1.0


- 한국어
- 일본어 역명 병기


예:


```text
신주쿠
新宿
데이터 모델

가능하면 모든 역과 노선은 다음 세 언어 데이터를 유지한다.

ko
ja
en

영어 UI 전체 지원은 추후 버전에서 고려한다.

5. 지원 철도회사
JR East

도쿄 주요 JR 노선을 지원한다.

기존 웹 프로젝트에서 구현한 노선을 우선적으로 앱으로 이전한다.

Tokyo Metro

다음 9개 노선을 지원한다.

G - 긴자선
M - 마루노우치선
H - 히비야선
T - 도자이선
C - 치요다선
Y - 유라쿠초선
Z - 한조몬선
N - 난보쿠선
F - 후쿠토신선
Toei Subway

다음 4개 노선을 지원한다.

A - 아사쿠사선
I - 미타선
S - 신주쿠선
E - 오에도선
6. 앱 기본 Navigation

예상 앱 구조:

Tokyo Railway Guide
│
├── Home
│
├── Search
│
├── Pass
│
├── Favorites
│
└── Settings

Bottom Tab Navigation 사용을 우선 고려한다.

예:

[ 홈 ] [ 검색 ] [ 패스 ] [ 즐겨찾기 ]
7. Home
요구사항

홈 화면에서는 사용자가 철도회사를 선택할 수 있어야 한다.

JR East
Tokyo Metro
Toei Subway

각 철도회사를 선택하면 해당 회사의 노선 목록을 표시한다.

노선 카드

노선 카드에는 최소한 다음 정보를 표시한다.

노선 코드
노선 색
한국어 노선명
일본어 노선명
주요 운행 구간

예:

G


긴자선
銀座線


시부야 ↔ 아사쿠사
8. Railway Screen

노선을 선택하면 해당 노선의 역 목록을 표시한다.

주요 요구사항:

역 순서 표시
역 코드 표시
한국어 역명
일본어 역명
환승역 표시
노선 색 표시
9. 일반 노선 UI

일반적인 노선은 모바일 환경에서 세로형 노선도를 기본으로 사용한다.

예:

G01
● 시부야
│ 渋谷
│
G02
● 오모테산도
│ 表参道
│
G03
● 가이엔마에
│ 外苑前

모바일 화면에서 가독성과 터치 편의성을 최우선으로 한다.

10. Yamanote Line

야마노테선은 순환선이므로 일반 노선과 별도의 UX를 적용한다.

기본 화면

세로형 역 목록을 사용한다.

목적:

역 이름 가독성 확보
충분한 터치 영역 확보
현재 진행 방향 이해
방향

다음 방향 정보를 명확하게 표시한다.

内回り
외선/내선 방향 정보


外回り

한국어 표현은 실제 서비스 구현 전 다시 검토한다.

전체 노선도

별도의 전체보기 기능에서는 원형 노선도를 제공하는 것을 고려한다.

즉:

기본 화면
→ 세로형


전체 노선도
→ 원형
11. Oedo Line

오에도선은 일반 직선 노선과 다른 구조를 가지고 있으므로
별도의 UX를 적용한다.

실제 지리적 모양을 완벽하게 복제하는 것보다 다음 정보를
사용자가 이해하기 쉽게 표현하는 것을 우선한다.

역 순서
현재 역
다음 역
진행 방향
목적지 방향
UX 원칙

노선도는 실제 철도의 모양을 그대로 복제하는 그림이 아니라,
승객이 다음 행동을 결정할 수 있도록 돕는 인터페이스다.

12. Station Detail

역을 선택하면 Station Detail 화면을 표시한다.

최소 제공 정보:

역 이름


노선 정보


환승 노선


다음 열차


시간표


운행 정보


이용 가능한 교통수단 / 패스
13. Next Train

현재 시간을 기준으로 다음 열차 정보를 표시한다.

최소 표시 정보:

출발 시간
출발까지 남은 시간
방향
목적지
열차 종류
열차 번호 (데이터 제공 시)

예:

다음 열차


12:37
7분 후


보통
니시마고메행
14. Timetable

역별 시간표를 제공한다.

구분:

평일
토요일
휴일

ODPT 데이터 구조에 따라 실제 제공 가능한 Calendar 정보를 사용한다.

15. Train Information

노선의 현재 운행 상태를 표시한다.

예:

● 정상 운행


현재 15분 이상의 지연은 없습니다.

지연 발생 시:

⚠ 운행 정보


야마노테선에 지연이 발생하고 있습니다.

가능하면 다음 정보를 제공한다.

현재 상태
운행 정보 원문
한국어 보조 설명
마지막 업데이트 시간
16. Transfer Information

환승 정보는 앱 전체에서 동일한 데이터와 동일한 UI를 사용한다.

기존 웹 프로젝트처럼 각 철도회사별 화면에서
환승 정보를 개별적으로 구현하지 않는다.

원칙

노선 정보는 공통 Railway Registry에서 관리한다.

예:

E
오에도선
大江戸線
Toei
노선 색

은 앱 전체에서 한 번만 정의한다.

환승이 없는 역

환승 정보가 없는 역은 별도의 Transfer 데이터를 만들지 않는다.

UI에서도 환승 영역을 표시하지 않거나:

환승 노선 없음

으로 처리한다.

환승역

환승 관계가 존재하는 역에서만 환승 데이터를 사용한다.

예:

니혼바시
日本橋


환승


G 긴자선
T 도자이선
17. Transfer Data Source

가능한 환승 정보는 ODPT Station 데이터의 다음 정보를 활용한다.

connectingRailway
connectingStation

ODPT 데이터를 앱에서 직접 표시하지 않고
공통 데이터 형태로 Normalize한다.

예:

odpt.Railway:TokyoMetro.Ginza


↓


metro-ginza


↓


G
긴자선
銀座線
18. Railway Registry

모든 노선 정보는 공통 Registry에서 관리한다.

예:

Railway {
  id
  operator
  code
  nameKo
  nameJa
  nameEn
  color
}

목적:

노선 이름 통일
노선 색 통일
환승 정보 통일
검색 결과 통일
패스 정보 연결
19. Station Search

앱의 핵심 기능 중 하나이다.

사용자는 철도회사를 먼저 선택하지 않고
역 이름을 바로 검색할 수 있어야 한다.

검색 지원:

한국어
일본어
영어

예:

검색


신주쿠

결과:

신주쿠
新宿
Shinjuku


JY 야마노테선
M  마루노우치선
S  신주쿠선
E  오에도선
20. Station Group

동일하거나 공식적으로 연결된 환승역을
Station Group으로 묶는 구조를 고려한다.

주의:

단순히 역 이름이 같다는 이유만으로 자동으로 같은 역으로 판단하지 않는다.

공식적인 환승 관계를 기준으로 연결한다.

예:

StationGroup
    │
    ├── JR Station
    ├── Tokyo Metro Station
    └── Toei Station
21. Favorites

사용자는 다음 항목을 즐겨찾기에 등록할 수 있어야 한다.

역
노선

초기 버전에서는 로컬 저장소 사용을 우선 고려한다.

후속 버전에서 로그인 및 서버 동기화를 검토한다.

22. IC Card Information

Suica / PASMO 정보를 제공한다.

중요:

Suica와 PASMO는 관광용 무제한 패스와 동일한 카테고리로 취급하지 않는다.

UI에서 별도로 구분한다.

예:

IC 카드


✓ Suica
✓ PASMO
23. Tourist Pass

관광객용 교통패스 정보를 제공한다.

초기 지원 대상으로 고려하는 상품:

Tokyo Subway Ticket
Tokyo Metro 24-hour Ticket
기타 필요한 관광 패스

패스 정보는 출시 전에 반드시 공식 운영기관 자료를 기준으로 검증한다.

24. Pass Detail

교통패스를 선택하면 다음 정보를 제공한다.

패스 이름
유효 시간
이용 가능 운영사
이용 가능 노선
이용 불가능 노선
주의사항
정보 기준일

예:

Tokyo Subway Ticket


이용 가능


✓ Tokyo Metro
✓ Toei Subway


이용 불가


✕ JR East
25. My Pass

사용자가 현재 가지고 있는 패스를 선택할 수 있도록 한다.

예:

내 패스


Tokyo Subway Ticket

선택 이후 앱의 노선/역 화면에서:

✓ 현재 패스로 이용 가능

또는

✕ 현재 패스로 이용 불가
별도 운임 필요

를 표시한다.

26. Push Notification

지연 Push Notification 기능을 구현하는 것을 목표로 한다.

사용자는 원하는 노선을 알림 대상으로 선택할 수 있다.

예:

야마노테선


지연 알림
ON

지연 발생 시:

Tokyo Railway Guide


⚠ 야마노테선 운행 정보


야마노테선에 지연이 발생했습니다.
27. Push 중복 방지

서버는 이전 운행 상태와 현재 상태를 비교한다.

예:

NORMAL
   ↓
DELAYED

변경 시 알림을 전송한다.

계속 DELAYED 상태라면 동일한 알림을 반복해서 보내지 않는다.

향후:

DELAYED
   ↓
NORMAL

복구 알림도 고려한다.

28. Backend Requirements

모바일 앱에서 ODPT API를 직접 호출하지 않는 것을 원칙으로 한다.

구조:

Expo App
     │
     │ HTTPS
     ▼
Backend API
     │
 ┌───┴────┐
 ▼        ▼
ODPT      DB

ODPT API Key는 Backend 환경변수에서 관리한다.

절대 Expo 앱 번들에 API Key를 포함하지 않는다.

29. Backend 초기 전략

초기 버전에서는 기존 웹 프로젝트에서 경험한
Next.js Route Handler 기반 API 사용을 우선 고려한다.

필요한 경우 향후 별도의 Backend 서버로 분리한다.

후보:

Next.js API
Spring Boot

v1.0에서는 불필요한 복잡성을 추가하지 않는다.

30. Error Handling

모든 API 기반 화면은 최소 다음 상태를 처리해야 한다.

Loading
열차 정보를 불러오는 중...
Empty
현재 표시할 열차 정보가 없습니다.
Network Error
네트워크 연결을 확인해주세요.


[ 다시 시도 ]
API Error

사용자가 이해할 수 있는 메시지를 표시하고
내부 오류 내용을 그대로 노출하지 않는다.

31. Mobile UX Requirements

앱은 실제 여행 중 사용하는 상황을 고려한다.

원칙:

한 손 조작 가능
충분한 터치 영역
작은 텍스트 최소화
노선 색 적극 활용
역 코드 적극 활용
중요한 정보 우선 표시
과도한 애니메이션 지양
빠른 검색
최소한의 화면 이동
32. Accessibility

가능한 범위에서 다음을 고려한다.

색상만으로 노선을 구분하지 않는다.
노선 코드와 색을 함께 사용한다.
충분한 텍스트 대비를 유지한다.
버튼의 터치 영역을 충분히 확보한다.
Dynamic Type 대응 가능성을 고려한다.
33. Performance

목표:

앱 시작 속도 최적화
불필요한 API 호출 방지
동일 데이터 캐싱
검색 데이터 로컬 활용 검토
실시간 데이터와 정적 데이터를 분리

정적 데이터 예:

노선 이름
역 이름
노선 색
패스 적용 범위

실시간 데이터 예:

열차
지연
운행 정보
다음 열차
34. Security

필수 요구사항:

ODPT API Key 클라이언트 노출 금지
Secret은 서버 환경변수에서 관리
Production/Development 환경 분리
API 입력값 검증
서버 오류 정보의 과도한 클라이언트 노출 방지
35. v1.0 Scope

v1.0의 핵심 기능:

 Expo 프로젝트 구성
 공통 디자인 시스템
 JR East 노선
 Tokyo Metro 9개 노선
 Toei Subway 4개 노선
 일반 노선도
 야마노테선 전용 UX
 오에도선 전용 UX
 역 상세
 통합 역 검색
 공통 환승 정보
 다음 열차
 시간표
 운행 / 지연 정보
 즐겨찾기
 Suica / PASMO 정보
 관광패스 정보
 My Pass
 Loading / Error / Empty UI
 실제 iPhone 테스트
 Android 테스트
36. v1.1 이후

v1.0 안정화 이후 고려:

 지연 Push Notification
 지연 복구 Notification
 사용자별 알림 설정
 Backend DB
 계정
 기기간 즐겨찾기 동기화
 현재 위치
 가까운 역 검색
 영어 UI
 여행 일정과 연계
 추가 철도회사 지원
37. Store Release

개발 완료 후 다음 단계를 목표로 한다.

Development
    ↓
Physical Device Test
    ↓
EAS Build
    ↓
TestFlight
    ↓
Bug Fix
    ↓
App Store Submission

Android 역시 실제 기기 테스트 후 배포 가능성을 검토한다.

38. Definition of Done

v1.0은 단순히 앱이 실행되는 것을 완료로 판단하지 않는다.

다음 조건을 만족해야 한다.

기능

핵심 기능이 실제 기기에서 정상적으로 작동한다.

데이터

JR / Tokyo Metro / Toei의 노선 및 환승 정보가 일관되게 표시된다.

UX

야마노테선과 오에도선을 모바일에서 이해하기 쉽게 사용할 수 있다.

API

ODPT 실시간 데이터를 안정적으로 가져온다.

Security

API Key가 앱에 노출되지 않는다.

Error Handling

네트워크/API 오류에서도 앱이 비정상 종료되지 않는다.

Quality

주요 화면의 디자인과 컴포넌트가 일관된다.

39. Development Principle

이번 프로젝트에서는 개발 속도보다 구조와 이해를 중요하게 생각한다.

개발 순서:

설계
 ↓
구현
 ↓
테스트
 ↓
검증
 ↓
리팩터링
 ↓
다음 기능

기능을 한꺼번에 구현하지 않는다.

공통 구조를 먼저 검증한 뒤 전체 노선으로 확대한다.

특히 처음에는 대표적인 몇 개 역만 사용해서 다음 구조를 검증한다.

일반역
단일 환승역
복수 환승역
다른 철도회사 간 환승역
야마노테선 역
오에도선 역

구조가 검증된 이후 전체 데이터를 추가한다.

40. Project Goal

최종 목표는 단순한 포트폴리오용 철도 앱을 만드는 것이 아니다.

한국인 관광객이 도쿄 여행 중 실제로 사용할 수 있는
철도 정보 서비스를 만드는 것을 목표로 한다.

사용자가 Tokyo Railway Guide 하나로 다음 정보를 이해할 수 있어야 한다.

어디서 타는가
       +
어느 방향인가
       +
언제 오는가
       +
어디서 환승하는가
       +
현재 정상 운행인가
       +
내 교통패스로 탈 수 있는가

Last Updated: 2026-08
Status: Planning
Target: Tokyo Railway Guide Mobile App v1.0
