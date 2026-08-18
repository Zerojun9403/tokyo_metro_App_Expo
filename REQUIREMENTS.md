# Tokyo Railway Guide App - Requirements

> 한국인 관광객을 위한 도쿄 철도 정보 모바일 애플리케이션

---

# 1. Project Overview

## 프로젝트명

**Tokyo Railway Guide**

## 목표 플랫폼

- iOS
- Android

## 주요 기술 스택

### Mobile

- Expo
- React Native
- TypeScript
- Expo Router

### Backend

- 초기: Next.js Route Handler 기반 API 고려
- ODPT API
- 필요 시 PostgreSQL 등 Database 도입
- 향후 필요에 따라 Spring Boot 등 별도 Backend 분리 가능

### Deployment

- EAS Build
- TestFlight
- App Store
- Android 배포 검토
- Backend: Vercel 등

---

# 2. Project Vision

기존에 개발한 다음 웹 프로젝트를 기반으로 한다.

```text
JR East
+
Tokyo Metro
+
Toei Subway
        ↓
Tokyo Railway Guide
```

그러나 기존 웹사이트를 단순히 React Native로 옮기는 것이 목적은 아니다.

모바일 환경에서 실제 도쿄 여행 중 사용할 수 있는 새로운 UX를 설계한다.

최종적인 서비스 방향은:

> 한국인 관광객이 도쿄에서 철도를 이용할 때 필요한 정보를
> 하나의 앱에서 쉽게 확인할 수 있도록 한다.

---

# 3. Core User Questions

앱은 사용자가 다음 질문에 빠르게 답을 얻을 수 있도록 해야 한다.

1. 이 역에는 어떤 노선이 있는가?
2. 다음 열차는 언제 오는가?
3. 어느 방향 열차를 타야 하는가?
4. 어디에서 환승할 수 있는가?
5. 현재 노선은 정상 운행 중인가?
6. 내가 가진 교통패스로 이 노선을 이용할 수 있는가?
7. 원하는 역을 운영회사와 관계없이 검색할 수 있는가?

향후 버전에서는 다음 질문까지 해결하는 것을 목표로 한다.

8. 내 위치에서 가장 가까운 역은 어디인가?
9. 목적지까지 어떻게 이동하면 되는가?
10. 내가 가진 패스를 이용하면 어떤 경로가 유리한가?
11. 이 역 주변에는 어떤 관광지가 있는가?
12. 관광지로 가려면 어느 출구를 이용하는 것이 좋은가?

---

# 4. Target Users

## Primary

- 도쿄를 여행하는 한국인 관광객
- 일본 철도 시스템에 익숙하지 않은 사용자
- JR / Tokyo Metro / Toei의 차이를 잘 모르는 사용자
- 일본어 철도 안내를 이해하기 어려운 사용자

## Secondary

- 도쿄 철도 정보를 빠르게 확인하려는 사용자
- 특정 역 또는 노선의 운행 정보를 확인하려는 사용자

---

# 5. Supported Languages

## v1.0

UI의 기본 언어는 한국어로 한다.

역명 및 노선명에는 일본어를 병기한다.

예:

```text
신주쿠
新宿
```

## Data Model

가능하면 철도회사, 노선, 역 데이터에는 다음 언어를 유지한다.

```text
ko
ja
en
```

예:

```ts
name: {
  ko: "신주쿠",
  ja: "新宿",
  en: "Shinjuku",
}
```

영어 전체 UI는 Future Version에서 검토한다.

---

# 6. Supported Operators

초기 지원 운영사는 다음 세 곳이다.

```text
JR East
Tokyo Metro
Toei Subway
```

---

# 7. JR East

기존 웹 프로젝트에서 구현한 도쿄 주요 JR 노선을 우선 지원한다.

각 노선은 공통 Railway 데이터 구조를 사용한다.

---

# 8. Tokyo Metro

다음 9개 노선을 지원한다.

```text
G  긴자선
M  마루노우치선
H  히비야선
T  도자이선
C  치요다선
Y  유라쿠초선
Z  한조몬선
N  난보쿠선
F  후쿠토신선
```

각 노선의 공식 노선 코드와 노선 색을 사용한다.

---

# 9. Toei Subway

다음 4개 노선을 지원한다.

