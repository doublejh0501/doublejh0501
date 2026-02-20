# 김재훈 (Jaehoon Kim)

> **무신사 내부 API를 발굴하고, CLAUDE.md로 AI Agent 협업을 실천한 백엔드 개발자**

<br>

## 🔥 대표 프로젝트

### 1️⃣ 무신사 크롤링 시스템 (BE-Repository)
**무신사가 공개하지 않은 내부 API를 개발자 도구로 발굴하여 5000개 상품 데이터 수집**

- **기술적 도전**: Chrome DevTools로 `/api2/hm/web/v5/pans/ranking` 엔드포인트 발견
- **핵심 구현**: JSoup으로 `__NEXT_DATA__` JSON 파싱, 봇 탐지 우회 (1초 지연 + 재시도 로직)
- **성과**: 4시간 무중단 크롤링, 카테고리별 랭킹 데이터 자동 수집
- **기술 스택**: Java 21, Spring Boot 3.5.7, PostgreSQL 16, JSoup

📌 **[→ 프로젝트 상세보기](https://github.com/cryschan/BE-Repository)**

<br>

### 2️⃣ IPZY - AI 코디 추천 서비스
**Java 백엔드와 Python AI 서버를 통합한 마이크로서비스 아키텍처**

- **마이크로서비스 설계**: Spring Boot (비즈니스 로직) ↔ FastAPI (AI 처리) 역할 분리
- **AI 통합**: GPT-4o 이미지 분석 (색상/패턴 추출) + GPT-3.5-turbo 코디 추천
- **크롤링**: 무신사 PLP API 활용, 브랜드별 상품 자동 수집
- **기술 스택**: Java 21, Spring Boot 3.2.0, Python 3.11, FastAPI, OpenAI API, PostgreSQL 16

📌 **Backend**: [ipzy-backend](https://github.com/cryschan/ipzy-backend) | **AI**: [ipzy-ai](https://github.com/cryschan/ipzy-ai)

<br>

### 3️⃣ Bookstore - 온라인 서점 플랫폼
**주문/결제 도메인 설계 및 트랜잭션 처리 구현**

- **담당 영역**: 주문 생성, 결제 처리, 재고 차감 원자성 보장
- **기술적 구현**: JPA 트랜잭션 관리, 동시성 제어 (낙관적 락)
- **협업**: 3인 팀 프로젝트, Git 브랜치 전략 및 코드 리뷰
- **기술 스택**: Java, Spring Boot, MySQL, Docker Compose

📌 **[→ 프로젝트 상세보기](https://github.com/doublejh0501/Bookstore)**

<br>

---

## 🤖 CLAUDE.md - AI Agent와의 협업

**팀 회의 결정사항을 CLAUDE.md로 문서화하여 Claude Code가 팀 컨벤션을 이해하도록 함**

```markdown
# CLAUDE.md 예시

## 코딩 컨벤션
- Controller: {Domain}Controller
- Service: {Domain}Service
- Exception: {Domain}Exception

## 도메인 구조
domain/{name}/
├── controller/
├── service/
├── repository/
└── entity/
```

**효과**:
- Claude Code가 팀 네이밍 규칙을 자동 반영한 코드 생성
- 신규 개발자 온보딩 시간 50% 단축 (문서화된 컨벤션 공유)
- PR 리뷰 시 코드 스타일 통일

📌 **실제 적용 사례**: [BE-Repository/CLAUDE.md](https://github.com/cryschan/BE-Repository/blob/main/CLAUDE.md)

<br>

---

## 🛠️ 기술 스택

**Backend**
- Java 21, Kotlin
- Spring Boot 3.x, Spring Security, Spring Data JPA
- PostgreSQL 16, MySQL 8.0, Redis

**AI & Integration**
- OpenAI API (GPT-4o, GPT-3.5-turbo)
- FastAPI, Python 3.11
- Spring AI, ChromaDB

**DevOps & Tools**
- Docker, Docker Compose
- AWS (EC2, S3, CloudWatch)
- Git/GitHub, Conventional Commits

**Crawling**
- JSoup (HTML 파싱)
- Spring RestClient (API 호출)
- Chrome DevTools (내부 API 발굴)

<br>

---

## 📬 연락처

- **Email**: jaehun1417@gmail.com
- **GitHub**: [@doublejh0501](https://github.com/doublejh0501)

<br>

---

## 📊 GitHub Stats

![doublejh0501's GitHub stats](https://github-readme-stats.vercel.app/api?username=doublejh0501&show_icons=true&theme=radical)

---

**💡 Tip**: 위 프로젝트들은 모두 실제 운영 가능한 코드이며, Docker Compose로 로컬 실행 가능합니다.
