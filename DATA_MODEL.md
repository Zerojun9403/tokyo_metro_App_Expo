# Tokyo Railway Guide App - Data Model

> Tokyo Railway Guide Mobile App의 공통 철도 데이터 모델 설계 문서

---

# 1. 목적

본 문서는 Tokyo Railway Guide에서 사용하는 철도 데이터를
일관된 형태로 관리하기 위한 기준을 정의한다.

기존 웹 프로젝트에서는 JR East, Tokyo Metro, Toei Subway를
각각 독립적으로 개발하면서 다음과 같은 문제가 발생했다.

- 같은 노선의 이름이 화면마다 다르게 표시될 수 있음
- 환승 노선 일부가 누락될 수 있음
- 노선별 UI에서 환승 데이터를 중복 관리함
- 동일한 역 정보를 여러 파일에서 반복 정의함
- 프로젝트별 데이터 구조가 조금씩 달라짐

모바일 앱에서는 이를 해결하기 위해 공통 데이터 모델을 사용한다.

핵심 원칙:

> 같은 정보는 가능한 한 한 곳에서 정의하고 ID를 통해 참조한다.

---

# 2. 전체 데이터 관계

앱의 기본 데이터 구조는 다음과 같다.

```text
Operator
   │
   └── Railway
          │
          └── RailwayStation
                    │
                    ▼
                 Station
                    │
                    ├── StationGroup
                    │
                    └── Transfer

FareProduct
     │
     ├── Operator
     └── Railway
```

향후에는 이 구조를 기반으로:

```text
Station
   │
   ├── GPS
   ├── Route Graph
   ├── Tourist Spot
   └── Exit
```

등을 확장할 수 있다.

---

# 3. ID 규칙

앱 내부에서는 ODPT ID를 그대로 Primary ID로 사용하지 않는다.

앱에서 사용할 안정적인 내부 ID를 별도로 정의한다.

예:

```text
Operator

jr-east
tokyo-metro
toei
```

Railway:

```text
jr-yamanote
metro-ginza
metro-marunouchi
toei-asakusa
toei-oedo
```

Station:

```text
shinjuku
shibuya
nihombashi
asakusa
```

RailwayStation:

```text
jr-yamanote-shinjuku
metro-marunouchi-shinjuku
toei-oedo-shinjuku
```

ODPT ID는 외부 데이터 연결용으로 별도 보존한다.

---

# 4. MultilingualText

한국어 / 일본어 / 영어를 공통 구조로 관리한다.

```ts
export type MultilingualText = {
  ko: string;
  ja: string;
  en: string;
};
```

예:

```ts
const shinjuku = {
  ko: "신주쿠",
  ja: "新宿",
  en: "Shinjuku",
};
```

이 구조는:

- Station
- Railway
- Operator
- FareProduct

등에서 공통으로 사용한다.

---

# 5. Operator

철도 운영사를 나타낸다.

```ts
export type Operator = {
  id: string;

  name: MultilingualText;

  odptId?: string;
};
```

예:

```ts
export const TOEI: Operator = {
  id: "toei",

  name: {
    ko: "도에이 지하철",
    ja: "都営地下鉄",
    en: "Toei Subway",
  },

  odptId: "odpt.Operator:Toei",
};
```

초기 Operator:

```text
jr-east
tokyo-metro
toei
```

---

# 6. Railway

철도 노선을 나타낸다.

```ts
export type Railway = {
  id: string;

  operatorId: string;

  code: string;

  name: MultilingualText;

  color: string;

  odptId?: string;
};
```

예:

```ts
export const OEDO_LINE: Railway = {
  id: "toei-oedo",

  operatorId: "toei",

  code: "E",

  name: {
    ko: "오에도선",
    ja: "大江戸線",
    en: "Oedo Line",
  },

  color: "#B6007A",

  odptId: "odpt.Railway:Toei.Oedo",
};
```

---

# 7. Railway Registry

모든 노선의 기본 정보는 공통 Registry에서 관리한다.

```ts
export const RAILWAYS: Record<string, Railway> = {
  "toei-oedo": {
    id: "toei-oedo",
    operatorId: "toei",
    code: "E",

    name: {
      ko: "오에도선",
      ja: "大江戸線",
      en: "Oedo Line",
    },

    color: "#B6007A",
    odptId: "odpt.Railway:Toei.Oedo",
  },

  "toei-asakusa": {
    id: "toei-asakusa",
    operatorId: "toei",
    code: "A",

    name: {
      ko: "아사쿠사선",
      ja: "浅草線",
      en: "Asakusa Line",
    },

    color: "#E85298",
    odptId: "odpt.Railway:Toei.Asakusa",
  },
};
```