```text
A  아사쿠사선
I  미타선
S  신주쿠선
E  오에도선
```

---

# 10. Navigation

초기 앱 구조는 다음과 같이 설계한다.

```text
Tokyo Railway Guide
│
├── Home
├── Search
├── Pass
├── Favorites
└── Settings
```

Bottom Tab Navigation을 우선 고려한다.

예:

```text
[ 홈 ] [ 검색 ] [ 패스 ] [ 즐겨찾기 ]
```

필요하다면 Settings는 Home 또는 별도 메뉴에서 접근할 수 있도록 한다.

---

# 11. Home

홈 화면에서는 사용자가 철도 운영사를 선택할 수 있어야 한다.

```text
JR East

Tokyo Metro

Toei Subway
```

운영사를 선택하면 해당 운영사의 노선 목록으로 이동한다.

향후 Location 기능이 추가되면 홈 화면에:

```text
현재 위치 주변 역
```

영역을 추가할 수 있도록 구조를 고려한다.

---

# 12. Railway Card

노선 카드에는 최소한 다음 정보를 표시한다.

- 노선 코드
- 노선 색
- 한국어 노선명
- 일본어 노선명
- 주요 운행 구간

예:

```text
G

긴자선
銀座線

시부야 ↔ 아사쿠사
```

---

# 13. Railway Screen

노선을 선택하면 해당 노선의 역을 순서대로 표시한다.

표시 정보:

- 역 순서
- 역 코드
- 한국어 역명
- 일본어 역명
- 환승 여부
- 노선 색

---

# 14. General Railway UI

일반적인 노선은 모바일 환경에서 세로형 노선도를 기본으로 한다.

예:

```text
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
```

목표:

- 작은 화면에서 높은 가독성
- 충분한 터치 영역
- 역 순서의 직관적인 이해
- 환승역 빠른 식별

---

# 15. Yamanote Line

야마노테선은 순환선이므로 일반 노선과 별도의 UX를 적용한다.

## 기본 화면

모바일 기본 화면에서는 세로형 역 목록을 우선 사용한다.

목적:

- 역명 가독성
- 충분한 터치 영역
- 진행 방향 이해
- 작은 화면 대응

## Direction

내선순환 / 외선순환 방향을 사용자가 쉽게 이해할 수 있도록 표시한다.

일본어 원문도 함께 제공하는 것을 고려한다.

```text
内回り
外回り
```

한국어 표현은 구현 전 정확한 철도 용어를 다시 검토한다.

## Full Railway Map

별도의 전체보기에서는 원형 노선도를 제공하는 것을 고려한다.

```text
기본
→ 세로형 노선도

전체보기
→ 원형 노선도
```

---

# 16. Oedo Line

오에도선은 일반적인 직선형 노선과 다른 구조이므로 별도의 UX를 적용한다.

실제 지리적 형태를 완벽하게 복제하는 것보다 사용자가 다음 정보를 이해하는 것을 우선한다.

- 역 순서
- 현재 역
- 다음 역
- 진행 방향
- 목적지 방향

## UX Principle

> 노선도는 실제 철도의 형태를 그대로 복제하는 그림이 아니라
> 승객이 다음 행동을 결정할 수 있도록 돕는 인터페이스다.

---

# 17. Station Detail

역을 선택하면 Station Detail 화면을 표시한다.

최소 제공 정보:

```text
역 이름

역 코드

노선

환승 노선

다음 열차

시간표

운행 정보

교통패스 이용 여부
```

향후 버전에서는:

```text
현재 위치와의 거리
주변 관광지
추천 출구
```

등을 추가할 수 있도록 한다.

---

# 18. Next Train

현재 시간을 기준으로 다음 열차 정보를 표시한다.

최소 표시 정보:

- 출발 시간
- 남은 시간
- 방향
- 목적지
- 열차 종류
- 열차 번호 (제공되는 경우)

예:

```text
다음 열차

12:37
7분 후

보통
니시마고메행
```

---

# 19. Timetable

역별 시간표를 제공한다.

가능한 경우 다음 Calendar를 구분한다.

```text
평일
토요일
휴일
```

실제 구현은 ODPT에서 제공하는 Calendar 데이터를 기준으로 한다.

