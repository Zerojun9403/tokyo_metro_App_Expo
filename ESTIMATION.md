# Tokyo Railway Guide App - Development Estimation

> Tokyo Railway Guide 모바일 애플리케이션 개발 공수 산정 문서

---

## 1. 문서 목적

본 문서는 **Tokyo Railway Guide Mobile App v1.0** 개발에 필요한 예상 작업량을 산정하고 실제 개발 과정에서 발생한 작업량과 비교하기 위한 문서다.

단순히 개발 완료 날짜를 예상하는 것이 아니라 다음 항목을 관리하는 것을 목적으로 한다.

- 기능별 예상 개발 공수
- 실제 개발 공수
- 개발 진행률
- 예상보다 오래 걸린 작업
- 예상보다 빠르게 완료된 작업
- 추가 요구사항으로 발생한 공수
- 프로젝트 전체 일정 변화

개발 완료 후에는 예상 공수와 실제 공수를 비교하여 향후 프로젝트의 개발 일정 산정에 활용한다.

---

# 2. Estimation Unit

본 프로젝트에서는 개발 공수를 **MD(Man-Day)** 기준으로 산정한다.

## MD

```text
1 MD = 개발자 1명이 하루 동안 수행하는 작업량
```

예:

```text
0.5 MD = 약 반나절 작업
1 MD   = 약 하루 작업
2 MD   = 약 이틀 작업
```

본 프로젝트는 **1인 개발 프로젝트**이므로 인원에 따른 공수 분배는 적용하지 않는다.

단, 실제 개발은 회사 업무 이후 또는 여유 시간에 진행될 수 있으므로:

```text
MD ≠ 실제 달력상의 날짜
```

로 본다.

---

# 3. Project Scope

공수 산정 대상은 다음 버전이다.

```text
Project:
Tokyo Railway Guide

Target:
Mobile App v1.0

Platform:
iOS
Android

Frontend:
Expo
React Native
TypeScript
Expo Router

Backend:
Backend API
ODPT API

Deployment:
EAS Build
TestFlight
```

---

# 4. Initial Estimation

초기 예상 개발 공수:

```text
28 MD
```

리스크 버퍼:

```text
20%
```

계산:

```text
28 MD × 0.20 = 5.6 MD
```

따라서 리스크를 포함한 예상 공수는:

```text
28 MD + 5.6 MD
= 33.6 MD
≈ 34 MD
```

---

# 5. Estimated Development Effort

| ID | 작업 | 예상 공수 | 실제 공수 | 상태 |
|---|---|---:|---:|---|
| 01 | 요구사항 및 화면 구조 설계 | 1 MD | - | ⬜ |
| 02 | Railway / Station / Transfer 데이터 모델 | 1.5 MD | - | ⬜ |
| 03 | Expo 프로젝트 및 Router 기본 구조 | 0.5 MD | - | ⬜ |
| 04 | 디자인 시스템 및 공통 컴포넌트 | 1 MD | - | ⬜ |
| 05 | Home / 운영사 / 노선 선택 | 1 MD | - | ⬜ |
| 06 | 일반 노선도 UI | 2 MD | - | ⬜ |
| 07 | 야마노테선 순환선 UX | 1.5 MD | - | ⬜ |
| 08 | 오에도선 특수 노선 UX | 1.5 MD | - | ⬜ |
| 09 | 통합 역 검색 | 1.5 MD | - | ⬜ |
| 10 | 역 상세 화면 | 1 MD | - | ⬜ |
| 11 | 환승정보 통합 | 1.5 MD | - | ⬜ |
| 12 | ODPT API 연동 | 2 MD | - | ⬜ |
| 13 | 다음 열차 / 시간표 | 1.5 MD | - | ⬜ |
| 14 | 운행 / 지연정보 | 1 MD | - | ⬜ |
| 15 | 즐겨찾기 | 1 MD | - | ⬜ |
| 16 | Suica / PASMO / 교통패스 데이터 | 1.5 MD | - | ⬜ |
| 17 | My Pass / 이용 가능 여부 | 1.5 MD | - | ⬜ |
| 18 | Backend DB / Push Notification | 3 MD | - | ⬜ |
| 19 | 실제 기기 테스트 / 버그 수정 | 2 MD | - | ⬜ |
| 20 | TestFlight / 배포 준비 | 1 MD | - | ⬜ |
| | **TOTAL** | **28 MD** | **-** | |

---

# 6. Status Definition

개발 상태는 다음과 같이 기록한다.

```text
⬜ Not Started
🟨 In Progress
🟩 Completed
🟥 Blocked
```

예:

```text
| 통합 역 검색 | 1.5 MD | 1 MD | 🟩 |
```

---

# 7. Phase Estimation

프로젝트를 크게 6개 Phase로 나눈다.

---

## Phase 1 - Planning & Architecture

### 작업

- 요구사항 정의
- 앱 화면 구조
- 데이터 모델
- Railway Registry
- Station 모델
- Transfer 모델
- FareProduct 모델
- API 구조