앱의 다른 데이터에서:

```text
오에도선
大江戸線
#B6007A
```

등을 반복해서 작성하지 않는다.

대신:

```ts
railwayId: "toei-oedo"
```

만 저장한다.

---

# 8. Station

Station은 물리적 또는 논리적인 역 자체를 나타낸다.

```ts
export type Station = {
  id: string;

  name: MultilingualText;

  latitude?: number;
  longitude?: number;
};
```

예:

```ts
export const SHINJUKU: Station = {
  id: "shinjuku",

  name: {
    ko: "신주쿠",
    ja: "新宿",
    en: "Shinjuku",
  },

  latitude: 35.6896,
  longitude: 139.7006,
};
```

주의:

좌표는 예시 구조이며 실제 데이터 입력 시 공식/검증된 데이터를 사용한다.

---

# 9. 왜 Station과 RailwayStation을 분리하는가?

신주쿠역을 예로 들면 하나의 "신주쿠"라는 장소에 여러 노선이 존재한다.

```text
신주쿠
  │
  ├── JR 야마노테선
  ├── Tokyo Metro 마루노우치선
  ├── Toei 신주쿠선
  └── Toei 오에도선
```

각 노선에서는 서로 다른 역 코드가 존재한다.

따라서:

```text
Station
```

과:

```text
RailwayStation
```

을 분리한다.

---

# 10. RailwayStation

특정 노선에 속한 역을 나타낸다.

```ts
export type RailwayStation = {
  id: string;

  stationId: string;
  railwayId: string;

  stationCode: string;

  order: number;

  odptId?: string;
};
```

예:

```ts
{
  id: "toei-oedo-shinjuku",

  stationId: "shinjuku",
  railwayId: "toei-oedo",

  stationCode: "E27",

  order: 27,

  odptId: "...",
}
```

---

# 11. order가 중요한 이유

현재 v1에서는 역을 순서대로 표시하기 위해 사용한다.

```text
01
 ↓
02
 ↓
03
 ↓
04
```

하지만 향후 Route Navigation에서도 활용할 수 있다.

예:

```text
Station A
   ↓
Station B
   ↓
Station C
```

즉:

```ts
order: number;
```

는 화면 정렬뿐 아니라 향후 철도 그래프 생성에도 활용할 수 있다.

---

# 12. StationGroup

주의가 필요한 데이터다.

모든 동일 이름 역을 자동으로 하나로 합치지 않는다.

실제 철도에서 공식적으로 연결되거나 하나의 환승 거점으로 취급할 수 있는 역들을 그룹으로 묶는다.

```ts
export type StationGroup = {
  id: string;

  name: MultilingualText;

  stationIds: string[];
};
```

예:

```ts
{
  id: "shinjuku-group",

  name: {
    ko: "신주쿠",
    ja: "新宿",
    en: "Shinjuku",
  },

  stationIds: [
    "shinjuku",
  ],
}
```

향후 서로 다른 이름의 공식 환승역을 처리할 때도 확장할 수 있다.

---

# 13. Transfer

Transfer는 환승 관계를 나타낸다.

```ts
export type Transfer = {
  fromRailwayStationId: string;
  toRailwayStationId: string;

  official: boolean;

  walkingMinutes?: number;
};
```

예:

```ts
{
  fromRailwayStationId: "metro-marunouchi-shinjuku",
  toRailwayStationId: "toei-oedo-shinjuku",

  official: true,
}
```

---

# 14. 환승 데이터 관리 원칙

환승정보를 각 노선 파일에 직접 작성하지 않는다.

잘못된 예:

```text
yamanote.ts

transfers:
- Marunouchi
- Shinjuku
- Oedo
```

그리고:

```text
marunouchi.ts

transfers:
- Yamanote
- Shinjuku
```

처럼 관리하면 오에도선 하나를 누락할 수 있다.

---

# 15. 올바른 환승 구조

공통 데이터에서 관계를 관리한다.

```text
Station / StationGroup
          │
          ├── JR Yamanote
          ├── Metro Marunouchi
          ├── Toei Shinjuku
          └── Toei Oedo
```

UI에서는 이 관계를 이용해 환승 노선을 계산한다.

따라서 어느 화면에서 접근하더라도 같은 결과를 얻을 수 있다.

---

# 16. 환승이 없는 역