---

# 20. Train Information

현재 노선의 운행 상태를 표시한다.

정상:

```text
● 정상 운행

현재 15분 이상의 지연은 없습니다.
```

지연:

```text
⚠ 운행 정보

현재 해당 노선에 지연이 발생하고 있습니다.
```

가능하면 다음 정보를 제공한다.

- 현재 상태
- 운행 정보
- 일본어 원문
- 한국어 보조 설명
- 업데이트 시각

---

# 21. Common Railway Registry

모든 노선 정보는 공통 Railway Registry에서 관리한다.

예:

```ts
type Railway = {
  id: string;
  operatorId: string;

  code: string;

  name: {
    ko: string;
    ja: string;
    en: string;
  };

  color: string;
};
```

예:

```text
toei-oedo

E
오에도선
大江戸線
Oedo Line
```

은 앱 전체에서 한 번만 정의한다.

목적:

- 노선명 통일
- 노선 색 통일
- 노선 코드 통일
- 환승 UI 통일
- 검색 결과 통일
- 패스 데이터 연결

---

# 22. Station Data

Station은 각 노선에 속한 실제 역 데이터를 나타낸다.

기본 구조 예:

```ts
type Station = {
  id: string;
  railwayId: string;

  code: string;

  name: {
    ko: string;
    ja: string;
    en: string;
  };

  order: number;

  latitude?: number;
  longitude?: number;
};
```

## Important

v1.0에서 GPS 기능을 사용하지 않더라도 가능하면:

```text
latitude
longitude
```

를 보존한다.

향후 가까운 역 기능에서 사용하기 위함이다.

또한:

```text
order
```

를 유지하여 향후 경로 탐색 시 역 연결 관계를 구성할 수 있도록 한다.

---

# 23. Transfer Information

환승 정보는 앱 전체에서 동일한 데이터와 동일한 UI를 사용한다.

기존 웹 프로젝트처럼 운영사/노선마다 환승 정보를 개별적으로 표현하지 않는다.

## Transfer Station

예:

```text
신주쿠
新宿

환승 노선

JY  야마노테선
M   마루노우치선
S   신주쿠선
E   오에도선
```

어떤 화면에서 신주쿠역을 열어도 동일한 기준으로 표시되어야 한다.

---

# 24. Station Without Transfer

환승 노선이 없는 역은 별도의 Transfer 관계를 만들 필요가 없다.

예:

```text
마고메
馬込
```

환승 관계가 없다면:

```text
transfer = undefined
```

또는 빈 배열로 처리한다.

UI에서는 환승 영역을 숨기거나:

```text
환승 노선 없음
```

으로 표시한다.

---

# 25. Transfer Data Source

가능한 경우 ODPT Station 데이터의:

```text
connectingRailway
connectingStation
```

정보를 활용한다.

ODPT 원본 ID를 화면에 직접 사용하지 않는다.

예:

```text
odpt.Railway:Toei.Oedo
        ↓
Normalize
        ↓
toei-oedo
        ↓
Railway Registry
        ↓
E  오에도선
```

---

# 26. Station Group

동일하거나 공식적으로 환승 연결된 역들을 Station Group으로 관리하는 구조를 고려한다.

예:

```text
StationGroup
     │
     ├── JR Station
     ├── Tokyo Metro Station
     └── Toei Station
```

## Important

역 이름이 같다는 이유만으로 자동으로 하나의 역으로 합치지 않는다.

또한 역 이름이 다르다는 이유만으로 별개의 환승역이라고 판단하지 않는다.

공식적인 환승 관계 및 실제 철도 데이터를 기준으로 연결한다.

---

# 27. Station Search

앱의 핵심 기능이다.

사용자는 운영사를 먼저 선택하지 않고 역 이름을 바로 검색할 수 있어야 한다.

검색 언어:

- 한국어
- 일본어
- 영어

예:

```text
검색

신주쿠
```

결과:

```text
신주쿠
新宿
Shinjuku

JY  야마노테선
M   마루노우치선
S   신주쿠선
E   오에도선
```

---

# 28. Favorites

사용자는 다음 항목을 즐겨찾기에 등록할 수 있다.

- 역
- 노선

초기 버전에서는 로컬 저장소 사용을 우선한다.