### 예상 공수

```text
2.5 MD
```

### 목표

코딩을 시작하기 전에 앱 전체 구조를 결정한다.

특히 웹 프로젝트에서 발생했던 데이터 중복 문제를 앱에서는 초기에 해결한다.

---

# 8. Phase 2 - App Foundation

### 작업

- Expo 프로젝트 생성
- TypeScript 설정
- Expo Router
- Navigation
- 디자인 시스템
- 공통 UI
- Home
- 철도회사 선택
- 노선 선택

### 예상 공수

```text
2.5 MD
```

### 목표

모든 기능의 기반이 되는 앱 구조를 완성한다.

---

# 9. Phase 3 - Railway UX

### 작업

- 일반 노선도
- 야마노테선
- 오에도선
- Station Detail
- Transfer UI

### 예상 공수

```text
7.5 MD
```

### 주요 Risk

본 프로젝트에서 UI/UX 난도가 가장 높은 영역이다.

특히:

```text
Yamanote Line
→ 순환선

Oedo Line
→ 특수한 노선 구조
```

때문에 예상보다 개발 시간이 증가할 가능성이 있다.

---

# 10. Phase 4 - Search & Realtime Data

### 작업

- 통합 역 검색
- ODPT API
- 다음 열차
- 시간표
- 운행 정보
- 지연 정보

### 예상 공수

```text
6 MD
```

### 목표

앱을 단순한 정적 노선도에서 실제 철도 정보 서비스로 확장한다.

---

# 11. Phase 5 - Tourist Features

### 작업

- 즐겨찾기
- Suica
- PASMO
- Tokyo Subway Ticket
- Tokyo Metro 24-hour Ticket
- 교통패스 적용 범위
- My Pass

### 예상 공수

```text
4 MD
```

### 목표

일반적인 철도 앱과 차별화되는 관광객 중심 기능을 구현한다.

---

# 12. Phase 6 - Notification & Release

### 작업

- Backend DB
- Push Notification
- 지연 상태 감지
- 중복 알림 방지
- 실제 기기 테스트
- 버그 수정
- EAS Build
- TestFlight

### 예상 공수

```text
5.5 MD
```

---

# 13. Phase Summary

| Phase | 내용 | 예상 공수 |
|---|---|---:|
| Phase 1 | Planning & Architecture | 2.5 MD |
| Phase 2 | App Foundation | 2.5 MD |
| Phase 3 | Railway UX | 7.5 MD |
| Phase 4 | Search & Realtime | 6 MD |
| Phase 5 | Tourist Features | 4 MD |
| Phase 6 | Notification & Release | 5.5 MD |
| **TOTAL** | | **28 MD** |

---

# 14. Development Priority

모든 기능을 동시에 개발하지 않는다.

우선순위는 다음과 같다.

## P0 - Must Have

앱의 핵심 기능이다.

```text
Expo 기본 구조
Railway 데이터
Station 데이터
노선 선택
역 선택
노선도
역 상세
통합 역 검색
환승 정보
ODPT API
다음 열차
운행 정보
```

---

## P1 - Should Have

관광객용 앱의 차별화 기능이다.

```text
즐겨찾기
Suica / PASMO 정보
Tokyo Subway Ticket
Tokyo Metro 24-hour Ticket
My Pass
```

---

## P2 - Advanced

앱 기본 기능 안정화 이후 구현한다.

```text
Push Notification
Backend DB
지연 알림 설정
지연 복구 알림
```

---

# 15. Milestones

## Milestone 1

### App Skeleton

목표:

```text
Expo
+
Router
+
Home
+
Railway Select
```

완료 조건:

앱을 실행하고 철도회사와 노선을 선택할 수 있다.

---

## Milestone 2

### Railway Navigation

목표:

```text
Railway
↓
Station
↓
Station Detail
```

완료 조건:

실제 역 데이터를 이용하여 역 상세 화면까지 이동할 수 있다.

---

## Milestone 3

### Realtime Railway

목표:

```text
Expo
↓
Backend
↓
ODPT
```

완료 조건:

실제 휴대폰에서 실시간 열차/운행 정보를 확인할 수 있다.

---

## Milestone 4

### Tourist Guide

목표:

```text
Station
+
Pass
+
My Pass
```

완료 조건:

사용자가 특정 교통패스로 해당 노선을 이용할 수 있는지 확인할 수 있다.

---

## Milestone 5

### Release Candidate

목표:

```text
iPhone
+
Android
+
Production API
```

완료 조건:

주요 기능 테스트를 통과하고 치명적인 오류가 없다.

---

## Milestone 6

### Store Build

목표:

```text
EAS Build
↓
TestFlight
```

완료 조건:

실제 배포용 앱 빌드가 설치 및 실행된다.

---

# 16. Risk Management

현재 예상되는 주요 리스크는 다음과 같다.

