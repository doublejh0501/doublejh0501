# 김재훈 (Jaehoon Kim)

> **기획부터 구현까지, AI 도구를 활용해 빠르게 실행하는 백엔드 개발자**
>
> Chrome DevTools로 E-commerce 내부 API를 발굴하고, CLAUDE.md로 팀 지식을 관리합니다.
> Docker Compose로 바로 실행 가능한 실용적인 코드를 작성합니다.

<br>

## 🔥 대표 프로젝트

### 1️⃣ 패션 E-commerce 크롤링 시스템 (BE-Repository)
**소프트랩스 인턴 프로젝트 - 패션 블로그 자동 생성 시스템**

![크롤링 시스템 구조](images/be-repository-architecture.png)
<!-- 아키텍처 다이어그램을 images/be-repository-architecture.png에 업로드하세요 -->

**문제**: 패션 쇼핑몰은 공개 API를 제공하지 않아서 트렌드 데이터를 수집할 방법이 없었습니다.

**해결 과정**:
1. Chrome DevTools Network 탭으로 타겟 사이트 상품 로딩 추적
2. XHR 요청 중 JSON 응답 반환하는 내부 API 엔드포인트 발견
3. Headers 분석 (User-Agent, Cookie, Referer) 후 Postman으로 테스트
4. Java RestClient로 구현
5. 처음엔 연속 요청으로 서버 부하 발생 → 카테고리별 1초 지연 + HTTP 502/503 에러 발생 시 재시도 로직 (2초 후 최대 2회)
6. JSoup으로 HTML 파싱 (API로 제공되지 않는 색상 정보 추출)

**기술 선택 이유**:
- PostgreSQL 16 배열 타입: `colors[]`, `seasons[]`로 다중 값 저장 (정규화 없이 JOIN 불필요)
- RestClient (Spring 6.1+): RestTemplate 대비 최신, 간결한 문법

**인프라 구축**:
- AWS EC2 t3.small 선정: t2.micro는 Spring Boot 실행 시 OOM 발생 → 비용 대비 성능 균형점
- Docker 기반 배포: PostgreSQL 컨테이너 + Spring Boot JAR
- SSH로 로그 실시간 모니터링

**팀 협업**:
- 5명 팀 (나: Fashion 크롤링, AI 통합, BlogTemplate / 팀원들: User, Auth, Blog, Dashboard, 프론트엔드)
- 주간 회의에서 기술 결정 논의 (PostgreSQL 배열 타입 선택, 크롤링 전략)
- Slack으로 회고 및 이슈 공유, Conventional Commits 적용
- 주간 회의록 작성, Swagger로 API 문서화

**결과**: 스케줄러로 자동 수집 시스템 구축, 안정적인 데이터 수집 완료

**테스트**: JUnit 5 + Mockito로 크롤링 로직 및 재시도 메커니즘 단위 테스트

**기술 스택**: Java 21, Spring Boot 3.5.7, PostgreSQL 16, Spring AI 1.0.0-M4, OpenAI API, JSoup 1.18.3, RestClient, AWS EC2 t3.small, Docker, JUnit 5, Mockito