로그인 및 서버 동기화는 Future Version에서 검토한다.

---

# 29. IC Card Information

Suica / PASMO 정보를 제공한다.

중요:

Suica/PASMO와 관광용 무제한 패스는 다른 성격이므로 UI에서 구분한다.

예:

```text
IC CARD

Suica
PASMO
```

---

# 30. Tourist Pass

관광객용 철도 패스 정보를 제공한다.

초기 대상으로 고려:

```text
Tokyo Subway Ticket

Tokyo Metro 24-hour Ticket
```

필요에 따라 다른 관광패스를 추가한다.

## Data Accuracy

패스 적용 범위, 가격, 유효시간 등의 정보는 출시 시점에 반드시 운영기관 공식 자료를 기준으로 검증한다.

가능하면 다음 정보를 표시한다.

```text
정보 기준일
```

---

# 31. Pass Detail

교통패스를 선택하면 다음 정보를 제공한다.

- 이름
- 유효시간
- 이용 가능 운영사
- 이용 가능 노선
- 이용 불가능 노선
- 주의사항
- 정보 기준일

예:

```text
Tokyo Subway Ticket

이용 가능

✓ Tokyo Metro
✓ Toei Subway

이용 불가

✕ JR East
```

---

# 32. My Pass

사용자가 현재 보유한 패스를 선택할 수 있도록 한다.

예:

```text
내 패스

Tokyo Subway Ticket
```

이후 Railway / Station 화면에서:

```text
✓ 현재 패스로 이용 가능
```

또는:

```text
✕ 현재 패스로 이용 불가
별도 운임 필요
```

를 표시할 수 있다.

---

# 33. Backend

ODPT API Key를 Expo 앱에서 직접 사용하지 않는다.

기본 구조:

```text
Expo App
     │
     │ HTTPS
     ▼
Backend API
     │
     ├── ODPT
     │
     └── Database
```

API Key는 Backend 환경변수에서 관리한다.

---

# 34. Backend Initial Strategy

초기 버전에서는 기존 웹 프로젝트에서 사용한 경험을 활용하여:

```text
Next.js Route Handler
```

기반 API를 우선 고려한다.

서비스 규모가 커지거나 Backend 기능이 복잡해지면 별도 서버로 분리할 수 있다.

후보:

```text
Spring Boot
```

단, v1.0부터 불필요한 복잡성을 도입하지 않는다.

---

# 35. Error Handling

모든 API 화면은 다음 상태를 처리해야 한다.

## Loading

```text
열차 정보를 불러오는 중...
```

## Empty

```text
현재 표시할 정보가 없습니다.
```

## Network Error

```text
네트워크 연결을 확인해주세요.

[ 다시 시도 ]
```

## API Error

내부 서버 오류 내용을 그대로 사용자에게 노출하지 않는다.

---

# 36. Mobile UX Requirements

실제 여행 중 한 손으로 사용하는 상황을 고려한다.

원칙:

- 충분한 터치 영역
- 작은 텍스트 최소화
- 노선 색 적극 활용
- 노선 코드 적극 활용
- 중요한 정보 우선
- 빠른 역 검색
- 최소한의 화면 이동
- 과도한 애니메이션 지양
- 모바일 화면에 맞는 정보 밀도 유지

---

# 37. Accessibility

가능한 범위에서 다음을 고려한다.

- 색상만으로 노선을 구분하지 않는다.
- 노선 코드 + 색상을 함께 사용한다.
- 충분한 텍스트 대비
- 충분한 터치 영역
- Dynamic Type 대응 고려

---

# 38. Performance

정적 데이터와 실시간 데이터를 분리한다.

## Static

```text
운영사
노선명
노선 색
역명
역 순서
역 좌표
환승 관계
교통패스 적용 범위
```

## Realtime

```text
열차 위치
다음 열차
시간표
지연
운행 정보
```

목표:

- 불필요한 API 호출 방지
- 캐싱 가능한 데이터 구분
- 검색 데이터의 로컬 활용
- 네트워크가 느린 환경 고려

---

# 39. Security

필수 요구사항:

- ODPT API Key 클라이언트 노출 금지
- Secret은 서버 환경변수에서 관리
- Production / Development 환경 분리
- API 입력값 검증
- 서버 내부 오류 정보 노출 방지