환승이 없는 역에는 Transfer 데이터를 만들지 않는다.

예:

```text
Station
마고메

RailwayStation
A02
```

Transfer:

```text
없음
```

UI:

```text
환승 영역 숨김
```

또는 필요하면:

```text
환승 노선 없음
```

으로 표시한다.

---

# 17. ODPT Adapter

ODPT 데이터 구조를 앱 전체에서 직접 사용하지 않는다.

중간 Adapter / Normalization 계층을 둔다.

```text
ODPT API

odpt.Railway:Toei.Oedo

        ↓

ODPT Adapter

        ↓

toei-oedo

        ↓

Railway Registry

        ↓

E
오에도선
大江戸線
```

이렇게 하면 UI가 ODPT의 ID 규칙에 직접 의존하지 않는다.

---

# 18. ODPT Mapping

ODPT ID와 내부 ID를 연결하는 Mapping을 관리한다.

예:

```ts
export const ODPT_RAILWAY_MAP: Record<string, string> = {
  "odpt.Railway:Toei.Oedo": "toei-oedo",

  "odpt.Railway:Toei.Asakusa": "toei-asakusa",

  "odpt.Railway:TokyoMetro.Ginza": "metro-ginza",
};
```

---

# 19. Static Data와 Realtime Data 분리

철도 데이터는 크게 두 종류로 나눈다.

## Static

자주 변하지 않는 데이터:

```text
Operator
Railway
Station
RailwayStation
StationGroup
Transfer
FareProduct
```

## Realtime

계속 변하는 데이터:

```text
Train
TrainInformation
Next Train
Delay
Timetable
```

UI에서는 두 데이터를 결합한다.

---

# 20. Train

실시간 열차 데이터의 공통 형태를 정의한다.

```ts
export type Train = {
  id: string;

  railwayId: string;

  trainNumber?: string;

  fromStationId?: string;
  toStationId?: string;

  destinationStationIds: string[];

  direction?: string;

  trainType?: string;

  delaySeconds: number;

  updatedAt?: string;
};
```

ODPT 응답은 Backend/Adapter에서 이 구조로 변환한다.

---

# 21. TrainInformation

운행정보 공통 모델:

```ts
export type TrainInformation = {
  railwayId: string;

  status:
    | "normal"
    | "delayed"
    | "suspended"
    | "unknown";

  text: {
    ko?: string;
    ja?: string;
    en?: string;
  };

  updatedAt?: string;
  validUntil?: string;
};
```

예:

```ts
{
  railwayId: "toei-asakusa",

  status: "normal",

  text: {
    ko: "현재 15분 이상의 지연은 없습니다.",
    ja: "現在、１５分以上の遅延はありません。",
  },

  updatedAt: "2026-08-17T12:39:00+09:00",
}
```

---

# 22. FareProduct

IC 카드 또는 관광객용 교통패스를 나타낸다.

```ts
export type FareProduct = {
  id: string;

  name: MultilingualText;

  type:
    | "ic-card"
    | "day-pass"
    | "tourist-pass";

  validOperatorIds: string[];

  validRailwayIds?: string[];

  durationMinutes?: number;

  description?: MultilingualText;

  restrictions?: string[];

  verifiedAt?: string;
};
```

---

# 23. FareProduct Example

예:

```ts
{
  id: "tokyo-subway-ticket",

  name: {
    ko: "도쿄 서브웨이 티켓",
    ja: "Tokyo Subway Ticket",
    en: "Tokyo Subway Ticket",
  },

  type: "tourist-pass",

  validOperatorIds: [
    "tokyo-metro",
    "toei",
  ],

  verifiedAt: "2026-XX-XX",
}
```

실제 적용 범위와 상품 조건은 출시 전에 공식 자료로 검증한다.

---

# 24. Favorite

초기에는 서버 계정 없이 로컬에 저장한다.

```ts
export type Favorite = {
  id: string;

  type:
    | "station"
    | "railway";

  targetId: string;

  createdAt: string;
};
```

---

# 25. My Pass

사용자가 현재 사용 중인 패스를 저장한다.

초기 버전에서는 로컬 저장소 사용을 고려한다.

```ts
export type UserPass = {
  fareProductId: string;

  activatedAt?: string;

  expiresAt?: string;
};
```

---

# 26. Future GPS Support

v1.0에서는 GPS 기능을 구현하지 않는다.

그러나 Station에:

```ts
latitude?: number;
longitude?: number;
```

를 유지한다.

향후:

