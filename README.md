# 김재훈 | Backend Developer

<div align="center">

<img src="images/profile.jpg" alt="프로필 사진" width="200px">

**"기획부터 운영까지, AI 도구를 활용할 줄 아는 생산성 높은 개발자"**

요구사항을 명확히 파악하고 다양한 리더, 팔로워 경험으로 팀과 적극적으로 소통하며 문제를 해결합니다.

회의 결정사항을 문서화하여 협업 효율을 높이고, 기술적 난관에 유연하게 대처합니다.

[![GitHub](https://img.shields.io/badge/GitHub-doublejh0501-181717?style=flat&logo=github)](https://github.com/doublejh0501)
[![Email](https://img.shields.io/badge/Email-jaehun1417@gmail.com-EA4335?style=flat&logo=gmail)](mailto:jaehun1417@gmail.com)

</div>

---

## 🚀 Projects

### 1. Backend Developer Intern | 소프트랩스 (SoftLabs)
**2025.11 - 2025.12 (2개월) | 팀 프로젝트 (5명) | 실무 인턴십**

> 패션 블로그 자동 생성 시스템 개발 (Fashion 크롤링, AI 통합, BlogTemplate 도메인)

#### 나의 역할
- Fashion 크롤링, AI 로직 및 통합, BlogTemplate 도메인 (단독)

#### 핵심 성과

**1. 패션 E-commerce 크롤링 - 내부 API 발굴**

문제 상황:
- 타겟 패션 쇼핑몰은 공개 API를 제공하지 않아서 데이터 수집 방법을 찾아야 했습니다.

해결 과정:
1. Chrome DevTools Network 탭에서 사이트 상품 로딩 추적
2. XHR 요청 중 JSON 응답 반환하는 내부 API 엔드포인트 발견
3. Headers 분석 (User-Agent, Cookie, Referer) 후 Postman 테스트
4. Java RestClient로 구현

안정적인 데이터 수집:
- 문제: 연속 요청 시 서버 부하로 차단
- 해결: 카테고리별 요청 사이 적절한 지연 시간 설정, HTTP 502/503 에러 발생 시 재시도 로직 (2초 후 최대 2회), SSH로 서버 로그 실시간 모니터링
- 결과: 안정적으로 상품 데이터 수집 완료

기술 구현:
- RestClient (Spring 6.1+)로 JSON API 호출
- JSoup으로 HTML 파싱 (API로 제공되지 않는 색상 정보 추출)

**2. 팀 협업 및 문서화 - 5명 팀원과 회의 기반 의사결정**

팀 구성: 나 (Fashion 크롤링, AI 통합, BlogTemplate), 팀원들 (User, Auth, Blog, Dashboard, 프론트엔드)

협업 방식:
- 주간 회의에서 기술적 결정 논의 및 합의
- Slack으로 회고 및 이슈 공유
- Conventional Commits (feat:, fix:, docs:) 적용

문서화 및 지식 공유:
- 주간 회의록 작성: 기술 결정사항 (PostgreSQL 배열 타입 선택, 크롤링 전략), 이슈 해결 방법 기록
- Slack 채널로 즉시 공유: 크롤링 에러 해결 방법 (HTTP 502/503 재시도 로직), EC2 배포 가이드
- API 문서화: Swagger로 BlogTemplate API 문서 작성, 프론트엔드 팀과 JSON 응답 형식 협의
- 코드 리뷰 시 결정사항 문서화: PR 코멘트에 "왜 이렇게 구현했는지" 기록

**3. 인프라 비용 최적화 및 배포**

EC2 인스턴스 선정:
- t2.micro (프리티어) 시도했지만 Spring Boot 실행 시 메모리 부족 (OOM)
- t3.small (2 vCPU, 2GB RAM) 선택, 비용 대비 성능 균형점

배포 및 운영:
- AWS EC2 t3.small 배포 전체 담당 (팀 5명 배포 가이드 작성)
- Docker 이미지 빌드 자동화
- PostgreSQL Docker 컨테이너 구성
- SSH 로그 모니터링, 환경 변수 관리

#### 기술 스택
- Backend: Spring Boot 3.5.7, Java 21
- Database: PostgreSQL 16 (배열 타입으로 colors[], seasons[] 저장)
- AI: Spring AI 1.0.0-M4, OpenAI API
- Crawling: JSoup 1.18.3, RestClient
- Infrastructure: AWS EC2 t3.small, Docker

기술 선택 이유:
- PostgreSQL 16: 배열 타입으로 다중 값 저장 (정규화 없이 `colors[]` 저장하여 JOIN 불필요)
- RestClient: RestTemplate 대비 최신 Spring 6.1+ API, 간결한 문법

#### GitHub
[Repository](https://github.com/cryschan/BE-Repository)

---

### 2. IPZY - AI 패션 코디 추천 서비스
**2025.11 - 2025.12 | 팀 프로젝트 (5명) | 부트캠프 최종 프로젝트**

> 사용자 퀴즈 분석부터 AI 추천, 코디 이미지 생성까지 자동화하는 플랫폼

#### 시스템 아키텍처

```
┌─────────────┐         ┌──────────────────┐         ┌─────────────────┐
│   Frontend  │────────▶│  Spring Boot     │────────▶│   FastAPI       │
│   (React)   │         │  (Java 21)       │         │   (Python 3.11) │
└─────────────┘         │                  │         │                 │
                        │  - Product       │         │  - OpenAI API   │
                        │  - Recommendation│         │  - ChromaDB     │
                        │  - User          │         │  - Image Process│
                        └─────────┬────────┘         └────────┬────────┘
                                  │                           │
                        ┌─────────▼────────┐       ┌─────────▼────────┐
                        │   PostgreSQL 16  │       │    AWS S3        │
                        │   (pgvector)     │       │  (이미지 저장)    │
                        └──────────────────┘       └──────────────────┘
```

#### 나의 역할
- **Product 도메인**: 패션 상품 크롤링, 상품 DB 설계 (단독)
- **AI 서비스**: Python FastAPI 전체 개발 (OpenAI, ChromaDB, 단독)
- **Recommendation 통합**: Java와 Python 마이크로서비스 통합 (단독)

#### 핵심 기능

**1. CLAUDE.md 기반 팀 지식 관리 체계 구축**

문제 인식:
- 회의에서 결정된 기술적 선택(PostgreSQL 배열 타입, 카테고리 매핑 로직)이 각자 머리에만 있음
- Claude Code에게 질문할 때 팀 최신 결정사항이 반영 안 되어 혼동 발생
- 예: "상품 색상 어떻게 저장?" 질문 시 Claude는 "정규화해서 별도 테이블"이라 답변했지만 팀 결정과 다름

해결 - CLAUDE.md 문서 작성:

내용 구성:
1. 프로젝트 구조: 도메인별 역할, 모듈 간 관계
2. 코딩 컨벤션: Service는 @Transactional 필수, DTO는 record 사용
3. 회의 결정사항 (날짜별):
   - "2025.11.20: PostgreSQL 배열 타입 사용 결정 (정규화 대신)"
   - "2025.11.22: 무신사 크롤링 시 1초 지연 필요 (봇 탐지 우회)"
4. 문서화 요령:
   - 언제: 회의 후 즉시, 중요한 기술 선택 시
   - 무엇: 기술 선택 이유, 변경사항, 주의사항
   - 어떻게: 날짜 명시, 이유 포함, 간결하게

결과:
- Claude Code가 팀 결정사항 이해하고 일관된 코드 생성 (예: "크롤링 코드 작성" 요청 시 자동으로 1초 지연 로직 포함, "상품 색상 저장 로직" 요청 시 PostgreSQL 배열 타입 활용)
- 팀원 간 기술적 결정 공유 및 혼동 방지
- 문서화 습관 정착으로 지속 가능한 협업 체계

**2. AI 추천 시스템 (Python FastAPI - 전담)**

기술 선택 배경:

FastAPI 선택 이유 (Flask 대신):
- OpenAI API 호출 시 비동기 처리 필수, async/await 네이티브 지원
- Pydantic으로 타입 안전성 확보
- 자동 OpenAPI 문서 생성 (Java 팀과 협업 용이)

ChromaDB 선택 이유 (Pinecone 대신):
- Pinecone 유료 (월 $70), ChromaDB 무료
- Docker로 로컬 운영 가능
- Python 네이티브, OpenAI 임베딩 호환

구현 내용:
- OpenAI API (GPT-3.5-turbo)로 퀴즈 답변 분석
- ChromaDB 벡터 검색으로 카테고리별 유사 상품 추출
- rembg (U2-Net)로 상품 이미지 배경 제거
- Pillow로 코디 합성 이미지 생성
- AWS S3에 이미지 업로드 (boto3)

프롬프트 엔지니어링:
- 초기: "코디 추천해줘" 요청 시 이상한 조합 (상의 3개, 하의 0개)
- 개선: "상의 1개, 하의 1개, 신발 1개, 색상 계열 통일" 명시
- 결과: 추천 품질 향상, 자연스러운 코디 조합

테스트 코드 (pytest):
- OpenAI API 호출 모킹 테스트
- ChromaDB 검색 결과 검증
- 이미지 처리 파이프라인 단위 테스트

**3. 마이크로서비스 통합 (Java와 Python)**

구현:
- Java RestClient로 Python FastAPI 호출
- 중복 조합 회피: TreeSet으로 이전 추천 ID 추적
- 타임아웃 설정 (연결 5초, 읽기 60초)

협업 및 문서화:

API 문서 기반 협업:
- Swagger 문서로 Java/Python 마이크로서비스 API 스펙 정의
- 프론트엔드 팀과 JSON 응답 구조 협의 (필드명, 타입, 예시 값)
- API 변경 시 Swagger 문서 즉시 업데이트 → Slack 공지 → 팀원 확인 프로세스

CLAUDE.md 기반 지식 공유:
- 기술 결정사항 날짜별 기록 (PostgreSQL 배열 타입, 카테고리 매핑 로직)
- 크롤링 로직 주의사항 문서화 (1초 지연, User-Agent 설정)
- 신규 팀원이 CLAUDE.md 읽고 즉시 개발 시작 가능한 수준으로 작성

코드 리뷰 및 품질 관리:
- GitHub PR 리뷰, CodeRabbit AI 리뷰 적극 활용
- Conventional Commits 적용 (feat:, fix:, docs:)

#### 배포 및 운영
- AWS EC2 배포 전체 담당 (팀원 배포 가이드 작성)
- Docker Compose로 Java/Python 마이크로서비스 통합 배포
- PostgreSQL Docker 컨테이너 구성
- 환경 변수 관리 (.env), 멀티 컨테이너 오케스트레이션
- AWS S3 이미지 저장소 구성 및 권한 관리
- SSH 로그 모니터링

#### Tech Stack
- **Backend**: Spring Boot 3.2.0, Java 21, PostgreSQL 16 (pgvector)
- **AI Service**: FastAPI 0.109.0, Python 3.11, OpenAI API, ChromaDB
- **Image**: Pillow, rembg (U2-Net)
- **Infrastructure**: AWS EC2, Docker Compose, AWS S3
- **Frontend**: React 19, TypeScript 5.9, Vercel
- **Testing**: JUnit 5, Mockito, pytest

#### GitHub
- [Backend Repository](https://github.com/cryschan/ipzy-backend)
- [AI Service Repository](https://github.com/cryschan/ipzy-ai)

---

### 3. Musinsa Price Tracker - 무신사 가격 추적 시스템
**2026.03 (1주) | 개인 프로젝트**

> 소프트랩스 Java 크롤링 경험을 Node.js로 재구현 (FETCHING 지원 대비)

#### 프로젝트 개요
소프트랩스 인턴에서 Java로 패션 크롤링을 경험한 후, Node.js 생태계 학습을 위해 같은 도메인(무신사 크롤링)을 NestJS로 재구현한 프로젝트.

**목표**: Java와 Node.js의 비동기 처리 방식 차이를 실전으로 학습

#### 시스템 아키텍처

```
┌─────────────┐         ┌──────────────────┐         ┌─────────────────┐
│   Crawler   │────────▶│     NestJS       │────────▶│     MySQL       │
│  (Axios)    │         │  (TypeScript)    │         │  (TypeORM)      │
└─────────────┘         │                  │         │                 │
                        │  - Products API  │         │  - products     │
                        │  - Crawler API   │         │  - price_history│
                        └──────────────────┘         └─────────────────┘
```

#### 주요 기능

**1. 무신사 API 리버스 엔지니어링**

문제 인식:
- 초기: Puppeteer 헤드리스 브라우저로 HTML 파싱 시도
- 실패: 무신사가 SPA 구조로 변경되어 동적 렌더링 파싱 실패
- 느리고 복잡한 브라우저 자동화

해결 - 무신사 내부 API 직접 호출:

리버스 엔지니어링 과정:
1. Chrome DevTools Network 탭에서 무신사 검색 페이지 분석
2. `api.musinsa.com` 도메인의 JSON API 발견
3. 요청 헤더 분석 (User-Agent, Accept) 후 Postman 테스트
4. 나이키 신발 카테고리 API 엔드포인트 확인
5. Axios로 구현 (Puppeteer 대비 10배 빠름)

```typescript
// 소프트랩스에서 배운 방식: 내부 API 직접 호출
const apiUrl = 'https://api.musinsa.com/api2/dp/v2/plp/goods?brand=nike&category=103';
const response = await axios.get(apiUrl);
const products = response.data.data.list; // 30개 상품
```

결과:
- 30개 나이키 신발 실시간 수집 (에어맥스, 에어조던 등)
- Puppeteer 대비 응답 속도 10배 향상
- 안정적인 JSON 파싱

**2. 가격 변동 추적 시스템 설계**

문제:
- 매번 크롤링할 때마다 히스토리를 저장하면 불필요한 데이터 증가
- 가격이 변하지 않았는데도 중복 저장됨

해결 - 효율적인 히스토리 관리:

DB 설계:
- `products`: 상품 기본 정보 (goods_no UNIQUE로 중복 방지)
- `price_history`: 가격 변동 시에만 저장

가격 변동 감지 로직:
```typescript
// goods_no로 기존 상품 조회
// 신규 상품: 생성 + 히스토리 저장
// 기존 상품: 가격/품절 상태 변경 시에만 히스토리 저장
if (product.price !== productData.price) {
  await this.createHistory(product);
}
```

실제 성과:
- 아스트로그래버: 139,000원 → 125,100원 (10% 할인 감지)
- 불필요한 데이터 저장 방지 (변동 없으면 히스토리 미생성)
- 할인가/정상가 구분 추적

**3. Java vs Node.js 비동기 처리 비교 학습**

소프트랩스 Java 방식:
- 블로킹 I/O: 응답 대기 중 스레드 block
- 요청당 스레드 생성, 블로킹으로 메모리 낭비

Node.js 방식:
- 논블로킹 I/O: 이벤트 루프가 다른 작업 처리
- 단일 스레드 이벤트 루프, 논블로킹으로 효율적
- async/await으로 비동기 코드를 동기처럼 작성

| 항목 | Java (소프트랩스) | Node.js (이 프로젝트) |
|------|------------------|---------------------|
| HTTP | RestClient | Axios |
| 파싱 | JSoup | 무신사 API 직접 호출 |
| I/O | 블로킹 | 논블로킹 (async/await) |
| 프레임워크 | Spring Boot | NestJS |
| ORM | JPA | TypeORM |

학습 포인트:
- Java: 요청당 스레드 생성, 블로킹으로 메모리 낭비
- Node.js: 단일 스레드 이벤트 루프, 논블로킹으로 효율적
- async/await으로 비동기 코드를 동기처럼 작성

**4. TypeORM 관계 설정**
- Product ↔ PriceHistory 양방향 관계 구현
- @OneToMany, @ManyToOne 데코레이터로 자동 JOIN
- GET /products/:id/history: 상품별 가격 변동 히스토리 조회
- Swagger로 자동 문서화

#### 기술적 성과
- 30개 나이키 신발 실시간 추적
- goods_no UNIQUE constraint로 중복 방지
- 가격 변동 시에만 히스토리 저장 (효율적 DB 설계)
- Java RestClient → Node.js Axios 전환 경험
- Puppeteer 실패 → API 직접 호출 성공

#### Tech Stack
- **Backend**: NestJS, TypeScript, TypeORM
- **Database**: MySQL 8.0
- **Crawler**: Axios (무신사 API 직접 호출)
- **Infrastructure**: Docker Compose
- **Documentation**: Swagger UI

#### 프로젝트 규모
- Backend: 800+ 라인 (TypeScript)
- REST API: 6개 엔드포인트
- 30개 상품 추적, 가격 히스토리 자동 관리

#### GitHub
[Repository](https://github.com/doublejh0501/musinsa-price-tracker)

---

### 4. Dev Quiz - 개발 용어 퀴즈 플랫폼
**2026.03 (2주) | 개인 프로젝트**

> Reactive Programming 학습 및 면접 대비를 위한 개발 용어 퀴즈 플랫폼

![Dev Quiz 메인 화면](images/devquiz-main.png)

#### 프로젝트 개요
면접 준비를 위한 개발 용어 학습 플랫폼. Spring WebFlux + R2DBC로 Reactive Programming 실습.

#### 시스템 아키텍처

```
┌─────────────┐         ┌──────────────────┐         ┌─────────────────┐
│   Next.js   │────────▶│  Spring WebFlux  │────────▶│  PostgreSQL     │
│ (TypeScript)│         │  (Kotlin)        │         │  (R2DBC)        │
└─────────────┘         │                  │         │                 │
                        │  - REST API      │         │  - 6 카테고리   │
                        │  - Reactive      │         │  - 42 문제      │
                        └─────────┬────────┘         └─────────────────┘
                                  │
                        ┌─────────▼────────┐
                        │     Redis        │
                        │   (캐싱)         │
                        └──────────────────┘
```

#### 주요 기능

**1. Reactive Programming 구현**

기술 선택 배경:

Spring WebFlux 선택 이유 (Spring MVC 대신):
- 비동기 논블로킹 I/O로 높은 동시성 처리
- Reactive Streams 표준 준수 (Mono, Flux)
- 메모리 효율적 백프레셔 관리

R2DBC 선택 이유 (JDBC 대신):
- Reactive Database 연동 (PostgreSQL)
- 비동기 쿼리 실행
- WebFlux와 완벽한 통합

구현 내용:
```kotlin
@GetMapping("/api/quizzes")
fun getAllQuizzes(): Flux<QuizResponse> {
    return quizRepository.findAll()
        .map { it.toResponse() }
        .limitRate(10) // 백프레셔 관리
}
```

- Spring WebFlux 비동기 논블로킹 REST API 6개 구현
- R2DBC로 PostgreSQL Reactive 연동
- Mono/Flux 기반 스트림 처리와 백프레셔 관리
- Redis 캐싱 (퀴즈 데이터)으로 응답 속도 개선

결과:
- 동시성 처리 향상 (메모리 효율적)
- 비동기 논블로킹으로 응답 속도 개선

**2. TypeScript + Kotlin 타입 안정성**

기술 조합 이유:
- Kotlin: null-safety, data class, sealed class로 타입 안전성
- TypeScript: 프론트엔드 타입 안전성, API 응답 타입 정의
- 양방향 타입 체크로 런타임 에러 최소화

구현:
```typescript
// TypeScript
interface QuizResponse {
  id: number;
  question: string;
  options: string[];
  correctAnswer: string;
  category: string;
}
```

```kotlin
// Kotlin
data class QuizResponse(
    val id: Long,
    val question: String,
    val options: List<String>,
    val correctAnswer: string,
    val category: String
)
```

- Next.js 15 App Router 동적 라우팅
- TailwindCSS 반응형 UI
- API 응답 타입 정의 (TypeScript interface ↔ Kotlin data class 매핑)

결과:
- 양방향 타입 체크로 런타임 에러 최소화
- API 변경 시 컴파일 타임 에러로 조기 발견

**3. 테스트 주도 개발**

테스트 전략:
- JUnit 5 + MockK: 단위 테스트 10개
- WebFlux 테스트: WebTestClient로 비동기 API 테스트
- 통합 테스트: TestContainer PostgreSQL

구현:
```kotlin
@Test
fun `퀴즈 조회 API 테스트`() {
    webTestClient.get()
        .uri("/api/quizzes/1")
        .exchange()
        .expectStatus().isOk
        .expectBody<QuizResponse>()
        .consumeWith { response ->
            assertThat(response.responseBody?.id).isEqualTo(1)
        }
}
```

- 퀴즈 CRUD API 테스트 (7개 테스트 메서드)
- 카테고리 필터링 테스트
- 예외 처리 테스트
- Reactive 비동기 테스트 (StepVerifier로 Mono/Flux 검증)
- Repository 모킹 및 Service 계층 단위 테스트

테스트 커버리지:
- 7개 테스트 메서드 (QuestionServiceTest)
- JUnit 5 + MockK 단위 테스트
- Reactive 비동기 테스트 (StepVerifier)
- WebTestClient로 비동기 API 통합 테스트

**4. Next.js 15 App Router**

```
app/
├── page.tsx (메인 페이지)
├── quiz/
│   └── [category]/
│       └── page.tsx (카테고리별 퀴즈)
└── layout.tsx (공통 레이아웃)
```

특징:
- Server Components 기본 지원 (성능 향상)
- 동적 라우팅 간소화
- TailwindCSS 반응형 디자인

#### 기술적 성과
- Reactive Programming으로 높은 동시성 처리
- TypeScript + Kotlin 타입 안정성 확보
- 테스트 커버리지로 코드 품질 보장
- Swagger UI 자동 API 문서화

#### Tech Stack
- **Backend**: Kotlin, Spring Boot 3.2, WebFlux, PostgreSQL (R2DBC), Redis
- **Frontend**: Next.js 15, TypeScript, TailwindCSS
- **Testing**: JUnit 5, MockK (10개 테스트)
- **Infrastructure**: Docker Compose
- **Documentation**: Swagger UI

#### 프로젝트 규모
- Backend: 1,700+ 라인 (Kotlin)
- Frontend: 800+ 라인 (TypeScript)
- 총 6개 REST API, 42개 샘플 데이터

#### GitHub
[Repository](https://github.com/doublejh0501/quizdispensor)

---

### 5. NGS gamecamp - Steam 게임 판매 플랫폼
**2025.09 - 2025.10 | 팀 프로젝트 (6명)**

> Steam API 연동 게임 정보 수집 + 소셜 로그인 + 결제 시스템을 갖춘 종합 게임 판매 플랫폼

#### 프로젝트 개요
Steam API 연동 게임 정보 수집 + 소셜 로그인 + 결제 시스템

#### 나의 역할
User, Security 모듈 전체 담당 (단독)

#### 주요 성과

**1. JWT + OAuth2 인증 시스템 구현**

기술 선택 배경:
- JWT Stateless 선택 (Session 대신): 수평 확장 용이, 서버 간 세션 공유 불필요, Redis 인프라 비용 절감
- HttpOnly 쿠키 선택 (LocalStorage 대신): XSS 공격 방어, Access Token (6시간) 보안 강화

구현 내용:
- Google, Kakao, Naver OAuth2 통합 (provider별 attribute 매핑)
- 커스텀 필터로 Authorization 헤더 + HttpOnly 쿠키 동시 인증
- 신규 유저 자동 가입 (upsert)

문제 해결 - OAuth2 테스트 어려움:
- 문제: OAuth2는 로컬 테스트 불가 (실제 provider 서버 필요)
- 발생 이슈: 백엔드 인증 후 프론트엔드 리다이렉트 실패, 각 provider(Google, Kakao, Naver)마다 리다이렉트 URL 형식 상이
- 해결: OAuth2 성공 핸들러에서 프론트 URL로 JWT 포함 리다이렉트, 각 provider 콘솔에서 정확한 리다이렉트 URL 등록
- 결과: 실행하며 문제 발견 → 즉시 수정 반복으로 안정적인 소셜 로그인 구현

**2. 동시성 환경 팔로우 중복 방지 이중 방어**

문제 인식:
- 같은 사용자를 동시에 여러 번 팔로우하면 중복 insert 가능성
- 서비스 로직만으로는 동시 요청 시 중복 체크 통과

해결 전략:
- DB 레벨 방어: @UniqueConstraint (user_id, target_type, target_id)로 데이터 무결성 보장
- 서비스 레벨 방어: 선행 검증으로 사용자 친화적 에러 메시지 반환
  - 중복 팔로우: "이미 팔로우 한 상태입니다"
  - 자가 팔로우: "자기 자신을 팔로우할 수 없습니다"

```java
// DB 제약
@UniqueConstraint(columnNames = {"user_id", "target_type", "target_id"})

// 서비스 로직
if (followRepository.existsByUserIdAndTargetTypeAndTargetId(...)) {
    throw new IllegalArgumentException("이미 팔로우 한 상태입니다");
}
```

결과:
- 동시 요청 상황에서도 중복 팔로우 완벽 차단
- ConstraintViolationException 대신 명확한 에러 안내로 UX 개선

#### 테스트 코드 작성
- 29개 테스트 파일 (Service, Controller, Repository 계층)
- OAuth2 인증 통합 테스트 (Google, Kakao, Naver)
- 팔로우 서비스 단위 테스트
- 중복 팔로우, 자가 팔로우 예외 케이스 검증
- 주문/결제 통합 테스트

#### Tech Stack
- Backend: Spring Boot 3.5.5, Java 21
- Database: MySQL 8.0
- Security: JWT, OAuth2 (Google, Kakao, Naver)
- Testing: JUnit 5, Mockito (29개 테스트 파일)

#### GitHub
[Repository](https://github.com/doublejh0501/BE_Repository)

---

### 6. Bookstore - Yes24 스타일 도서 판매 사이트
**2025.08 - 2025.10 | 팀 프로젝트 (3명)**

> Yes24 스타일 온라인 서점 플랫폼

#### 나의 역할
팀 리더 + Order 모듈 (단독)

#### 주요 성과

**1. 팀 리더십 및 프로젝트 기획**

프로젝트 구조 설계:
- User, Book, Order 도메인 분담 결정
- 기술 스택 선정 및 근거 설명 (Spring Boot, MySQL, Redis, KakaoPay API)

협업 관리:
- 주간 회의 주도, 일정 관리, 이슈 트래킹
- Git 브랜치 전략 수립 (feature → develop → main)
- 코드 리뷰 프로세스 운영

**2. Order 모듈 개발**

주문-재고 트랜잭션 처리:

구현:
```java
@Transactional
public Order createOrder(OrderRequest request) {
    // 1. 재고 확인 (부족 시 즉시 실패)
    if (book.getStock() < request.getQuantity()) {
        throw new OutOfStockException();
    }

    // 2. 주문 생성 + 재고 차감
    Order order = orderRepository.save(new Order(...));
    book.decreaseStock(request.getQuantity());

    // 3. 결제 처리
    paymentService.pay(order);

    // 성공: 커밋, 실패: 롤백 (재고 복원)
    return order;
}
```

- 장바구니 기반 주문 생성 및 재고 확인
- @Transactional로 주문-재고 원자성 보장
- 재고 부족 시 주문 생성 전 예외 발생
- 결제 실패 시 자동 롤백 (재고 복원)

비즈니스 로직:
1. 재고 확인하여 부족 시 즉시 실패
2. 주문 생성하고 재고 차감
3. 결제 처리
4. 성공하면 커밋, 실패하면 롤백

결과: 동시성 문제 없이 데이터 일관성 유지

#### 테스트 코드 (JUnit 5, Mockito)
- OrderServiceTest: 15개 테스트 메서드
- ArgumentCaptor로 메서드 호출 검증
- 트랜잭션 통합 테스트 (주문-결제-재고 연계)
- 결제 실패 시 롤백 검증
- 재고 부족, 빈 장바구니 등 edge case 커버

#### Tech Stack
- Spring Boot 3.5, Java 21, MySQL 8.0, Redis 7, KakaoPay API
- Testing: JUnit 5, Mockito

#### GitHub
[Repository](https://github.com/doublejh0501/Bookstore)

---

### 7. TicketingPlatform - 공연 예매 플랫폼
**2024.09 - 2024.12 | 팀 프로젝트 (5명)**

> 공연 정보 조회, 좌석 선택, 예매, 커뮤니티 기능

![TicketingPlatform 메인 화면](images/ticketing-main.png)

#### 나의 역할
팀 리더 + Frontend Developer + Controller

#### 주요 구현

**1. 팀 리더십**
- 프로젝트 도메인 분담 (Performance, Reservation, User, Community)
- 기술 스택 선정 (Spring Boot 3.2.4, Java 17, Thymeleaf, H2)
- Git 브랜치 전략 및 협업 프로세스 정립

**2. 전역 UI/UX 체계 구축**
- 전역 헤더, 푸터, 네비게이션 바 설계
- 공통 CSS/JS 컴포넌트화로 화면 일관성 확보
- 반응형 디자인 적용

**3. 공연 예매 시스템 UI**
- JavaScript 기반 동적 좌석 선택
- 실시간 예약 상태 표시 (예약 가능/선택됨/예약 완료)
- AJAX 비동기 처리 (댓글 작성/수정/삭제)

**4. Spring Security 연동 UI**
- Thymeleaf로 로그인/OAuth 버튼 구현
- CSRF 토큰 hidden input 처리
- Security 태그로 권한별 메뉴 동적 표시/숨김

#### Tech Stack
- Spring Boot 3.2.4, Java 17, Thymeleaf, H2, Spring Security
- Frontend: HTML5, CSS3, JavaScript ES6

#### GitHub
[Repository](https://github.com/jjunh0/TicketingPlatform)

---

### 8. Lang잔고를 부탁해 - AI 대출 상담 챗봇
**2025.10 - 2025.11 | 팀 프로젝트 (4명)**

> LangChain 기반 AI 대출 상담 챗봇

#### 나의 역할
팀 리더 + Compute Policies 모듈

#### 시스템 아키텍처

```
사용자 질문 → ChromaDB 검색 → LLM 응답 생성 → 금융 계산 엔진 → 대출 가능 여부 판단
```

#### 주요 성과

**1. 팀 리더십 및 문서화**
- 금융 계산 엔진 구조 설계 (LTV, DTI, DSR)
- README 작성: 프로젝트 개요, 실행 방법, 환경 변수 설정 가이드
- 시스템 흐름도, API 문서, 테스트 케이스 문서화
- Slack 주간 회고: 완료 작업, 다음 주 계획, 이슈 해결 방법 공유

**2. Compute Policies 모듈**
- LTV, DTI, DSR 계산 로직
- 대출 가능 여부 판단
- 정책 파라미터 버전 관리

**3. 프롬프트 엔지니어링 - 챗봇 답변 품질 개선**
- 문제: GPT-3.5-turbo 사용 초기, 대출 가능 여부 질문에 금융 계산 근거 없이 단정적으로 답변하거나 ChromaDB에서 불러온 금융 정보를 잘못 해석하는 경우 발생
- 해결: GPT-3.5-turbo → GPT-4.0으로 모델 교체 (복잡한 금융 조건 분기 처리 향상)
- 시스템 프롬프트 개선: 금융 상담사 역할 명시, LTV/DTI/DSR 계산 근거를 응답에 포함하도록 명시, ChromaDB 검색 문서 기반으로만 답변하도록 제약 추가
- 결과: 계산 근거가 포함된 구조화된 응답 생성, 불확실한 정보 단정 답변 감소

#### Tech Stack
- Python 3.11, LangChain, LangGraph, ChromaDB, FastAPI, OpenAI GPT-4.0

#### GitHub
[Repository](https://github.com/doublejh0501/Loanchat_dev)

---

## 🛠 Technical Skills

### Backend
- **Java 21, Spring Boot 3.x**: JPA, Security, RESTful API
- **Kotlin, Spring Boot 3.2**: WebFlux (Reactive Programming), R2DBC
- **Python 3.11, FastAPI**: 비동기 웹 프레임워크

### Frontend
- **Next.js 15**: App Router, Server Components
- **TypeScript, TailwindCSS**
- **React 19**

### Database
- **PostgreSQL 16**: 배열 타입, JSONB, pgvector, R2DBC (Reactive)
- **MySQL 8.0**: 인덱스 최적화, 트랜잭션 관리
- **Redis 7**: 캐싱

### AI & Vector DB
- **OpenAI API**: GPT-3.5-turbo, GPT-4.0, 프롬프트 엔지니어링
- **ChromaDB**: 벡터 검색
- **LangChain/LangGraph**: RAG 파이프라인

### Web Crawling
- **JSoup**: HTML 파싱
- **RestClient** (Spring 6.1+): REST API 크롤링

### Testing
- **JUnit 5**: Mockito (Java), MockK (Kotlin)
- **pytest**: Python 단위 테스트
- **WebTestClient**: WebFlux 비동기 테스트

### Infrastructure
- **AWS**: EC2 (t3.small)
- **Docker**: 멀티 스테이지 빌드, Docker Compose

### Tools
- **Git/GitHub**: Conventional Commits (feat:, fix:, docs:)
- **CodeRabbit**: AI 코드 리뷰
- **Swagger**: API 문서화
- **Slack**: 팀 커뮤니케이션

---

## 🤖 AI 활용 능력

**팀 협업 최적화**
- **CLAUDE.md 기반 팀 지식 관리**: 회의 결정사항 문서화, Claude Code가 팀 맥락 이해

**개발 생산성 도구**
- **Claude Code (Sonnet 4.5)**: 크롤링 로직, 테스트 코드 작성
- **GitHub Copilot**: DTO, Entity 자동 완성
- **ChatGPT**: 기술 스택 비교, 에러 디버깅

**AI 프로젝트 경험**
- OpenAI API 프롬프트 엔지니어링
- CodeRabbit AI 코드 리뷰

---

## 🎓 Education

**패스트캠퍼스 AI융합 백엔드 개발 부트캠프**
2025.06 - 2025.12 | 서울 강남
- 6개월 풀타임 (Java, Spring Boot, Python, AI)
- 5개 팀 프로젝트, Git 협업 경험

**강원대학교 컴퓨터공학 학사 졸업**
2019.03 - 2025.08 | 강원 춘천

---

## 📜 Certifications

- SQLD (SQL 개발자) (2025)
- 정보처리기사 (2025)

---

## 🌐 Language

- TOEIC 865점 (2025)
- 영문 기술 문서 독해 가능

---

## 📫 Contact

- **Email**: jaehun1417@gmail.com
- **Phone**: 010-8801-9685
- **GitHub**: https://github.com/doublejh0501
- **Location**: 경기도 남양주시

---

**최종 업데이트**: 2026.03.30