---

# 40. v1.0 - Railway Core

v1.0의 목표는 **철도 정보 앱의 핵심을 완성하는 것**이다.

## Must Have

- [ ] Expo 프로젝트 구성
- [ ] Expo Router
- [ ] 공통 디자인 시스템
- [ ] Railway Registry
- [ ] Station 데이터 모델
- [ ] Transfer 데이터 모델
- [ ] JR East 지원
- [ ] Tokyo Metro 9개 노선
- [ ] Toei Subway 4개 노선
- [ ] 일반 노선도
- [ ] 야마노테선 전용 UX
- [ ] 오에도선 전용 UX
- [ ] 역 상세
- [ ] 통합 역 검색
- [ ] 공통 환승 정보
- [ ] 다음 열차
- [ ] 시간표
- [ ] 운행 / 지연 정보
- [ ] 즐겨찾기
- [ ] Suica / PASMO 정보
- [ ] 관광패스
- [ ] My Pass
- [ ] Loading / Error / Empty State
- [ ] 실제 iPhone 테스트
- [ ] Android 테스트

---

# 41. v1.1 - Notification

v1.0 안정화 이후 Push Notification을 구현한다.

## Features

- [ ] 지연 Push Notification
- [ ] 사용자별 노선 알림 설정
- [ ] Push Token 관리
- [ ] 운행 상태 저장
- [ ] 중복 지연 알림 방지
- [ ] 지연 복구 알림 검토

예:

```text
NORMAL
   ↓
DELAYED

→ Push 발송
```

계속 DELAYED 상태라면 같은 알림을 반복 발송하지 않는다.

이를 위해 Backend 및 Database 사용을 검토한다.

---

# 42. v1.5 - Location

스마트폰의 GPS 기능을 활용한다.

## Features

- [ ] 위치 권한 요청
- [ ] 현재 위치 확인
- [ ] 현재 위치에서 가까운 역 계산
- [ ] 가까운 역 거리 표시
- [ ] 가까운 역 Station Detail 연결

예:

```text
현재 위치 주변

신주쿠역
180m
JY  M  S  E

신주쿠니시구치역
420m
E
```

## Implementation Principle

역 데이터에 저장된:

```text
latitude
longitude
```

와 사용자 GPS 좌표 사이의 거리를 계산한다.

이 기능을 v1.0에서 구현하지 않더라도 Station 데이터에는 가능한 한 좌표를 보존한다.

---

# 43. v2.0 - Route Navigation

출발역과 도착역을 이용한 철도 경로 탐색 기능을 개발한다.

## Features

- [ ] 출발역 선택
- [ ] 도착역 선택
- [ ] 철도 네트워크 그래프 구성
- [ ] 환승 경로 계산
- [ ] 예상 이동시간
- [ ] 환승 횟수
- [ ] 경로 후보 표시

예:

```text
신주쿠
   ↓
마루노우치선
   ↓
아카사카미쓰케
   ↓
긴자선
   ↓
아사쿠사
```

## Future Considerations

단순한 연결 경로뿐만 아니라 다음 요소를 고려할 수 있다.

```text
열차 소요시간
대기시간
환승시간
급행 / 각역정차
직통운전
운행 시간
운임
실시간 지연
```

v2.0의 정확한 범위는 구현 전 별도로 정의한다.

---

# 44. v2.5 - Smart Pass Navigation

경로 탐색과 My Pass 기능을 결합한다.

목표:

> 사용자가 가지고 있는 교통패스를 고려한 이동 경로를 제공한다.

예:

```text
신주쿠 → 아사쿠사
```

결과 예:

```text
가장 빠른 경로

32분
추가 운임 발생
```

또는:

```text
내 패스로 이용 가능한 경로

Tokyo Subway Ticket

37분
추가 운임 ¥0
```

## Possible Features

- [ ] 패스 적용 가능 경로
- [ ] 추가 운임 발생 구간
- [ ] 패스 이용 불가능 노선 표시
- [ ] 가장 빠른 경로
- [ ] 패스 우선 경로
- [ ] 비용 우선 경로

이 기능은 일반 경로 탐색이 안정화된 이후 구현한다.

---

# 45. v3.0 - Travel