| Risk | 영향도 | 대응 |
|---|---|---|
| 야마노테선 모바일 UX | 높음 | 초기 Prototype 제작 |
| 오에도선 노선 표현 | 높음 | 별도 UI 적용 |
| 환승 데이터 불일치 | 높음 | Railway Registry + Normalize |
| ODPT 데이터 차이 | 높음 | 공통 Adapter 계층 사용 |
| 데이터 규모 증가 | 중간 | 정적/실시간 데이터 분리 |
| Push 중복 알림 | 중간 | 이전 상태 저장 및 비교 |
| API Key 노출 | 높음 | Backend에서만 관리 |
| 패스 정보 변경 | 중간 | 공식 자료 검증 + 기준일 표시 |
| iOS/Android UI 차이 | 중간 | 실제 기기 테스트 |
| Scope 증가 | 높음 | v1.0 요구사항 고정 |

---

# 17. Contingency

프로젝트 초기 예상 공수의 20%를 리스크 대응 공수로 확보한다.

```text
Base Estimate
28 MD

Risk Buffer
5.6 MD

Total Estimate
33.6 MD

Rounded
34 MD
```

따라서 프로젝트의 공식 초기 예상 범위는:

```text
28 ~ 34 MD
```

로 관리한다.

---

# 18. Personal Project Schedule

본 프로젝트는 1인 개인 프로젝트이므로 실제 일정은 MD와 다를 수 있다.

예를 들어 하루 평균 작업 가능 시간이 적은 경우:

```text
평일
약 1~3시간

주말
약 3~6시간
```

처럼 작업량이 달라질 수 있다.

따라서 일정 관리에서는:

```text
Estimated Effort
→ MD

Calendar Schedule
→ 실제 개발 가능 시간
```

을 분리해서 관리한다.

---

# 19. Actual Effort Tracking

개발을 시작하면 각 작업의 실제 공수를 기록한다.

예:

| 작업 | 예상 | 실제 | 차이 |
|---|---:|---:|---:|
| Expo Setup | 0.5 MD | 0.25 MD | -0.25 |
| Station Model | 1 MD | 1.5 MD | +0.5 |
| Yamanote UX | 1.5 MD | 2 MD | +0.5 |

---

# 20. Estimation Accuracy

프로젝트 완료 후 다음 계산을 통해 예상 정확도를 확인한다.

```text
공수 차이 = 실제 공수 - 예상 공수
```

예:

```text
예상
28 MD

실제
31 MD

차이
+3 MD
```

비율:

```text
3 / 28 × 100
≈ 10.7%
```

즉:

```text
예상 대비 약 10.7% 초과
```

로 기록한다.

---

# 21. Scope Change

개발 중 새로운 기능이 추가되면 기존 예상 공수에 몰래 포함시키지 않는다.

반드시 별도의 Change Request로 기록한다.

예:

```text
CR-001

기능:
현재 위치 기반 가까운 역

추가 예상 공수:
+2 MD

Reason:
v1.0 개발 중 추가 요구사항 발생
```

---

# 22. Change Request Log

| ID | 변경 내용 | 추가 공수 | 상태 |
|---|---|---:|---|
| - | 현재 없음 | - | - |

---

# 23. Development Log

개발하면서 주요 진행 상황을 기록한다.

예:

```text
2026-XX-XX

Phase:
Planning

작업:
Station / Railway 데이터 모델 설계

예상:
1.5 MD

실제:
-

문제:
-

결정:
환승 정보는 운영사별로 중복 관리하지 않고
공통 Railway Registry를 통해 표시한다.
```

---

# 24. Definition of Project Completion

다음 조건을 충족하면 v1.0 개발 완료로 판단한다.

- 핵심 노선 데이터 정상 표시
- 역 검색 정상 동작
- 환승정보 일관성 확보
- 다음 열차 정보 정상 표시
- 운행정보 정상 표시
- 관광패스 기능 정상 동작
- iOS 실제 기기 테스트 완료
- Android 테스트 완료
- Production API 정상 동작
- 치명적인 TypeScript 오류 없음
- 앱 비정상 종료 문제 없음
- Release Build 성공

---

# 25. Final Review

프로젝트 완료 후 다음 내용을 작성한다.

## Initial Estimate

```text
28 MD
```

## Risk Included Estimate

```text
34 MD
```

## Actual Effort

```text
TBD
```

## Difference

```text
TBD
```

## Estimation Accuracy

```text
TBD
```

## Biggest Underestimate

```text
TBD
```

## Biggest Overestimate

```text
TBD
```

## Lessons Learned

```text
TBD
```

---

# 26. Project Estimation Summary

```text
Tokyo Railway Guide Mobile App

Developer
1 Person

Base Effort
28 MD

Risk Buffer
20%

Risk Adjusted Effort
≈ 34 MD

Development Method

Requirements
    ↓
Architecture
    ↓
Prototype
    ↓
Implementation
    ↓
Testing
    ↓
Release
```

---

**Last Updated:** 2026-08  
**Status:** Planning  
**Version:** Estimation v1.0  
**Target:** Tokyo Railway Guide Mobile App v1.0
