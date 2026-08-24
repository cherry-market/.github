# 🍒 Cherry Market

> **K-POP 팬덤 굿즈 거래에 특화된 C2C 플랫폼**
> 상품 탐색, 거래 상태 관리, 실시간 채팅, 이미지 처리 등 굿즈 거래에 필요한 기능을 하나의 서비스로 구성했습니다.

<div align="center">

[![Docs](https://img.shields.io/badge/Documentation-Architecture-blue?style=for-the-badge\&logo=gitbook\&logoColor=white)](https://github.com/cherry-market/cherry-architecture)
[![Status](https://img.shields.io/badge/Public_Deployment-Paused-lightgrey?style=for-the-badge)](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/low-cost-migration.md)

</div>

> [!IMPORTANT]
> 현재 공개 배포는 중단된 상태입니다.
> 기존 AWS 기반 인프라는 구현·배포했던 구조로 문서화되어 있으며, 현재는 기존 설계 특성을 유지하면서 운영 비용을 낮추기 위한 **저비용 인프라 전환안을 설계한 상태**입니다. 전환 구조는 아직 구현하지 않았습니다.
> → [저비용 인프라 전환 설계](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/low-cost-migration.md)

---

## 🏗️ Engineering Overview

| Category             | Stack & Architecture                                                                              |
| :------------------- | :------------------------------------------------------------------------------------------------ |
| **Frontend**         | React 19, TypeScript, Vite, FSD (Feature-Sliced Design), Zustand, Tailwind CSS                    |
| **Admin**            | React, TypeScript, Zustand, Tailwind CSS                                                          |
| **Backend**          | Spring Boot 3, Java 21, JPA/Hibernate, Spring Security, MySQL, Redis                              |
| **Real-time**        | STOMP over WebSocket, SimpleBroker                                                                |
| **Previous Infra**   | AWS EC2, RDS, S3, Lambda, CloudFront, ALB, Docker Compose                                         |
| **Migration Design** | Static Hosting, Spring Boot Runtime, Managed MySQL, Redis/Valkey, Object Storage + Queue + Worker |
| **Development**      | Human-led architecture & implementation with AI-assisted development                              |

> `Migration Design` 항목은 **Designed / Not Implemented** 상태입니다.

---

## 👋 서비스 소개

**Cherry**는 K-POP 굿즈 거래에 필요한 상품 탐색과 거래 흐름을 하나의 서비스로 구성한 C2C 플랫폼입니다.

포토카드, 앨범 등 팬덤 굿즈를 아티스트·멤버·상품 유형 등의 기준으로 구조화하고, 상품 등록부터 검색·채팅·거래 상태 변경까지 이어지는 흐름을 구현했습니다.

### 🚀 주요 기능

* **상품 탐색**

  * 아티스트 / 멤버 / 상품 타입 기반 데이터 구조
  * 필터·정렬 및 MySQL FULLTEXT 기반 상품 검색
  * 찜, 트렌딩 상품, 상품 목록 조회

* **실시간 채팅**

  * WebSocket / STOMP 기반 1:1 채팅
  * 읽음 처리 및 채팅 목록 실시간 갱신
  * 채팅과 거래 상태 변경 흐름 연결

* **거래 상태 관리**

  * `판매중 → 예약중 → 판매완료` 상태 전환
  * 상태 변경 규칙과 예외 케이스를 도메인 단위로 관리

* **비동기 이미지 처리**

  * Presigned URL을 이용한 원본 이미지 직접 업로드
  * `S3 ObjectCreated Event → Lambda(sharp)` 기반 비동기 리사이징
  * Detail / Thumbnail 생성 후 Backend Callback으로 처리 상태 반영

* **캐싱 및 성능 개선**

  * Redis Cache-Aside 적용
  * 상품 조회 경로의 캐시 적용 전후를 `wrk`로 측정
  * 캐시 무효화와 데이터 일관성을 함께 고려

* **관리자 기능**

  * 사용자 신고·차단
  * 사용자 제재 및 관리자 대시보드
  * 감사 로그와 관리자 API 접근 제어

---

## 🔄 Infrastructure Evolution

Cherry의 기존 공개 배포는 AWS 기반으로 구성했습니다.

```text
React / Vite
    ├── S3 + CloudFront
    │
    └── Spring Boot / EC2
            ├── RDS MySQL
            ├── Redis
            └── S3 Original
                    ↓
               S3 Event
                    ↓
              Lambda + sharp
                    ↓
            Detail / Thumbnail
                    ↓
             Backend Callback
```

현재는 특정 AWS 서비스를 그대로 유지하는 것보다, 기존 구조에서 중요했던 설계 특성을 보존하면서 운영 비용을 낮추는 방향으로 전환 구조를 설계했습니다.

유지하는 주요 기준은 다음과 같습니다.

* MySQL / JPA 기반 데이터 계층 유지
* Redis / Valkey 기반 Cache-Aside 구조 유지
* 이미지 처리와 API 서버의 실행 책임 분리
* 이벤트 기반 비동기 이미지 처리 유지
* 예상 사용량에 맞는 인프라 구성

전환 후보 구조는 다음과 같습니다.

```text
Static Hosting
    │
    └── Spring Boot Runtime
            ├── Managed MySQL
            ├── Managed Redis / Valkey
            └── Object Storage
                    ↓
                  Event
                    ↓
                  Queue
                    ↓
              Image Worker
                    ↓
            Detail / Thumbnail
                    ↓
             Backend Callback
```

> 위 구조는 **Designed / Not Implemented** 상태입니다.
> 상세한 판단 기준과 기존 구조와의 비교는 [저비용 인프라 전환 설계](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/low-cost-migration.md)에 기록했습니다.

---

## 🤖 AI-Augmented Development

Cherry는 요구사항 정의와 설계 판단을 사람이 주도하고, AI를 구현과 검증을 보조하는 도구로 활용했습니다.

### Human-led

* 요구사항 및 도메인 모델 정의
* 데이터 모델링과 아키텍처 설계
* 기술 선택과 트레이드오프 판단
* 핵심 비즈니스 로직 구현
* 코드 리뷰 및 최종 변경 승인

### AI-assisted

* 반복적인 구현 작업의 초안 생성
* 테스트 케이스 작성 보조
* 코드 및 보안 리뷰 보조
* 기술 문서와 설계 기록 정리

AI를 사용한 결과만 기록하는 것이 아니라, **어떤 판단을 사람이 수행했고 AI의 결과를 어떻게 검증·수정했는지**를 개발 기록으로 남기고 있습니다.

→ [AI-Augmented Development](https://github.com/cherry-market/cherry-architecture/blob/main/engineering/ai-augmented-development.md)

---

## 📚 Documentation

설계와 주요 기술적 의사결정은 [`cherry-architecture`](https://github.com/cherry-market/cherry-architecture)에 문서화하고 있습니다.

### Architecture

* [System Overview](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/system-overview.md) — 전체 시스템 및 기존 AWS 인프라 구성
* [저비용 인프라 전환 설계](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/low-cost-migration.md) — 기존 설계 특성을 유지한 저비용 전환 후보 구조
* [ERD](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/erd.md) — 데이터 모델 설계
* [CI/CD + Runtime](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/cicd.md) — 배포 파이프라인 및 런타임 구조
* [Product Domain](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/product-domain.md) — 상품 조회, 캐싱, 검색, 찜, 트렌딩
* [Image Pipeline Domain](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/image-pipeline-domain.md) — Presigned URL, S3, Lambda 기반 이미지 처리
* [Chat Domain](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/chat-domain.md) — WebSocket / STOMP 채팅 구조
* [Auth Domain](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/auth-domain.md) — JWT / Spring Security 기반 인증 구조
* [Admin Domain](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/admin-domain.md) — 관리자 기능 및 접근 제어

### Architecture Decision Records

주요 설계 선택은 ADR로 별도 기록합니다.

→ [Architecture Decision Records](https://github.com/cherry-market/cherry-architecture/tree/main/decisions)

### Performance

Redis Cache-Aside 적용 전후의 조회 성능을 동일한 조건에서 측정하고 결과를 기록했습니다.

→ [Caching Optimization Report](https://github.com/cherry-market/cherry-architecture/blob/main/performance/caching-optimization.md)

### Engineering

* [AI-Augmented Development](https://github.com/cherry-market/cherry-architecture/blob/main/engineering/ai-augmented-development.md)
* [Security Review](https://github.com/cherry-market/cherry-architecture/blob/main/engineering/security-review-ai-collaboration.md)

---

## 📂 Repositories

| Repository                                                                      | Role               | Description                            |
| :------------------------------------------------------------------------------ | :----------------- | :------------------------------------- |
| **[cherry-client](https://github.com/cherry-market/cherry-client)**             | **Frontend**       | React + Vite + FSD 기반 사용자 웹 애플리케이션     |
| **[cherry-server](https://github.com/cherry-market/cherry-server)**             | **Backend**        | Spring Boot 기반 REST API 및 WebSocket 서버 |
| **[cherry-admin](https://github.com/cherry-market/cherry-admin)**               | **Admin**          | 관리자 대시보드 웹 애플리케이션                      |
| **[cherry-architecture](https://github.com/cherry-market/cherry-architecture)** | **Documentation**  | 시스템 아키텍처, ADR, 성능 분석, 엔지니어링 기록         |
| **[ops](https://github.com/cherry-market/ops)**                                 | **Infrastructure** | 기존 AWS 인프라 구축 및 배포 구성                  |

<br>

<div align="center">
  <sub>Cherry Market &mdash; Human-led Engineering with AI Assistance</sub>
</div>