철도 정보와 관광 정보를 연결한다.

## Features

- [ ] 역 주변 관광지
- [ ] 관광지와 가까운 역
- [ ] 관광지와 가까운 출구
- [ ] 역에서 관광지까지 거리
- [ ] 관광지 기본 정보

예:

```text
아사쿠사
浅草

주변 관광지

센소지
도보 약 6분

가미나리몬
도보 약 5분

도쿄 스카이트리
약 1.5km
```

단순 관광지 목록보다 철도 이용자에게 필요한 정보를 우선한다.

예:

```text
센소지 방문

추천 역
아사쿠사

추천 출구
A1
```

---

# 46. Future Roadmap

전체 프로젝트의 장기적인 방향은 다음과 같다.

```text
v1.0
Railway Core
│
├── 노선
├── 역
├── 검색
├── 환승
├── 실시간 정보
└── 교통패스
        │
        ▼
v1.1
Notification
│
└── 지연 Push
        │
        ▼
v1.5
Location
│
├── GPS
└── 가까운 역
        │
        ▼
v2.0
Navigation
│
├── 출발역
├── 도착역
└── 경로 탐색
        │
        ▼
v2.5
Smart Pass
│
├── 패스 기반 경로
└── 추가 운임 판단
        │
        ▼
v3.0
Travel
│
├── 관광지
├── 추천 역
└── 추천 출구
```

---

# 47. Out of Scope for v1.0

다음 기능은 구현하고 싶은 기능이지만 **v1.0에서는 의도적으로 제외한다.**

```text
GPS 현재 위치
가까운 역
출발역 → 도착역 경로 탐색
최적 경로 계산
실시간 경로 재탐색
패스를 고려한 경로 탐색
역 주변 관광지
추천 출구
관광지 검색
사용자 계정
클라우드 즐겨찾기 동기화
```

이 기능들은 기술적으로 포기한 기능이 아니다.

v1.0의 범위를 통제하고 핵심 철도 기능의 완성도를 확보하기 위해 후속 버전으로 분리한다.

---

# 48. Future-Proof Data Requirements

v1.0에서 사용하지 않는 데이터라도 향후 기능에 필요한 데이터는 가능한 범위에서 보존한다.

## Station

```text
id
railwayId
stationCode
nameKo
nameJa
nameEn

order

latitude
longitude
```

### 사용 목적

```text
order
→ 향후 경로 탐색

latitude / longitude
→ 향후 가까운 역
```

---

## Railway

향후 경로 탐색을 위해 노선과 역의 관계가 명확해야 한다.

```text
Railway
    │
    ├── Station 01
    ├── Station 02
    ├── Station 03
    └── ...
```

---

## Transfer

향후 그래프 탐색을 위해 환승 관계를 명확하게 유지한다.

```text
Station
   │
   └── Transfer
          │
          └── Station
```

---

# 49. Scope Management

새로운 아이디어가 생겼다고 즉시 v1.0에 추가하지 않는다.

먼저 다음 중 하나로 분류한다.

```text
v1.0
v1.1
v1.5
v2.0
v2.5
v3.0
Backlog
```

새로운 기능이 v1.0에 반드시 필요한 경우:

1. 필요성 검토
2. 기존 요구사항 영향 분석
3. 예상 공수 계산
4. ESTIMATION.md 수정
5. 구현

순서로 진행한다.

---

# 50. Development Strategy

이번 프로젝트에서는 다음 순서를 지킨다.

```text
Requirements
     ↓
Data Model
     ↓
Architecture
     ↓
Prototype
     ↓
Implementation
     ↓
Testing
     ↓
Refactoring
     ↓
Release
```

기능을 한꺼번에 구현하지 않는다.

---

# 51. Prototype Strategy

전체 데이터를 입력하기 전에 대표 역을 이용하여 구조를 먼저 검증한다.

필요한 Test Case:

```text
일반역

단일 환승역

복수 환승역

JR ↔ Metro 환승역

Metro ↔ Toei 환승역

JR ↔ Metro ↔ Toei 복합 환승역

야마노테선 역

오에도선 역
```

이 구조가 정상적으로 동작한 후 전체 역 데이터를 확장한다.

---

# 52. Data Principle

