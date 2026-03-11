# 🍒 Cherry Market

> **FSD 아키텍처 기반의 글로벌 K-POP 팬덤 C2C 플랫폼**  
> _아이돌 굿즈 거래의 신뢰성 문제와 탐색 비효율을 해결하는 버티컬 커머스_

<div align="center">

[![Service URL](https://img.shields.io/badge/Service-cheryi.com-FF2E88?style=for-the-badge&logo=safari&logoColor=white)](https://cheryi.com)
[![Docs](https://img.shields.io/badge/Documentation-Architecture-blue?style=for-the-badge&logo=gitbook&logoColor=white)](https://github.com/cherry-market/cherry-architecture)
[![Status](<https://img.shields.io/badge/Status-Under_Development_(Phase_8:_MyPage_&_Profile)-orange?style=for-the-badge>)](#)

</div>

---

## 🏗️ Core Engineering Identity

| Category        | Stack & Architecture                                                      |
| :-------------- | :------------------------------------------------------------------------ |
| **Frontend**    | **React, TypeScript, FSD (Feature-Sliced Design)**, Zustand, Tailwind CSS |
| **Backend**     | **Spring Boot 3, Java 21**, JPA, MySQL, Redis, WebSocket/STOMP, AWS (EC2/S3/RDS) |
| **Methodology** | **AI-Augmented Development** (Human Architect + AI Scaffolding)           |

---

## 👋 서비스 소개 (Introduction)

**Cherry**는 범용 중고 거래 플랫폼의 한계를 넘어, **K-POP 굿즈 거래에 최적화된 경험**을 제공하는 C2C 플랫폼을 목표로 개발 중입니다.  
"포토카드, 앨범 등 팬덤 굿즈의 특수성(희소성, 상태 민감도)을 시스템적으로 어떻게 해결할 것인가?"를 기술적으로 풀어내고 있습니다.

### 🚀 핵심 제공 가치 (Key Value Props)

- **버티컬 최적화**: 아티스트/멤버/상품 타입별 계층적 구조화, 필터링, MySQL FULLTEXT 검색 시스템 구축
- **실시간 채팅 거래**: WebSocket/STOMP 기반 1:1 실시간 채팅, 읽음 확인, 메시지 큐잉, 채팅 내 거래 상태 변경
- **상태 시각화**: `판매중` → `예약중` → `판매완료` 트랜잭션 상태 관리 및 UX 시각화

---

## 🤖 개발 방법론: AI-Augmented Engineering

본 프로젝트는 단순한 코드 작성이 아닌, **AI 에이전트를 효율적인 팀원(Junior Developer)으로 활용하는 협업 프로세스**를 정립하여 진행했습니다.

- **Human-Lead (Architecting)**:
  - 비즈니스 요구사항 정의 (RFP) 및 아키텍처 설계
  - 데이터 모델링 (ERD) 및 핵심 비즈니스 로직 구현
  - 코드 리뷰 및 최종 머지 승인 (Quality Gatekeeper)
- **AI-Support (Acceleration)**:
  - 반복적인 보일러플레이트 코드 스캐폴딩
  - API 명세 및 [개발 히스토리](https://github.com/cherry-market/cherry-architecture/blob/main/engineering/ai-augmented-development.md) 문서화 자동화
  - 테스트 케이스 초안 생성

> 📚 1인 개발의 리소스 한계를 극복하고, **엔터프라이즈급 문서화 커버리지**와 **일관된 아키텍처 품질**을 확보하고 있습니다.

---

## 📚 주요 산출물 (Documentation)

모든 개발 과정과 의사결정은 문서화되어 관리되고 있습니다.

- **[📂 아키텍처 문서 (Architecture Docs)](https://github.com/cherry-market/cherry-architecture)**
  - **아키텍처**: [시스템 구성도](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/system-overview.md), [ERD](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/erd.md), [CI/CD](https://github.com/cherry-market/cherry-architecture/blob/main/architecture/cicd.md)
  - **설계 결정**: [ADR (Architecture Decision Records)](https://github.com/cherry-market/cherry-architecture/blob/main/decisions/README.md)
  - **성능 최적화**: [Redis 캐싱 Before/After 벤치마크](https://github.com/cherry-market/cherry-architecture/blob/main/performance/caching-optimization.md)
  - **엔지니어링**: [AI-Augmented Development 방법론](https://github.com/cherry-market/cherry-architecture/blob/main/engineering/ai-augmented-development.md)

---

## 📂 리포지토리 (Repositories)

| Repository                                                                      | Role         | Description                                        |
| :------------------------------------------------------------------------------ | :----------- | :------------------------------------------------- |
| **[cherry-client](https://github.com/cherry-market/cherry-client)**             | **Frontend** | React + Vite + FSD Architecture 구현체 (Private)   |
| **[cherry-server](https://github.com/cherry-market/cherry-server)**             | **Backend**  | Spring Boot REST API Server (Private)              |
| **[cherry-architecture](https://github.com/cherry-market/cherry-architecture)** | **Docs**     | 시스템 아키텍처, ADR, 성능 분석, 엔지니어링 방법론 |
| **[ops](https://github.com/cherry-market/ops)**                                 | **DevOps**   | AWS 인프라 구축 및 배포 스크립트 (Private)         |

<br>

<div align="center">
  <sub>Managed by <b>Cherry Team</b> (Human & AI Collaboration)</sub>
</div>