📌 [프로젝트 상세보기](https://github.com/cryschan/BE-Repository)

<br>

### 2️⃣ IPZY - AI 패션 코디 추천 서비스
**부트캠프 최종 프로젝트 (4명 팀) - 사용자 퀴즈 분석부터 AI 추천, 코디 이미지 생성까지 자동화**

![IPZY 메인 화면](images/ipzy-main.png)
<!-- 메인 화면 스크린샷을 images/ipzy-main.png에 업로드하세요 -->

![IPZY 추천 결과](images/ipzy-result.png)
<!-- 추천 결과 화면을 images/ipzy-result.png에 업로드하세요 -->

**나의 역할**:
- Product 도메인: 패션 상품 크롤링, 상품 DB 설계 (단독)
- AI 서비스: Python FastAPI 전체 개발 (OpenAI, ChromaDB, 단독)
- Recommendation 통합: Java와 Python 마이크로서비스 통합 (단독)

**시스템 아키텍처**

![IPZY 아키텍처](images/ipzy-architecture.png)
<!-- 아키텍처 다이어그램을 images/ipzy-architecture.png에 업로드하세요 -->

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

**핵심 1: CLAUDE.md 기반 팀 지식 관리 체계 구축**

문제 인식:
- 회의에서 결정된 기술적 선택(PostgreSQL 배열 타입, 카테고리 매핑 로직)이 각자 머리에만 있음
- Claude Code에게 질문할 때 팀 최신 결정사항이 반영 안 되어 혼동 발생
- 예: "상품 색상 어떻게 저장?" 질문 시 Claude는 "정규화해서 별도 테이블"이라 답변했지만 팀 결정과 다름

해결 - CLAUDE.md 문서 작성:
```markdown
# CLAUDE.md 예시

## 프로젝트 구조
- Product 도메인: 상품 크롤링 및 DB 저장
- Recommendation 도메인: AI 추천 및 이미지 합성

## 코딩 컨벤션
- Service는 @Transactional 필수
- DTO는 record 사용

## 회의 결정사항 (날짜별)
### 2025.11.20: PostgreSQL 배열 타입 사용 결정
- 이유: 정규화 대신 colors[] 저장하여 JOIN 불필요
- 적용: Product 엔티티에 @Column(columnDefinition = "text[]")

### 2025.11.22: 크롤링 시 1초 지연 필요
- 이유: 서버 부하 방지 및 안정성 확보
- 적용: Thread.sleep(1000)
```

결과:
- Claude Code가 팀 결정사항 이해하고 일관된 코드 생성
- 팀원 간 기술적 결정 공유 및 혼동 방지
- 신규 개발자 온보딩 시간 단축

**핵심 2: AI 추천 시스템 (Python FastAPI - 전담)**

기술 선택 배경:
- FastAPI 선택 (Flask 대신): OpenAI API 비동기 호출 시 async/await 네이티브 지원, Pydantic 타입 안전성, 자동 Swagger 문서 생성
- ChromaDB 선택 (Pinecone 대신): Pinecone 유료 (월 $70), ChromaDB 무료 + Docker 로컬 운영 가능

구현 내용:
1. OpenAI API (GPT-3.5-turbo)로 퀴즈 답변 분석
2. ChromaDB 벡터 검색으로 카테고리별 유사 상품 추출
3. rembg (U2-Net)로 상품 이미지 배경 제거
4. Pillow로 코디 합성 이미지 생성
5. AWS S3에 이미지 업로드 (boto3)

프롬프트 엔지니어링 개선:
- 초기: "코디 추천해줘" → 이상한 조합 (상의 3개, 하의 0개)
- 개선: "상의 1개, 하의 1개, 신발 1개, 색상 계열 통일" 명시
- 결과: 추천 품질 향상, 자연스러운 코디 조합

**핵심 3: 마이크로서비스 통합 (Java와 Python)**

구현:
- Java RestClient로 Python FastAPI 호출
- 중복 조합 회피: TreeSet으로 이전 추천 ID 추적
- 타임아웃 설정 (연결 5초, 읽기 60초)

협업 및 문서화:
- Swagger 문서로 Java/Python API 스펙 정의
- API 변경 시 Swagger 업데이트 → Slack 공지 → 팀원 확인 프로세스
- GitHub PR 리뷰, CodeRabbit AI 리뷰 적극 활용

**테스트**:
- Java: JUnit 5 + Mockito로 서비스 레이어 단위 테스트
- Python: pytest로 OpenAI API 모킹, ChromaDB 검색, 이미지 처리 파이프라인 테스트

**기술 스택**: Spring Boot 3.2.0, Java 21, PostgreSQL 16 (pgvector), FastAPI 0.109.0, Python 3.11, OpenAI API, ChromaDB 0.4.22, Pillow 10.2.0, rembg 2.0.54, Docker Compose, AWS S3, React 19, TypeScript 5.9, JUnit 5, Mockito, pytest

📌 **Backend**: [ipzy-backend](https://github.com/cryschan/ipzy-backend) | **AI**: [ipzy-ai](https://github.com/cryschan/ipzy-ai)

<br>

### 3️⃣ NGS gamecamp - Steam 게임 판매 플랫폼
**팀 프로젝트 (6명) - Steam API 연동 게임 정보 수집, 소셜 로그인, 결제 시스템**

![NGS gamecamp 메인](images/ngs-main.png)
<!-- 메인 화면을 images/ngs-main.png에 업로드하세요 -->

**나의 역할: User, Security 모듈 (단독)**

**핵심 1: JWT + OAuth2 인증 시스템**

![인증 흐름도](images/ngs-auth-flow.png)
<!-- 인증 흐름도를 images/ngs-auth-flow.png에 업로드하세요 -->

기술 선택 배경:
- JWT Stateless 선택 (Session 대신): 수평 확장 용이 (서버 간 세션 공유 불필요), Redis 불필요 (인프라 비용 절감)
- HttpOnly 쿠키 선택 (LocalStorage 대신): XSS 공격 방어, Access Token (6시간) + HttpOnly 쿠키 구조

구현:
- Google, Kakao, Naver OAuth2 통합 (provider별 attribute 매핑)
- 커스텀 필터로 Authorization 헤더 + HttpOnly 쿠키 동시 인증
- 신규 유저 자동 가입 (upsert)

문제 해결 - OAuth2 테스트 어려움:
- 상황: OAuth2는 로컬 테스트 불가 (실제 provider 서버 필요)
- 발생 문제: 백엔드 인증 후 프론트엔드 리다이렉트 실패, 각 provider마다 리다이렉트 URL 형식 다름
- 해결: OAuth2 성공 핸들러에서 프론트 URL로 JWT 포함 리다이렉트, 각 provider 콘솔에서 정확한 리다이렉트 URL 등록, 실행하며 문제 발견하고 즉시 수정 반복

**핵심 2: 동시성 환경 팔로우 중복 방지**

문제:
- 같은 사용자를 동시에 여러 번 팔로우하면 중복 insert 가능성
- 서비스 로직만으로는 동시 요청 시 중복 체크 통과

해결:
- DB 제약: `@UniqueConstraint(columnNames = {"user_id", "target_type", "target_id"})` → 데이터 무결성 보장
- 서비스 로직: `if (followRepository.existsByUserIdAndTargetTypeAndTargetId(...))` → 사용자 친화적 에러 메시지

결과:
- 동시 요청 상황에서도 중복 팔로우 방지
- ConstraintViolationException 대신 명확한 에러 안내

**테스트**: JUnit 5 + Mockito로 팔로우 서비스 단위 테스트, 중복 팔로우/자가 팔로우 예외 케이스 검증

**기술 스택**: Spring Boot 3.5.5, Java 21, MySQL 8.0, JWT (jjwt 0.12.5), OAuth2, JUnit 5, Mockito

📌 [프로젝트 상세보기](https://github.com/doublejh0501/BE_Repository)

<br>

### 4️⃣ Bookstore - Yes24 스타일 도서 판매 사이트
**팀 프로젝트 (3명) - 온라인 서점 플랫폼**

![Bookstore 메인](images/bookstore-main.png)
<!-- 메인 화면을 images/bookstore-main.png에 업로드하세요 -->

**나의 역할: 팀 리더 + Order 모듈 (단독)**

**핵심 1: 팀 리더십 및 프로젝트 기획**

프로젝트 구조 설계:
- User, Book, Order 도메인 분담 결정
- 기술 스택 선정 및 근거 설명 (Spring Boot, MySQL, Redis, KakaoPay API)

협업 관리:
- 주간 회의 주도, 일정 관리, 이슈 트래킹
- Git 브랜치 전략 수립 (feature → develop → main)
- 코드 리뷰 프로세스 운영

**핵심 2: Order 모듈 개발 - 주문/재고 트랜잭션 처리**

![주문 프로세스](images/bookstore-order-flow.png)
<!-- 주문 프로세스를 images/bookstore-order-flow.png에 업로드하세요 -->

문제: 주문 생성, 재고 차감, 결제 처리가 동시에 일어나는데 원자성을 어떻게 보장할까?

구현:
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

**테스트**: JUnit 5 + Mockito로 주문 생성, 재고 차감, 트랜잭션 롤백 시나리오 테스트

**기술 스택**: Spring Boot 3.5, Java 21, MySQL 8.0, Redis 7, KakaoPay API, JUnit 5, Mockito

📌 [프로젝트 상세보기](https://github.com/doublejh0501/Bookstore)

<br>

### 5️⃣ TicketingPlatform - 공연 예매 플랫폼
**팀 프로젝트 (5명) - 공연 정보 조회, 좌석 선택, 예매, 커뮤니티 기능**

![TicketingPlatform 메인](images/ticketing-main.png)
<!-- 메인 화면을 images/ticketing-main.png에 업로드하세요 -->

![좌석 선택 UI](images/ticketing-seat.png)
<!-- 좌석 선택 화면을 images/ticketing-seat.png에 업로드하세요 -->

**나의 역할: 팀 리더 + Frontend Developer + Controller**

**핵심 1: 팀 리더십**
- 프로젝트 전체 구조 설계 및 도메인 분담 (Performance, Reservation, User, Community)
- 기술 스택 선정 (Spring Boot 3.2.4, Java 17, Thymeleaf, H2)
- Git 브랜치 전략 수립, 주간 회의 주도

**핵심 2: 전역 UI/UX 체계 구축**
- 전역 헤더, 푸터, 네비게이션 바 설계
- 공통 CSS/JS 컴포넌트화로 화면 일관성 확보
- 반응형 디자인 적용

**핵심 3: 공연 예매 시스템 UI**
- JavaScript 기반 동적 좌석 선택
- 실시간 예약 상태 표시 (예약 가능/선택됨/예약 완료)
- 공연 선택 → 좌석 선택 → 결제 → 완료 단계별 진행 표시

**핵심 4: Spring Security 연동 프론트엔드**
- Thymeleaf로 로그인/OAuth 버튼 및 폼 구현
- CSRF 토큰 hidden input 처리
- Security 태그로 권한별 메뉴 동적 표시/숨김

**기술 스택**: Spring Boot 3.2.4, Java 17, Thymeleaf, H2, Spring Security, HTML5, CSS3, JavaScript ES6

📌 [프로젝트 상세보기](https://github.com/jjunh0/TicketingPlatform)

<br>

### 6️⃣ Lang잔고를 부탁해 - AI 대출 상담 챗봇
**팀 프로젝트 (4명) - LangChain 기반 AI 대출 상담 챗봇**

![챗봇 화면](images/loanchat-main.png)
<!-- 챗봇 화면을 images/loanchat-main.png에 업로드하세요 -->

**나의 역할: 팀 리더 + Compute Policies 모듈**

**시스템 아키텍처**

![LangChain 파이프라인](images/loanchat-architecture.png)
<!-- 아키텍처를 images/loanchat-architecture.png에 업로드하세요 -->

```
사용자 질문 → ChromaDB 검색 → LLM 응답 생성 → 금융 계산 엔진 → 대출 가능 여부 판단
```

**핵심 1: 팀 리더십 및 문서화**

전체 아키텍처 설계:
- 금융 계산 엔진 구조 설계 (LTV, DTI, DSR)
- 정책 파라미터 관리 방식 제안 (버전 관리)

회의 주도 및 일정 관리:
- 주간 회의 진행, 태스크 분배 및 우선순위 설정
- GitHub PR 리뷰, 코딩 컨벤션 가이드

문서화 및 지식 공유:
- README 작성: 프로젝트 개요, 실행 방법, 환경 변수 설정 (신규 팀원 온보딩용)
- 시스템 흐름도: LangChain 파이프라인 (사용자 질문 → ChromaDB 검색 → LLM 응답), 금융 계산 엔진 구조도
- API 문서: FastAPI 자동 Swagger 문서 생성, 엔드포인트별 요청/응답 예시
- 금융 계산 로직 문서화: LTV, DTI, DSR 계산 방식, 정책 파라미터 버전 관리 전략
- 테스트 케이스 문서화: pytest 테스트 시나리오 (정상 케이스, 예외 케이스)
- Slack 주간 회고: 완료 작업, 다음 주 계획, 이슈 해결 방법 공유

**핵심 2: Compute Policies 모듈 개발**

구현:
- LTV (Loan To Value) 계산: 담보 대비 대출 비율
- DTI (Debt To Income) 계산: 소득 대비 부채 상환 비율
- DSR (Debt Service Ratio) 계산: 소득 대비 총 부채 상환 비율
- 대출 가능 여부 판단
- 정책 파라미터 버전 관리 (향후 정책 변경 대비)

기술 스택: Python 3.11, LangChain, LangGraph, ChromaDB, FastAPI

📌 [프로젝트 상세보기](https://github.com/doublejh0501/Loanchat_dev)

<br>

---

## 🤖 CLAUDE.md로 팀 지식 관리

팀 프로젝트에서 회의 결정사항(네이밍 규칙, 도메인 구조, 기술 선택 이유)을 CLAUDE.md로 문서화했습니다.
Claude Code가 이 파일을 읽고 팀 컨벤션을 자동으로 반영한 코드를 생성합니다.

**CLAUDE.md 구성 요소**:
```markdown
# CLAUDE.md 예시

## 프로젝트 개요
데이터 수집 및 AI 분석 시스템 백엔드 (Spring Boot 3.5.7 + Java 21)

## 기술 스택
- Language: Java 21 (LTS)
- Framework: Spring Boot 3.5.7, Spring Security, Spring Data JPA
- Database: PostgreSQL 16

## 프로젝트 구조
src/main/java/io/github/cryschan/berepository/
├── _global/           # 전역 설정 및 공통 컴포넌트
└── domain/            # 도메인별 패키지 (DDD 스타일)

## 코딩 컨벤션
- Controller: {Domain}Controller
- Service: {Domain}Service
- Repository: {Domain}Repository
- Entity: 단수형 (User, Blog)
- DTO: {Domain}{Action}Request/Response
- Exception: {Domain}Exception

## 예외 처리
- 전역: GlobalExceptionHandler에서 처리
- 도메인별 예외는 DomainException 상속
- ErrorCode enum으로 에러 코드 관리

## 회의 결정사항 (날짜별)
### 2025.11.20: PostgreSQL 배열 타입 사용
- 이유: colors[], seasons[] 저장하여 JOIN 불필요
- 적용: @Column(columnDefinition = "text[]")
```

**효과**:
- Claude Code가 팀 네이밍 규칙을 자동 반영한 코드 생성
- 신규 개발자 온보딩 시간 단축 (문서화된 컨벤션 공유)
- PR 리뷰 시 코드 스타일 통일
- 팀원 간 기술적 결정 공유 및 혼동 방지

📌 **실제 적용 사례**: [BE-Repository/CLAUDE.md](https://github.com/cryschan/BE-Repository/blob/main/CLAUDE.md)

<br>

---

## 🛠️ 기술 스택

**Backend**
- Java 21, Spring Boot 3.x (JPA, Security, RESTful API)
- Python 3.11, FastAPI (비동기 웹 프레임워크)

**Database**
- PostgreSQL 16 (배열 타입, JSONB, pgvector)
- MySQL 8.0 (인덱스 최적화, 트랜잭션 관리)
- Redis 7 (캐싱)

**AI & Vector DB**
- OpenAI API (GPT-4o, GPT-3.5-turbo, 프롬프트 엔지니어링)
- ChromaDB (벡터 검색)
- LangChain, LangGraph (RAG 파이프라인)
- Spring AI 1.0.0-M4

**Web Crawling**
- JSoup (HTML 파싱)
- Spring RestClient (REST API 크롤링)
- Chrome DevTools (내부 API 발굴)

**Testing**
- JUnit 5, Mockito (Java 단위 테스트)
- pytest (Python 단위 테스트)
- 서비스 레이어 단위 테스트, 예외 케이스 검증

**Infrastructure & DevOps**
- AWS (EC2 t3.small, S3, CloudWatch)
- Docker, Docker Compose (멀티 스테이지 빌드)

**Collaboration & Tools**
- Git/GitHub (Conventional Commits: feat:, fix:, docs:)
- CodeRabbit (AI 코드 리뷰)
- Swagger (API 문서화)
- Slack (팀 커뮤니케이션, 회고)

**AI 활용 능력**
- Claude Code (Sonnet 4.5): 크롤링 로직, 테스트 코드 작성, CLAUDE.md로 프로젝트 컨텍스트 제공
- GitHub Copilot: DTO, Entity 자동 완성
- ChatGPT: 기술 스택 비교, 에러 디버깅

<br>

---

## 📬 연락처

- **Email**: jaehun1417@gmail.com
- **GitHub**: [@doublejh0501](https://github.com/doublejh0501)
- **Phone**: 010-8801-9685

<br>

---

## 📊 GitHub Stats

![doublejh0501's GitHub stats](https://github-readme-stats.vercel.app/api?username=doublejh0501&show_icons=true&theme=radical)

---

**💡 Tip**: 위 프로젝트들은 모두 실제 운영 가능한 코드이며, Docker Compose로 로컬 실행 가능합니다.