프로젝트의 중요한 원칙:

> 같은 정보는 가능한 한 한 곳에서 관리한다.

예:

```text
오에도선

❌

JR 데이터에 오에도선 정보
Metro 데이터에 오에도선 정보
Toei 데이터에 오에도선 정보


⭕️

Railway Registry

toei-oedo
   ↓
E
오에도선
大江戸線
Oedo Line
```

다른 기능에서는 해당 ID를 참조한다.

이를 통해:

- 노선명 불일치
- 색상 불일치
- 환승정보 누락
- 화면별 디자인 차이

를 줄인다.

---

# 53. Data Source Principle

데이터는 성격에 따라 출처를 구분한다.

## Railway / Station / Realtime

가능하면 ODPT 등 신뢰할 수 있는 철도 데이터를 사용한다.

## Fare / Pass

운영기관 공식 자료를 기준으로 검증한다.

## Tourist Information

향후 관광 기능 구현 시 신뢰할 수 있는 공식 또는 검증 가능한 데이터를 사용한다.

중요 정보는 AI의 기억만으로 데이터베이스에 입력하지 않는다.

---

# 54. Definition of Done - v1.0

다음 조건을 만족해야 v1.0을 완료한 것으로 판단한다.

## Railway

- 주요 지원 노선 정상 표시
- 역 순서 정상
- 노선 코드 정상
- 노선 색 일관성 확보

## Station

- 역명 정상 표시
- 역 상세 정상 작동
- 한국어/일본어 정상 표시

## Transfer

- JR / Metro / Toei 간 환승정보 일관성 확보
- 동일 환승 노선이 화면마다 다르게 표시되지 않음

## Search

- 한국어 역 검색
- 일본어 역 검색
- 영어 역 검색

## Realtime

- 다음 열차 정상 표시
- 시간표 정상 표시
- 운행정보 정상 표시

## Pass

- 패스 정보 정상 표시
- My Pass 이용 가능 여부 정상 표시

## UX

- 야마노테선 모바일 UX 검증
- 오에도선 모바일 UX 검증
- 주요 화면 디자인 통일

## Security

- ODPT API Key 클라이언트 미노출

## Stability

- Network Error 처리
- API Error 처리
- Empty State 처리
- 앱 비정상 종료 없음

## Device

- 실제 iPhone 테스트
- Android 테스트

---

# 55. Release Strategy

```text
Development
     ↓
Internal Test
     ↓
Physical Device Test
     ↓
Bug Fix
     ↓
EAS Build
     ↓
TestFlight
     ↓
Release Candidate
     ↓
App Store Submission
```

앱스토어 등록 여부와 관계없이 TestFlight에서 실제 설치 가능한 빌드를 만드는 것을 중요한 목표로 한다.

---

# 56. Final Product Direction

Tokyo Railway Guide의 장기적인 발전 방향은:

```text
Railway Information
        ↓
Realtime Railway
        ↓
Tourist Pass
        ↓
Location
        ↓
Navigation
        ↓
Smart Pass Navigation
        ↓
Travel Assistant
```

이다.

그러나 각 단계를 한 번에 구현하지 않는다.

현재 가장 중요한 목표는:

> **v1.0 Railway Core를 안정적으로 완성하는 것**

이다.

---

# 57. Project Philosophy

이 프로젝트에서는 개발 속도만을 목표로 하지 않는다.

```text
빠르게 많은 기능 구현
          ❌

구조를 이해하고
작게 구현하고
실제 기기에서 검증하고
다음 단계로 확장
          ⭕
```

최종적으로 프로젝트 개발자가 다음 내용을 직접 설명할 수 있어야 한다.

```text
왜 이런 데이터 구조를 선택했는가?

왜 환승정보를 공통 관리하는가?

왜 야마노테선과 오에도선 UI가 다른가?

왜 API Key를 Backend에서 관리하는가?

왜 GPS와 경로검색을 v1에서 제외했는가?

향후 경로검색을 위해 어떤 데이터를 보존했는가?

패스 정보와 철도 데이터를 어떻게 연결하는가?
```

---

**Last Updated:** 2026-08-18  
**Status:** Planning  
**Requirements Version:** 1.1  
**Target:** Tokyo Railway Guide Mobile App v1.0