```text
사용자 위치
    ↓
Station 좌표
    ↓
거리 계산
    ↓
가까운 역 정렬
```

에 사용한다.

---

# 27. Future Route Navigation

v2에서는 RailwayStation 관계를 그래프로 변환할 수 있다.

예:

```text
G01
 │
G02
 │
G03
 │
G04
```

각 역은 Node가 되고:

```text
인접 역
환승역
```

관계는 Edge가 된다.

예:

```text
Station A
   │
   │ Railway Edge
   ▼
Station B
   │
   │ Transfer Edge
   ▼
Station C
```

---

# 28. Route Graph

향후 다음 형태로 확장할 수 있다.

```ts
export type RouteEdge = {
  from: string;
  to: string;

  type:
    | "railway"
    | "transfer";

  railwayId?: string;

  travelMinutes?: number;
  walkingMinutes?: number;
};
```

v1.0에서는 구현하지 않는다.

단, 이를 만들 수 있도록 현재 데이터의 관계를 보존한다.

---

# 29. Future Smart Pass

향후 경로 탐색 결과와 FareProduct를 결합할 수 있다.

예:

```text
Route

Metro Marunouchi
       ↓
Metro Ginza
```

사용자 패스:

```text
Tokyo Subway Ticket
```

각 Railway ID를 FareProduct의 적용 범위와 비교한다.

결과:

```text
✓ 패스 이용 가능
```

또는:

```text
✕ 추가 운임 필요
```

---

# 30. Future Tourist Spot

v3에서는 관광지 데이터를 추가할 수 있다.

예상 모델:

```ts
export type TouristSpot = {
  id: string;

  name: MultilingualText;

  latitude: number;
  longitude: number;

  nearestStationIds: string[];

  recommendedExitIds?: string[];
};
```

v1.0에서는 구현하지 않는다.

---

# 31. Future Station Exit

향후 관광지 안내를 위해 출구 모델을 추가할 수 있다.

```ts
export type StationExit = {
  id: string;

  stationId: string;

  name: string;

  latitude?: number;
  longitude?: number;
};
```

예:

```text
아사쿠사역
   │
   ├── A1
   ├── A2
   ├── A3
   └── ...
```

---

# 32. 데이터 폴더 구조

초기 Expo 프로젝트에서는 다음 구조를 고려한다.

```text
src/
│
├── data/
│   ├── operators.ts
│   ├── railways.ts
│   ├── stations/
│   ├── railway-stations/
│   ├── transfers.ts
│   └── fare-products.ts
│
├── types/
│   ├── operator.ts
│   ├── railway.ts
│   ├── station.ts
│   ├── transfer.ts
│   ├── train.ts
│   └── fare-product.ts
│
├── services/
│   ├── api/
│   └── odpt/
│
└── utils/
    ├── railway.ts
    ├── station.ts
    └── distance.ts
```

실제 Expo 프로젝트 생성 후 필요에 따라 조정한다.

---

# 33. 데이터가 많아질 경우

모든 역을 하나의 거대한 파일에 넣지 않는다.

예:

```text
stations/
│
├── jr/
│   └── yamanote.ts
│
├── metro/
│   ├── ginza.ts
│   ├── marunouchi.ts
│   └── ...
│
└── toei/
    ├── asakusa.ts
    ├── mita.ts
    ├── shinjuku.ts
    └── oedo.ts
```

단:

> 파일은 노선별로 나누더라도 데이터의 타입과 ID 규칙은 동일하게 유지한다.

---

# 34. Single Source of Truth

본 프로젝트에서 가장 중요한 데이터 원칙이다.

예를 들어 오에도선은:

```text
Railway Registry

toei-oedo
```

에서 한 번만 정의한다.

Station 또는 Transfer 데이터에서는:

```ts
railwayId: "toei-oedo"
```

만 사용한다.

따라서:

```text
노선명
노선 코드
노선 색
운영사
```

를 여러 파일에서 반복하지 않는다.

---

# 35. UI 데이터 접근 예

환승정보를 표시할 때:

```ts
const railway = RAILWAYS[railwayId];
```

를 통해 가져온다.

UI:

```text
E
오에도선
大江戸線
```

검색 화면에서도 동일한 Registry를 사용한다.

Station Detail에서도 동일한 Registry를 사용한다.

따라서 앱 전체에서 표현이 일관된다.

---

# 36. Prototype Dataset

전체 역 데이터를 입력하기 전에 소규모 데이터로 모델을 검증한다.

최소 다음 유형을 테스트한다.

