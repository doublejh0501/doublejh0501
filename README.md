# 김재훈 (Jaehoon Kim)

> **기획부터 구현까지, AI 도구를 활용해 빠르게 실행하는 백엔드 개발자**
>
> 무신사 내부 API를 개발자 도구로 발굴하고, CLAUDE.md로 팀 지식을 관리합니다.
> Docker Compose로 바로 실행 가능한 실용적인 코드를 작성합니다.

<br>

## 🔥 대표 프로젝트

### 1️⃣ 무신사 크롤링 시스템 (BE-Repository)
**무신사가 공개하지 않은 내부 API를 개발자 도구로 발굴하여 약 5000개 상품 데이터 수집**

**문제**: 무신사는 개인에게 API를 제공하지 않아서 패션 트렌드 데이터를 수집할 방법이 없었습니다.

**해결**:
- Chrome DevTools Network 탭으로 내부 API 엔드포인트 발견 (`/api2/hm/web/v5/pans/ranking`)
- 처음엔 봇 탐지로 차단당했지만, 1초 지연 + 재시도 로직으로 우회
- JSoup으로 Next.js `__NEXT_DATA__` JSON을 파싱해 데이터 추출

**결과**: 4시간 동안 무중단 크롤링으로 카테고리별 랭킹 데이터 자동 수집 성공

**기술 스택**: Java 21, Spring Boot 3.5.7, PostgreSQL 16, JSoup

📌 [프로젝트 상세보기](https://github.com/cryschan/BE-Repository)

<br>

### 2️⃣ IPZY - AI 코디 추천 서비스
**Java 백엔드와 Python AI 서버를 통합한 마이크로서비스 아키텍처**

**기획**: "옷장에 있는 옷으로 어떤 코디를 할 수 있을까?"라는 아이디어에서 시작했습니다.

**구현**:
- Spring Boot(비즈니스 로직)와 FastAPI(AI 처리) 역할 분리
- GPT-4o로 사용자 이미지 분석 (색상/패턴 추출), GPT-3.5-turbo로 코디 추천
- 무신사 PLP API를 활용해 브랜드별 상품 자동 수집
- 처음엔 Flask를 쓰려고 했는데, OpenAI API 비동기 호출 때문에 FastAPI로 변경

**결과**: 이미지 업로드만으로 색상 분석부터 코디 추천까지 자동 처리

**기술 스택**: Java 21, Spring Boot 3.2.0, Python 3.11, FastAPI, OpenAI API, PostgreSQL 16

📌 **Backend**: [ipzy-backend](https://github.com/cryschan/ipzy-backend) | **AI**: [ipzy-ai](https://github.com/cryschan/ipzy-ai)

<br>

### 3️⃣ Bookstore - 온라인 서점 플랫폼
**주문/결제 도메인 설계 및 트랜잭션 처리 구현**

**문제**: 주문 생성, 결제 처리, 재고 차감이 동시에 일어나는데 원자성을 어떻게 보장할까?

**해결**:
- JPA 트랜잭션으로 주문/결제/재고 차감을 하나의 작업 단위로 묶음
- 동시성 문제는 낙관적 락(Optimistic Lock)으로 제어
- 3인 팀 프로젝트로 Git 브랜치 전략 및 코드 리뷰 진행

**결과**: 주문 도메인을 담당하며 트랜잭션 처리 로직 설계 완료

**기술 스택**: Java, Spring Boot, MySQL, Docker Compose

📌 [프로젝트 상세보기](https://github.com/doublejh0501/Bookstore)

<br>

---

## 🤖 CLAUDE.md로 팀 지식 관리

팀 프로젝트에서 회의 결정사항(네이밍 규칙, 도메인 구조)을 CLAUDE.md로 문서화했습니다.
Claude Code가 이 파일을 읽고 팀 컨벤션을 자동으로 반영한 코드를 생성합니다.

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