```text
1. 환승 없는 일반역

2. 같은 운영사 내 환승역

3. Metro ↔ Toei 환승역

4. JR ↔ Metro 환승역

5. JR ↔ Metro ↔ Toei 복합 환승역

6. 야마노테선 역

7. 오에도선 역
```

대표 후보:

```text
마고메
니혼바시
시부야
신주쿠
다이몬
```

실제 테스트 역은 공식 환승관계를 검증한 후 확정한다.

---

# 37. Validation

전체 데이터 입력 전에 다음 검증 로직을 추가하는 것을 고려한다.

예:

```text
RailwayStation의 railwayId가
RAILWAYS에 존재하는가?

Station ID가 실제로 존재하는가?

Transfer 대상 RailwayStation이 존재하는가?

중복 ID가 있는가?

stationCode가 누락되었는가?
```

개발 환경에서 잘못된 데이터를 조기에 발견하는 것이 목적이다.

---

# 38. Data Integrity Rules

다음 규칙을 지킨다.

### Rule 1

모든 ID는 고유해야 한다.

### Rule 2

존재하지 않는 Railway ID를 참조하면 안 된다.

### Rule 3

존재하지 않는 Station ID를 참조하면 안 된다.

### Rule 4

환승 관계는 공식/검증된 데이터를 기준으로 한다.

### Rule 5

노선 이름과 색을 Station 데이터에 중복 작성하지 않는다.

### Rule 6

ODPT 원본 ID와 앱 내부 ID를 구분한다.

### Rule 7

실시간 데이터와 정적 데이터를 분리한다.

### Rule 8

교통패스 정보는 공식 자료를 기준으로 검증한다.

---

# 39. Architecture Principle

데이터 흐름은 다음 구조를 목표로 한다.

```text
             Official / ODPT Data
                     │
                     ▼
                Adapter Layer
                     │
                     ▼
            Internal Data Model
                     │
         ┌───────────┼───────────┐
         ▼           ▼           ▼
      Search      Railway     Station
         │           │           │
         └───────────┼───────────┘
                     ▼
                 Expo UI
```

UI는 가능한 한 ODPT 원본 데이터 구조를 직접 해석하지 않는다.

---

# 40. 최종 핵심 관계

```text
Operator
   │
   ▼
Railway
   │
   ▼
RailwayStation
   │
   ▼
Station
   │
   ├───────────────┐
   ▼               ▼
StationGroup     Transfer


FareProduct
   │
   ├── Operator
   └── Railway


Realtime

ODPT
 │
 ▼
Adapter
 │
 ├── Train
 └── TrainInformation
```

---

# 41. v1.0에서 실제 구현할 모델

v1.0:

```text
Operator            ✓
Railway             ✓
Station             ✓
RailwayStation      ✓
StationGroup        ✓
Transfer            ✓
Train               ✓
TrainInformation    ✓
FareProduct         ✓
Favorite            ✓
UserPass            ✓
```

Future:

```text
RouteEdge            v2.0
TouristSpot          v3.0
StationExit          v3.0
```

GPS 자체는 별도 데이터 모델보다는 Station 좌표를 사용한다.

---

# 42. 설계 목표

이 데이터 모델의 최종 목표는 단순히 v1.0을 구현하는 것이 아니다.

현재:

```text
노선
역
환승
열차
패스
```

를 안정적으로 구현하면서도 향후:

```text
GPS
 ↓
가까운 역
 ↓
경로 탐색
 ↓
패스 기반 경로
 ↓
관광지 / 출구
```

까지 기존 데이터를 최대한 재사용할 수 있도록 한다.

---

# 43. 핵심 원칙 요약

```text
1. 같은 정보는 한 번만 정의한다.

2. 다른 데이터에서는 ID로 참조한다.

3. ODPT 데이터와 앱 내부 모델을 분리한다.

4. Station과 RailwayStation을 구분한다.

5. 환승정보는 공통 구조로 관리한다.

6. 환승이 없는 역은 불필요한 데이터를 만들지 않는다.

7. 정적 데이터와 실시간 데이터를 분리한다.

8. 좌표와 역 순서는 미래 기능을 위해 보존한다.

9. 전체 데이터를 넣기 전에 소규모 Prototype으로 검증한다.

10. 미래 기능 때문에 v1.0을 복잡하게 만들지는 않는다.
```

---

**Last Updated:** 2026-08-18  
**Status:** Draft / Architecture Design  
**Data Model Version:** 1.0  
**Target:** Tokyo Railway Guide Mobile App v1.0
