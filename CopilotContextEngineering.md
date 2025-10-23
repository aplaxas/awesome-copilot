# VS Code GitHub Copilot Context Engineering 완전 가이드

## 📌 Context Engineering이란?

**Context Engineering**은 AI 에이전트에게 프로젝트 관련 정보를 체계적으로 제공하여 코드 생성의 품질과 정확성을 향상시키는 접근 방식입니다. 핵심은 Custom Instructions, 구현 계획, 코딩 가이드라인 등을 통해 프로젝트의 필수 컨텍스트를 큐레이션하여 AI가 더 나은 결정을 내리고, 정확도를 개선하며, 상호작용 전반에 걸쳐 지속적인 지식을 유지하도록 하는 것입니다.

---

## 🔄 Context Engineering 워크플로우

### 전체 워크플로우 개요

```
┌─────────────────────────────────────────────────────────────┐
│                   1. 프로젝트 컨텍스트 큐레이션                │
│               (Custom Instructions)                         │
│   - PRODUCT.md, ARCHITECTURE.md, CONTRIBUTING.md           │
│   - .github/copilot-instructions.md                        │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ↓
┌─────────────────────────────────────────────────────────────┐
│                   2. 구현 계획 생성                          │
│              (Chat Mode + Prompt File)                      │
│   - plan.chatmode.md (계획 전용 페르소나)                   │
│   - plan-template.md (계획 템플릿)                          │
│   - plan.prompt.md (계획 프롬프트)                          │
│                                                             │
│   📁 계획 저장 위치:                                        │
│   .github/plans/                                            │
│     ├── features/      (새 기능)                           │
│     ├── modules/       (모듈별)                            │
│     ├── refactoring/   (리팩토링)                          │
│     └── bug-fixes/     (버그 수정)                         │
│                                                             │
│   📋 자동 업데이트: README.md 인덱스                        │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ↓
┌─────────────────────────────────────────────────────────────┐
│                   3. 구현 코드 생성                          │
│              (Chat Mode + Custom Instructions)              │
│   - implement.chatmode.md (구현 전용 페르소나)              │
│   - 계획 파일 참조: #.github/plans/features/xxx-plan.md    │
│   - 코딩 가이드라인 자동 적용                                │
└─────────────────────────────────────────────────────────────┘
```

## ![Workflow](https://code.visualstudio.com/assets/docs/copilot/context-engineering-guide/context-engineering-workflow.png)

## 🎯 1단계: 프로젝트 컨텍스트 큐레이션

### 목적

AI 에이전트에게 프로젝트의 핵심 컨텍스트를 제공하여 모든 상호작용에서 일관성 있고 정확한 응답을 얻습니다.

### 왜 중요한가?

AI 에이전트가 코드베이스에서 이 정보를 찾을 수도 있지만, 주석에 묻혀있거나 여러 파일에 흩어져 있을 수 있습니다. 가장 중요한 정보를 간결하게 요약하면 AI가 의사결정에 필요한 중요한 컨텍스트를 항상 사용할 수 있습니다.

### 구현 방법

#### **Step 1: 프로젝트 문서 작성**

리포지토리에 Markdown 파일로 관련 프로젝트 문서를 작성합니다:

- PRODUCT.md의 역할 (What & Why)
- ARCHITECTURE.md의 역할 (How to Structure):
- CONTRIBUTING.md 및 copilot-instructions.md의 역할 (How to Code)

```
📁 프로젝트 루트/
├── 📁 .github/
│   └── 📄 copilot-instructions.md ← 모든 채팅에 자동 적용
│
├── 📄 PRODUCT.md        제품 기능과 비전 설명
├── 📄 ARCHITECTURE.md   전체 시스템 아키텍처, 디자인 패턴, 디자인 원칙
└── 📄 CONTRIBUTING.md   개발자 가이드라인과 베스트 프랙티스
```

💡 **팁**: 기존 코드베이스가 있다면 AI를 사용해 이러한 문서를 생성할 수 있습니다:

```
다음 프롬프트 사용:
- "Generate an ARCHITECTURE.md (max 2 page) file that describes the overall architecture of the project."
- "Generate a PRODUCT.md (max 2 page) file that describes the product functionality of the project."
- "Generate a CONTRIBUTING.md (max 1 page) file that describes developer guidelines and best practices for contributing to the project."

⚠️ 반드시 검토하고 개선하여 정확성과 완전성을 보장하세요!
```

#### **Step 2: PRODUCT.md**

- **PRODUCT.md\*\***의 역할 (What & Why)\*\*:
- **제품의 기능과 비전**
- AI가 **비즈니스 목표**와의 일치를 위한 상위 레벨 비전과 목표를 이해하도록 돕는다
- "무엇을 만들 것인가"에 대한 컨텍스트를 제공
- 최대 2페이지

Example

```markdown
# 🌟 Product Management REST API (PM-API) 제품 비전 및 기능

## 1. 제품 비전 (Vision)

PM-API는 당사의 모든 제품 데이터 관리를 위한 중앙 집중식, 고성능 RESTful 백엔드 서비스입니다. 목표는 재고, 가격, 속성 데이터를 효율적이고 안정적으로 관리할 수 있는 단일 진실 공급원(Single Source of Truth)을 제공하여, E-commerce 플랫폼 및 내부 ERP 시스템에 안정적인 제품 정보를 공급하는 것입니다.

## 2. 비즈니스 가치 및 목표

- **데이터 일관성**: 모든 다운스트림 시스템에 걸쳐 제품 정보의 일관성을 100% 보장합니다.
- **성능**: 대량의 동시 읽기 요청(Read Request)에도 낮은 지연 시간(Latency)을 유지합니다.
- **확장성**: 향후 제품 속성 및 다국어 지원 확장에 유연한 데이터 모델을 제공합니다.

## 3. 핵심 기능 (Core Functionality)

PM-API는 Product 자원(Resource)에 대한 완전한 CRUD(Create, Read, Update, Delete) 기능을 제공합니다.

### 3.1. 제품 관리 (Products)

| 기능              | 엔드포인트 예시                | 설명                                                     |
| :---------------- | :----------------------------- | :------------------------------------------------------- |
| **생성** (Create) | `POST /api/products`           | 새 제품을 시스템에 등록합니다.                           |
| **조회** (Read)   | `GET /api/products/{id}`       | 특정 제품 상세 정보를 조회합니다.                        |
| **목록** (List)   | `GET /api/products?category=X` | 페이징 및 필터링 옵션을 포함하여 제품 목록을 조회합니다. |
| **수정** (Update) | `PUT /api/products/{id}`       | 제품의 메타데이터(이름, 설명, 가격 등)를 수정합니다.     |
| **삭제** (Delete) | `DELETE /api/products/{id}`    | 제품을 비활성화하거나 삭제합니다.                        |

### 3.2. 카테고리 관리 (Categories)

- 제품을 계층적으로 분류할 수 있는 카테고리 조회 및 연결 기능을 지원합니다.
- `GET /api/categories` 엔드포인트를 통해 카테고리 트리를 제공합니다.
```

#### **Step 3: ARCHITECTURE.md**

- **ARCHITECTURE.md\*\***의 역할 (How to Structure)\*\*:
- **스템의 구조적 설계의 이유와 원칙**에 집중
- AI에게 구현 계획을 세우는 데 필요한 **깊은 추론 컨텍스트**를 제공
- 최대 2페이지
  Example

```markdown
# 🏗 PM-API 아키텍처 및 디자인 원칙

## 1. 상위 레벨 구조: 계층형 아키텍처 (Layered Architecture)

시스템은 명확한 **관심사 분리(SoC)** 원칙에 따라 세 계층으로 구성됩니다. 각 계층은 단방향 의존성(하위 계층만 참조)을 가집니다.

### 1.1. Presentation Layer (Controllers)

- **책임**: HTTP 요청 수신, 응답 반환, 인증/권한 부여(`[Authorize]` 속성 사용).
- **구조**: ASP.NET Core Controller 패턴을 사용합니다.

### 1.2. Application Layer (Services)

- **책임**: 핵심 비즈니스 로직 실행 및 조정. 여러 Infrastructure 작업을 조정하여 트랜잭션 경계를 정의합니다.
- **원칙**: 복잡한 비즈니스 규칙을 캡슐화합니다.

### 1.3. Infrastructure Layer (Repositories/Persistence)

- **책임**: 데이터베이스 및 외부 서비스 통신 처리.
- **기술**: EF Core 9를 사용하여 MS SQL Server와 상호작용합니다.

## 2. 핵심 디자인 패턴

### 2.1. Repository 패턴

데이터 접근 로직을 캡슐화하여 Application Layer가 데이터베이스 세부 사항으로부터 분리되도록 합니다. EF Core `DbContext`는 이 계층 내에서만 관리됩니다.

### 2.2. DTO (Data Transfer Object)

계층 간 또는 API 엔드포인트 간 데이터 전송 시 사용합니다. 도메인 모델의 외부 노출을 **엄격히 금지**합니다.

## 3. 데이터 관리 및 보안

### 3.1. 영속성 및 마이그레이션

- **데이터베이스**: MS SQL Server.
- **마이그레이션**: 스키마 변경 시 EF Core 마이그레이션 기능을 사용해야 합니다.

### 3.2. 보안

- **인증/권한 부여**: JWT(JSON Web Token) 기반의 인증을 사용하며, 모든 API는 Controller 수준에서 `[Authorize]` 속성을 통해 접근 제어를 수행해야 합니다.
```

#### **Step 4: CONTRIBUTING.md**

- CONTRIBUTING.md 및 copilot-instructions.md의 역할 (How to Code)
- **윤리적 지침, 협업 관행, 그리고 Context Engineering 워크플로우의 흐름** 등 인간 개발자가 따라야 할 상위 레벨 규칙에 집중
- 기술적 상세 내용(예: C# 스타일)은 `.github/copilot-instructions.md`를 참조하도록 위임
- **최대 1페이지**
  Example

```markdown
# 🤝 PM-API 프로젝트 기여 가이드라인

이 문서는 AI 지원 개발을 포함하여 모든 팀원의 협업 관행과 윤리적 지침을 정의합니다.

## 1. 협업 및 윤리 원칙

- **존중과 피드백**: 모든 코드 리뷰와 커뮤니케이션은 건설적이고 존중하는 태도로 이루어져야 합니다.
- **보안 우선**: 코드를 작성하거나 리뷰할 때 항상 보안 취약점(예: SQL Injection, XSS)을 염두에 두어야 합니다.
- **책임감**: 작성된 코드에 대한 소유권을 가지며, 배포 전에 테스트를 통과하고 아키텍처 원칙을 준수했는지 확인합니다.

## 2. 품질 및 테스트 관행

- **TDD 원칙**: 구현 시작 전 [TDD 구현 모드 (tdd.chatmode)](.github/chatmodes/tdd.chatmode.md)를 사용하여 테스트를 먼저 작성하고 최소 구현을 진행하는 원칙을 따릅니다 [7].
- **테스트 커버리지**: 모든 새로운 기능 및 수정된 코드에 대해 **최소 80%**의 단위 테스트 커버리지를 목표로 합니다 [9].
- **테스트 프레임워크**: `xUnit`을 주된 테스트 프레임워크로 사용합니다.

## 3. Context Engineering 워크플로우 (필수)

모든 주요 기능 구현은 Context Engineering 3단계 워크플로우를 따라야 합니다 [10].

### 3.1. 계획 수립 (2단계)

새 기능을 시작하기 전에 반드시 `.github/plans/` 폴더에 상세 계획 파일(`*-plan.md`)을 생성해야 합니다. 계획은 아키텍처 및 디자인 원칙([ARCHITECTURE.md])을 반영해야 합니다 [11].

### 3.2. 구현 및 참조 (3단계)된 테스트 프레임워크로 사용합니다.

## 3. Context Engineering 워크플로우 (필수)

모든 주요 기능 구현은 Context Engineering 3단계 워크플로우를 따라야 합니다 [10].

### 3.1. 계획 수립 (2단계)

새 기능을 시작하기 전에 반드시 `.github/plans/` 폴더에 상세 계획 파일(`*-plan.md`)을 생성해야 합니다. 계획은 아키텍처 및 디자인 원칙([ARCHITECTURE.md])을 반영해야 합니다 [11].

### 3.2. 구현 및 참조 (3단계)

코드 구현 시, AI 에이전트에게 **계획 파일을 참조**하도록 명시적으로 지시해야 합니다 (예: `# .github/plans/features/new-product-plan.md 구현`) [8, 12, 13].

### 3.3. 검토 및 아카이브

- **코드 리뷰**: PR 제출 전, AI에게 구현 계획 대비 코드 변경사항을 검토하도록 요청하여 품질을 검증해야 합니다 [14].
- **계획 추적**: `.github/plans/README.md` 인덱스를 통해 진행 상황을 추적하고, 완료된 계획은 정기적으로 `archived/` 폴더로 이동합니다 [15-17].

## 4. 커밋 및 PR 규칙

- **커밋 메시지**: Angular 형식(`type: scope: description`)을 따릅니다 (예: `feat: user: Add new user registration endpoint`).
- **풀 리퀘스트**: PR 제목은 변경 사항을 명확히 요약해야 하며, 관련 이슈 번호를 포함합니다.
```

#### **Step 5: `.github/copilot-instructions.md` 파일 생성**

- CONTRIBUTING.md 및 copilot-instructions.md의 역할 (How to Code)
- **모든 채팅 상호작용에 자동으로 컨텍스트로 포함**
- AI가 즉각적으로 따라야 하는 **필수 기술 제약 조건 및 핵심 원칙**을 간결하게 정의
-

```markdown
# 🤖 PM-API 핵심 컨텍스트 및 규칙

이 지침은 모든 Copilot 상호작용에 자동 적용되며, 프로젝트의 가장 중요한 기술적 제약 조건을 정의합니다.

---

## 📚 컨텍스트 문서 참조

- [제품 비전과 목표](../PRODUCT.md): 비즈니스 목표와의 일치를 위한 제품의 상위 레벨 비전과 목표 이해 [4].
- [시스템 아키텍처와 디자인 원칙](../ARCHITECTURE.md): 개발 프로세스를 안내하는 전체 시스템 아키텍처 및 디자인 원칙 [4].
- [기여 가이드라인](../CONTRIBUTING.md): 프로젝트 기여자를 위한 윤리 및 협업 관행 개요.

---

## ⚙️ 필수 기술 스택 및 코딩 제약 조건

AI 에이전트는 코드 생성 시 다음 규칙을 **최우선**으로 준수해야 합니다.

### 1. 기술 스택 강제

1.  **프레임워크**: ASP.NET Core (.NET 8)의 **전통적인 Controller 패턴**만 사용합니다. Minimal API 사용은 금지됩니다.
2.  **ORM**: **Entity Framework Core 9 (EF Core 9)**를 사용하여 데이터 접근을 처리합니다.
3.  **데이터베이스**: MS SQL Server를 사용합니다.

### 2. 핵심 코딩 관행

1.  **비동기 처리**: 모든 I/O 작업(데이터 접근 포함)에는 `async`/`await` 패턴을 필수적으로 사용합니다.
2.  **명명 규칙**: 모든 공개(`public`) 클래스, 메서드, 속성, 변수는 **PascalCase**를 사용합니다 [5].
3.  **LINQ**: 데이터 쿼리 생성 시 **쿼리 구문**을 선호합니다.

### 3. Context Engineering 워크플로우

1.  **계획 필수**: 모든 새 기능은 `.github/plans/` 폴더 내의 상세 계획 파일(`*-plan.md`)에서 시작해야 합니다 [6].
2.  **구현 모드**: 코드 구현 시 **TDD 원칙**에 따라 테스트를 먼저 작성해야 합니다 [7].
```

**파일 구조:**

```
📁 프로젝트 루트/
├── 📁 .github/
│   └── 📄 copilot-instructions.md ← 모든 채팅에 자동 적용
│
├── 📄 PRODUCT.md        제품 기능과 비전 설명
├── 📄 ARCHITECTURE.md   전체 시스템 아키텍처, 디자인 패턴, 디자인 원칙
└── 📄 CONTRIBUTING.md   개발자 가이드라인과 베스트 프랙티스
```

#### **Step 6: 작게 시작하기**

⚡ **핵심 원칙:**

- ✅ 초기 프로젝트 전반 컨텍스트는 **간결하고 가장 중요한 정보에 집중**
- ✅ 불확실하면 **상위 레벨 아키텍처에만 집중**
- ✅ AI가 **반복적으로 저지르는 오류**를 해결하기 위해서만 새로운 규칙 추가
  - 예: 잘못된 쉘 명령 사용, 특정 파일 무시

---

## 🗺️ 2단계: 구현 계획 생성

### 목적

새로운 기능이나 버그 수정을 위한 상세한 구현 계획을 AI와 함께 생성하고 체계적으로 관리합니다.

### 왜 중요한가?

구현 계획은 코드 생성의 청사진 역할을 하며, 계획 단계와 구현 단계를 분리하여 더 나은 의사결정과 일관성 있는 코드 생성을 가능하게 합니다. 계획 파일들을 구조화하여 관리하면 팀 전체가 프로젝트의 진행 상황을 명확히 파악할 수 있습니다.

### Custom Chat Mode로 계획 페르소나 만들기

**Custom Chat Mode**를 사용하면:

- 계획 전용 가이드라인과 도구(예: 코드베이스에 대한 **읽기 전용 액세스**)를 갖춘 전용 페르소나 생성
- 브레인스토밍, 연구, 협업을 위한 특정 워크플로우 캡처
- 생성된 계획을 적절한 위치에 자동 저장

---

## 📁 계획 파일 관리 구조

### 권장 폴더 구조

```
📁 프로젝트 루트/
├── 📁 .github/
│   ├── 📄 copilot-instructions.md
│   │
│   ├── 📁 chatmodes/
│   │   ├── 📄 plan.chatmode.md
│   │   └── 📄 tdd.chatmode.md
│   │
│   ├── 📁 prompts/
│   │   └── 📄 plan-qna.prompt.md
│   │
│   └── 📁 plans/                        ← 계획 파일 전용 폴더
│       ├── 📄 README.md                  (계획 인덱스)
│       ├── 📄 _plan-template.md          (템플릿)
│       │
│       ├── 📁 features/                  (기능별)
│       │   ├── 📄 user-auth-plan.md
│       │   ├── 📄 payment-integration-plan.md
│       │   └── 📄 notification-system-plan.md
│       │
│       ├── 📁 modules/                   (모듈별)
│       │   ├── 📄 approval-general-plan.md
│       │   ├── 📄 approval-expense-plan.md
│       │   └── 📄 approval-vacation-plan.md
│       │
│       ├── 📁 refactoring/              (리팩토링)
│       │   ├── 📄 database-migration-plan.md
│       │   └── 📄 api-v2-migration-plan.md
│       │
│       ├── 📁 bug-fixes/                (버그 수정)
│       │   ├── 📄 memory-leak-fix-plan.md
│       │   └── 📄 auth-timeout-fix-plan.md
│       │
│       └── 📁 archived/                 (완료된 계획)
│           └── 📁 2025-Q1/
│               ├── 📄 initial-setup-plan.md
│               └── 📄 mvp-launch-plan.md
│
├── 📄 PRODUCT.md
├── 📄 ARCHITECTURE.md
└── 📄 CONTRIBUTING.md
```

---

## 📋 계획 관리 시스템

### 1. **계획 인덱스 파일 (README.md)**

`.github/plans/README.md` 파일로 모든 계획을 추적합니다:

````markdown
# Implementation Plans Index

이 디렉토리는 프로젝트의 모든 구현 계획을 관리합니다.

## 📊 현재 진행 상황

### 🚧 진행 중 (In Progress)

| 계획                                                | 상태 | 담당자 | 시작일     | 예상 완료  |
| --------------------------------------------------- | ---- | ------ | ---------- | ---------- |
| [사용자 인증](./features/user-auth-plan.md)         | 70%  | @john  | 2025-01-15 | 2025-01-30 |
| [결제 통합](./features/payment-integration-plan.md) | 40%  | @sarah | 2025-01-20 | 2025-02-10 |

### 📝 계획됨 (Planned)

| 계획                                                  | 우선순위 | 예상 시작  |
| ----------------------------------------------------- | -------- | ---------- |
| [알림 시스템](./features/notification-system-plan.md) | High     | 2025-02-01 |
| [일반 결재](./modules/approval-general-plan.md)       | Medium   | 2025-02-15 |

### ✅ 완료됨 (Completed)

| 계획                                                  | 완료일     | 담당자 |
| ----------------------------------------------------- | ---------- | ------ |
| [초기 설정](./archived/2025-Q1/initial-setup-plan.md) | 2025-01-10 | @mike  |

---

## 📂 폴더 구조

### `/features` - 새로운 기능

주요 사용자 대면 기능의 구현 계획

### `/modules` - 모듈별 기능

특정 비즈니스 모듈(결재, 인사, 회계 등)의 기능 계획

### `/refactoring` - 리팩토링

기존 코드 개선 및 기술 부채 해결 계획

### `/bug-fixes` - 버그 수정

복잡한 버그 수정을 위한 상세 계획

### `/archived` - 아카이브

완료된 계획 (분기별로 정리)

---

## 🔄 워크플로우

### 1. 새 계획 생성

```bash
# plan.chatmode로 계획 생성
/plan-qna 알림 시스템 추가

# 생성된 계획을 적절한 폴더에 저장
.github/plans/features/notification-system-plan.md
```
````

### 2. 계획 상태 업데이트

```bash
# README.md의 진행 상황 테이블 업데이트
# 계획 파일의 메타데이터 업데이트
```

### 3. 구현 시작

```bash
# tdd.chatmode로 구현
#.github/plans/features/notification-system-plan.md 구현
```

### 4. 완료 후 아카이브

```bash
# 완료된 계획을 archived로 이동
mv .github/plans/features/user-auth-plan.md \
   .github/plans/archived/2025-Q1/
```

---

## 📝 계획 파일 명명 규칙

### 형식

`<category>-<description>-plan.md`

### 예시

- `user-auth-plan.md` (기능: 사용자 인증)
- `approval-expense-plan.md` (모듈: 지출 결재)
- `database-migration-plan.md` (리팩토링: DB 마이그레이션)
- `memory-leak-fix-plan.md` (버그: 메모리 누수 수정)

### 규칙

- 소문자 사용
- 하이픈(-)으로 단어 구분
- 항상 `-plan.md`로 끝남
- 간결하고 설명적인 이름

---

## 🏷️ 계획 메타데이터 표준

모든 계획 파일은 다음 메타데이터를 포함해야 합니다:

```markdown
---
title: 사용자 인증 시스템
category: feature
priority: high
status: in-progress
assignee: @john
github_issue: #123
start_date: 2025-01-15
target_date: 2025-01-30
version: 1.2
last_updated: 2025-01-22
---
```

---

## 🔍 계획 검색

### GitHub 코드 검색 활용

```bash
# 특정 키워드로 계획 검색
# GitHub에서: path:.github/plans keyword

# 진행 중인 계획만
status:in-progress path:.github/plans

# 높은 우선순위 계획
priority:high path:.github/plans
```

### 로컬 검색

```bash
# grep으로 검색
grep -r "OAuth" .github/plans/

# 특정 상태의 계획
grep -r "status: in-progress" .github/plans/
```

````

---

## 🛠️ 구현 방법

### **Step 1: 계획 문서 템플릿 생성**

`.github/plans/_plan-template.md` 파일을 만들어 구현 계획 문서의 구조와 섹션을 정의합니다:

```markdown
---
title: [기능의 짧은 설명 제목]
category: [feature | module | refactoring | bug-fix]
priority: [high | medium | low]
status: planned
assignee: [@username]
github_issue: [#123 또는 N/A]
start_date: [YYYY-MM-DD]
target_date: [YYYY-MM-DD]
version: 1.0
date_created: [YYYY-MM-DD]
last_updated: [YYYY-MM-DD]
---

# 구현 계획: <기능 이름>

## 📋 개요
[기능의 요구사항과 목표에 대한 간략한 설명]

**비즈니스 가치:**
- 왜 이 기능이 필요한가?
- 예상되는 사용자 혜택은?

**성공 기준:**
- 어떻게 성공을 측정할 것인가?

---

## 🏗️ 아키텍처와 디자인

### 전체 설계
[상위 레벨 아키텍처와 디자인 고려사항 설명]

### 주요 컴포넌트
1. **컴포넌트 A**: 설명
2. **컴포넌트 B**: 설명

### 데이터 모델
[필요한 데이터베이스 스키마 변경사항]

### API 설계
[새로운 API 엔드포인트 또는 변경사항]

### 의존성
- 기존 코드와의 통합 포인트
- 외부 라이브러리/서비스
- 다른 기능/모듈과의 의존성

---

## ✅ 작업 분해

구현을 더 작고 관리 가능한 작업으로 분해 (Markdown 체크리스트 형식 사용).

### Phase 1: 기반 구조
- [ ] 작업 1: 설명 (예상 시간: 2h)
- [ ] 작업 2: 설명 (예상 시간: 3h)

### Phase 2: 핵심 기능
- [ ] 작업 3: 설명 (예상 시간: 4h)
- [ ] 작업 4: 설명 (예상 시간: 2h)

### Phase 3: 통합 및 테스트
- [ ] 작업 5: 설명 (예상 시간: 3h)
- [ ] 작업 6: 설명 (예상 시간: 2h)

**총 예상 시간:** [XX시간]

---

## 🧪 테스트 전략

### 단위 테스트
- 테스트해야 할 핵심 로직
- 엣지 케이스

### 통합 테스트
- 통합 포인트 테스트
- API 테스트

### E2E 테스트
- 주요 사용자 시나리오

**목표 테스트 커버리지:** 80%+

---

## 🔒 보안 고려사항

- 인증/권한 부여
- 데이터 검증
- 보안 취약점

---

## 📊 성능 고려사항

- 예상 부하
- 최적화 전략
- 모니터링 포인트

---

## 🚀 배포 계획

### 마이그레이션
- [ ] 데이터베이스 마이그레이션 스크립트
- [ ] 롤백 계획

### 배포 단계
1. 개발 환경
2. 스테이징 환경
3. 프로덕션 환경

### Feature Flag
- 단계적 롤아웃 전략

---

## ❓ 미해결 질문

명확히 해야 할 1-3개의 미해결 질문이나 불확실성:

1. **질문 1?**
   - 영향: [설명]
   - 결정 필요: [언제까지]

2. **질문 2?**
   - 영향: [설명]
   - 결정 필요: [언제까지]

---

## 📚 참고 자료

- [관련 문서](링크)
- [디자인 목업](링크)
- [API 문서](링크)

---

## 📝 변경 이력

### v1.0 - 2025-01-20
- 초기 계획 생성

### v1.1 - 2025-01-22
- 보안 고려사항 추가
- 작업 시간 추정 업데이트
````

---

### **Step 2: 계획 Chat Mode 생성**

`.github/chatmodes/plan.chatmode.md` 파일을 만들어 계획 페르소나를 정의합니다.

**생성 방법:**

```
Command Palette (⇧⌘P / Ctrl+Shift+P) 실행
→ "Chat: Configure Chat Modes"
→ "Create New custom chat mode file" 선택
```

```markdown
---
description: "상세한 구현 계획을 작성하는 아키텍트 및 플래너."
tools: ["fetch", "githubRepo", "problems", "usages", "search", "todos", "get_issue", "get_issue_comments", "list_issues"]
model: Claude Sonnet 4
---

# 계획 모드

당신은 새 기능과 버그 수정을 위한 상세하고 포괄적인 구현 계획을 작성하는 데 집중하는 아키텍트입니다. 목표는 복잡한 요구사항을 개발자가 쉽게 이해하고 실행할 수 있는 명확하고 실행 가능한 작업으로 분해하는 것입니다.

## 워크플로우

1. **분석 및 이해**: 코드베이스와 제공된 문서에서 컨텍스트를 수집하여 요구사항과 제약사항을 완전히 이해합니다.

2. **카테고리 결정**: 계획의 유형을 결정합니다:

   - **feature**: 새로운 사용자 대면 기능
   - **module**: 특정 비즈니스 모듈의 기능
   - **refactoring**: 코드 개선 및 기술 부채 해결
   - **bug-fix**: 복잡한 버그 수정

3. **계획 구조화**: 제공된 [구현 계획 템플릿](.github/plans/_plan-template.md)을 사용하여 계획을 구조화합니다.

4. **파일 저장**: 완성된 계획을 적절한 위치에 저장합니다:

   - Features: `.github/plans/features/<name>-plan.md`
   - Modules: `.github/plans/modules/<module>-<name>-plan.md`
   - Refactoring: `.github/plans/refactoring/<name>-plan.md`
   - Bug fixes: `.github/plans/bug-fixes/<name>-fix-plan.md`

5. **인덱스 업데이트**: 계획 생성 후 `.github/plans/README.md`의 진행 상황 테이블에 새 계획을 추가할 것을 제안합니다.

6. **검토를 위한 일시정지**: 사용자 피드백이나 질문에 따라 필요한 대로 계획을 반복하고 개선합니다.

## 계획 파일 명명 규칙

- 소문자 사용
- 하이픈(-)으로 단어 구분
- 카테고리를 반영한 설명적 이름
- 항상 `-plan.md`로 끝남

예시:

- `user-authentication-plan.md`
- `approval-expense-plan.md`
- `database-migration-plan.md`
- `memory-leak-fix-plan.md`

## 메타데이터 필수 항목

모든 계획은 다음 메타데이터를 포함해야 합니다:

- title: 계획 제목
- category: feature | module | refactoring | bug-fix
- priority: high | medium | low
- status: planned | in-progress | completed
- github_issue: 관련 이슈 번호 (있는 경우)
- start_date: 시작일 (YYYY-MM-DD)
- target_date: 목표 완료일 (YYYY-MM-DD)
```

💡 **중요 메타데이터:**

- `model: Claude Sonnet 4` - 추론과 깊은 이해에 최적화된 언어 모델
- `tools` - GitHub 이슈에 액세스하려면 GitHub MCP 서버 설치 필요

---

### **Step 3: 계획 모드 사용**

Chat view에서 **plan** 채팅 모드를 선택하고 기능 구현 작업을 입력합니다:

```
✅ 기본 사용:
"이메일과 비밀번호를 사용한 사용자 인증 추가, 등록, 로그인, 로그아웃, 비밀번호 재설정 기능 포함"

✅ GitHub 이슈 참조:
"이슈 #43에서 기능 구현"
```

AI는 자동으로:

1. 계획을 생성
2. 적절한 폴더에 파일 저장 (예: `.github/plans/features/user-auth-plan.md`)
3. README.md 업데이트 제안

---

### **Step 4 (선택사항): 명확화 Prompt File 추가**

`.github/prompts/plan-qna.prompt.md` 파일을 만들어 계획 모드를 호출하고 명확화 단계를 추가합니다:

```markdown
---
mode: plan
description: 상세한 구현 계획 작성.
---

내 기능 요청을 간략히 분석한 후, 요구사항을 명확히 하기 위해 3가지 질문을 하세요. 그런 다음에만 계획 워크플로우를 시작하세요.
```

**사용 방법:**

```
Chat view에서 슬래시 커맨드 사용:
/plan-qna 고객 정보를 표시하고 편집하는 고객 상세 페이지 추가
```

AI가 구현 계획을 작성하기 전에 오해를 줄이기 위한 **3가지 명확화 질문**을 먼저 합니다.

---

## 🤖 자동화 도구 (선택사항)

### 계획 관리 스크립트

#### `scripts/plan-manager.sh`

```powershell
# plan-manager.ps1
# 계획 관리 유틸리티 스크립트

param(
    [Parameter(Position=0)]
    [string]$Command,

    [Parameter(Position=1)]
    [string]$Arg1,

    [Parameter(Position=2)]
    [string]$Arg2
)

$PLANS_DIR = ".github/plans"

# 새 계획 생성
function Create-Plan {
    param(
        [string]$Name,
        [string]$Category
    )

    if ([string]::IsNullOrEmpty($Name) -or [string]::IsNullOrEmpty($Category)) {
        Write-Host "Usage: .\plan-manager.ps1 create <plan-name> <category>"
        Write-Host "Categories: features, modules, refactoring, bug-fixes"
        exit 1
    }

    $filename = "${Name}-plan.md"
    $filepath = Join-Path $PLANS_DIR $Category $filename

    # 디렉토리가 없으면 생성
    $directory = Split-Path $filepath -Parent
    if (-not (Test-Path $directory)) {
        New-Item -ItemType Directory -Path $directory -Force | Out-Null
    }

    # 템플릿 복사
    $templatePath = Join-Path $PLANS_DIR "_plan-template.md"
    Copy-Item $templatePath $filepath

    # 메타데이터 자동 채우기
    $today = Get-Date -Format "yyyy-MM-dd"
    $content = Get-Content $filepath -Raw
    $content = $content -replace "date_created: \[YYYY-MM-DD\]", "date_created: $today"
    $content = $content -replace "last_updated: \[YYYY-MM-DD\]", "last_updated: $today"
    Set-Content $filepath $content

    Write-Host "✅ 계획 생성됨: $filepath" -ForegroundColor Green
    Write-Host "📝 다음 단계:"
    Write-Host "   1. 계획 파일 편집"
    Write-Host "   2. ${PLANS_DIR}\README.md 업데이트"
}

# 계획 아카이브
function Archive-Plan {
    param(
        [string]$FilePath
    )

    if ([string]::IsNullOrEmpty($FilePath)) {
        Write-Host "Usage: .\plan-manager.ps1 archive <plan-filepath>"
        exit 1
    }

    if (-not (Test-Path $FilePath)) {
        Write-Host "❌ 파일을 찾을 수 없습니다: $FilePath" -ForegroundColor Red
        exit 1
    }

    $date = Get-Date
    $quarter = "Q$([Math]::Ceiling($date.Month / 3))"
    $year = $date.Year
    $quarterFolder = "${year}-${quarter}"
    $archiveDir = Join-Path $PLANS_DIR "archived" $quarterFolder

    # 아카이브 디렉토리 생성
    if (-not (Test-Path $archiveDir)) {
        New-Item -ItemType Directory -Path $archiveDir -Force | Out-Null
    }

    # 완료 날짜 추가
    $today = Get-Date -Format "yyyy-MM-dd"
    $content = Get-Content $FilePath -Raw
    $content = $content -replace "status: in-progress", "status: completed"
    $content = $content -replace "status: planned", "status: completed"
    $content += "`n`n## ✅ 완료`n완료일: ${today}"
    Set-Content $FilePath $content

    # 파일 이동
    $fileName = Split-Path $FilePath -Leaf
    $destination = Join-Path $archiveDir $fileName
    Move-Item $FilePath $destination -Force

    Write-Host "✅ 계획이 아카이브됨: $destination" -ForegroundColor Green
    Write-Host "📝 ${PLANS_DIR}\README.md를 업데이트하세요"
}

# 계획 목록
function List-Plans {
    param(
        [string]$Status
    )

    Write-Host "📋 계획 목록" -ForegroundColor Cyan
    Write-Host "===================="

    $planFiles = Get-ChildItem -Path $PLANS_DIR -Filter "*-plan.md" -Recurse |
                 Where-Object { $_.FullName -notmatch "\\archived\\" }

    foreach ($file in $planFiles) {
        $content = Get-Content $file.FullName -Raw

        if ($content -match "status:\s*(\S+)") {
            $fileStatus = $matches[1]
        } else {
            $fileStatus = "unknown"
        }

        if ($content -match "title:\s*(.+)") {
            $title = $matches[1].Trim()
        } else {
            $title = $file.BaseName
        }

        if ([string]::IsNullOrEmpty($Status) -or $fileStatus -eq $Status) {
            Write-Host "[$fileStatus] $title" -ForegroundColor Yellow
            Write-Host "  → $($file.FullName)" -ForegroundColor Gray
            Write-Host ""
        }
    }
}

# 계획 상태 업데이트
function Update-Status {
    param(
        [string]$FilePath,
        [string]$NewStatus
    )

    if ([string]::IsNullOrEmpty($FilePath) -or [string]::IsNullOrEmpty($NewStatus)) {
        Write-Host "Usage: .\plan-manager.ps1 update-status <plan-filepath> <status>"
        Write-Host "Status: planned, in-progress, completed"
        exit 1
    }

    if (-not (Test-Path $FilePath)) {
        Write-Host "❌ 파일을 찾을 수 없습니다: $FilePath" -ForegroundColor Red
        exit 1
    }

    $content = Get-Content $FilePath -Raw
    $content = $content -replace "status:\s*\S+", "status: $NewStatus"

    $today = Get-Date -Format "yyyy-MM-dd"
    $content = $content -replace "last_updated:\s*.+", "last_updated: $today"

    Set-Content $FilePath $content

    Write-Host "✅ 상태 업데이트됨: $NewStatus" -ForegroundColor Green
}

# 도움말 표시
function Show-Help {
    Write-Host "Usage: .\plan-manager.ps1 <command> [args]"
    Write-Host ""
    Write-Host "Commands:"
    Write-Host "  create <name> <category>              새 계획 생성"
    Write-Host "  archive <filepath>                    계획 아카이브"
    Write-Host "  list [status]                         계획 목록 (planned|in-progress|completed)"
    Write-Host "  update-status <filepath> <status>     상태 업데이트"
    Write-Host ""
    Write-Host "Examples:"
    Write-Host "  .\plan-manager.ps1 create user-profile features"
    Write-Host "  .\plan-manager.ps1 list in-progress"
    Write-Host "  .\plan-manager.ps1 archive .github\plans\features\user-auth-plan.md"
    Write-Host "  .\plan-manager.ps1 update-status .github\plans\features\user-auth-plan.md completed"
}

# 명령 처리
switch ($Command) {
    "create" {
        Create-Plan -Name $Arg1 -Category $Arg2
    }
    "archive" {
        Archive-Plan -FilePath $Arg1
    }
    "list" {
        List-Plans -Status $Arg1
    }
    "update-status" {
        Update-Status -FilePath $Arg1 -NewStatus $Arg2
    }
    default {
        Show-Help
        exit 1
    }
}
```

**사용 예시:**

```powershell
# 새 계획 생성
.\plan-manager.ps1 create user-profile features

# 진행 중인 계획 목록
.\plan-manager.ps1 list in-progress

# 계획 아카이브
.\plan-manager.ps1 archive .github\plans\features\user-auth-plan.md

# 상태 업데이트
.\plan-manager.ps1 update-status .github\plans\features\user-auth-plan.md completed
```

---

### GitHub Actions 자동화 (선택사항)

계획 파일 변경 시 자동으로 README.md 검증:

#### `.github/workflows/validate-plans.yml`

```yaml
name: Validate Plans

on:
  pull_request:
    paths:
      - ".github/plans/**/*.md"

jobs:
  validate:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Check plan metadata
        run: |
          echo "🔍 Checking plan files..."

          for file in $(find .github/plans -name "*-plan.md" -not -path "*/archived/*"); do
            echo "Checking $file"
            
            # 필수 메타데이터 확인
            if ! grep -q "^title:" "$file"; then
              echo "❌ Missing 'title' in $file"
              exit 1
            fi
            
            if ! grep -q "^category:" "$file"; then
              echo "❌ Missing 'category' in $file"
              exit 1
            fi
            
            if ! grep -q "^status:" "$file"; then
              echo "❌ Missing 'status' in $file"
              exit 1
            fi
            
            echo "✅ $file is valid"
          done

          echo "✅ All plan files are valid"

      - name: Check README is updated
        run: |
          echo "🔍 Checking if README.md is updated..."

          # 새로 추가된 계획 파일 확인
          new_plans=$(git diff --name-only origin/main...HEAD | grep "\.github/plans/.*-plan\.md" | grep -v archived || true)

          if [ -n "$new_plans" ]; then
            echo "📝 New plans detected:"
            echo "$new_plans"
            
            # README.md가 변경되었는지 확인
            if ! git diff --name-only origin/main...HEAD | grep -q ".github/plans/README.md"; then
              echo "⚠️  Please update .github/plans/README.md with the new plans"
              exit 1
            fi
          fi

          echo "✅ README.md is properly updated"
```

---

## 💻 3단계: 구현 코드 생성

### 목적

2단계에서 생성한 구현 계획을 기반으로 실제 코드를 생성하며, 1단계의 코딩 가이드라인이 자동으로 적용됩니다.

### 왜 중요한가?

구현 단계를 계획 단계와 분리하고, 전용 구현 페르소나를 사용하여 일관성 있고 테스트된 코드를 생성합니다.

### 구현 방법

#### **방법 1: 직접 구현 (작은 작업)**

구현 계획을 기반으로 코드를 생성하도록 에이전트에 직접 프롬프트:

```
✅ Agent 모드에서:
"#.github/plans/features/user-auth-plan.md 구현"

또는

"user-authentication-plan.md에 기반하여 사용자 인증 기능 구현"
```

---

#### **방법 2: 계획 저장 후 구현 (크거나 복잡한 기능)**

**단계별 프로세스:**

```
1️⃣ Agent 모드로 전환

2️⃣ 구현 계획을 파일로 저장 (plan 모드에서 이미 자동 저장됨)
   계획은 이미 .github/plans/ 폴더에 저장되어 있음

3️⃣ 새 채팅 시작 (⌘N / Ctrl+N)
   → 채팅 컨텍스트 재설정

4️⃣ 구현 계획 파일 참조하여 구현
   프롬프트: "#.github/plans/features/user-auth-plan.md 구현"
```

💡 **팁**:

- **Agent 모드**는 다단계 작업을 실행하고 계획과 프로젝트 컨텍스트를 기반으로 목표를 달성하는 최선의 방법을 파악하는 데 최적화
- 계획 파일을 제공하거나 프롬프트에서 참조하기만 하면 됨

---

#### **방법 3: TDD 구현 Chat Mode 생성 (권장)**

`.github/chatmodes/tdd.chatmode.md` 또는 `implement.chatmode.md` 파일로 테스트 주도 구현에 특화된 커스텀 채팅 모드 생성:

```markdown
---
description: "상세한 구현 계획을 테스트 주도 개발자로 실행."
model: GPT-4o
---

# TDD 구현 모드

주어진 구현 계획에 대해 고품질의 완전히 테스트된 유지보수 가능한 코드를 생성하는 전문 TDD 개발자입니다.

## 테스트 주도 개발

1. **테스트 우선 작성**: 수용 기준과 예상 동작을 인코딩하기 위해 먼저 테스트 작성/업데이트
2. **최소 구현**: 테스트 요구사항을 충족하는 최소한의 코드 구현
3. **즉시 테스트 실행**: 각 변경 후 즉시 대상 테스트 실행
4. **전체 테스트 스위트 실행**: 다음 작업으로 이동하기 전에 회귀를 포착하기 위해 전체 테스트 스위트 실행
5. **리팩토링**: 모든 테스트를 그린으로 유지하면서 리팩토링

## 핵심 원칙

- **점진적 진행**: 시스템이 작동하는 상태를 유지하는 작고 안전한 단계
- **테스트 주도**: 테스트가 동작을 안내하고 검증
- **품질 중심**: 기존 패턴과 관례 준수

## 성공 기준

- ✅ 계획된 모든 작업 완료
- ✅ 각 작업에 대한 수용 기준 충족
- ✅ 테스트 통과 (단위, 통합, 전체 스위트)
```

**사용 방법:**

```
1. Chat view에서 "tdd" 모드 선택
2. 프롬프트 입력:
   "#.github/plans/features/user-auth-plan.md를 기반으로 구현해줘"
```

💡 **중요**:

- 더 작은 언어 모델은 명시적 지침에 따라 코드를 생성하는 데 탁월
- `implement` 채팅 모드는 `model` 속성을 설정하면 이점 (예: `model: GPT-4o`)

---

#### **코드 리뷰 단계 (권장)**

새로운 페어 프로그래머 역할로 AI 활용:

```
1️⃣ 새 채팅 시작 (⌘N / Ctrl+N)

2️⃣ 리뷰 요청 프롬프트:
   "#.github/plans/features/user-auth-plan.md 대비 코드 변경사항을 검토해줘.
    놓친 요구사항이나 불일치가 있는지 확인해줘."

3️⃣ AI가 다음을 확인:
   ✅ 모든 계획된 작업 완료 여부
   ✅ 수용 기준 충족 여부
   ✅ 코딩 가이드라인 준수 여부
   ✅ 잠재적 버그나 개선 사항
```

---

## 🎯 완전한 워크플로우 예시

### 실제 사용 시나리오

```bash
# 1. 새 기능 계획 생성
Chat: plan 모드 선택
입력: /plan-qna 알림 시스템 추가

AI 응답:
"명확화 질문 3개..."
[사용자 답변]
"계획 생성 중..."
✅ 계획이 .github/plans/features/notification-system-plan.md에 저장되었습니다.
📝 .github/plans/README.md를 업데이트하는 것을 권장합니다.

# 2. README 업데이트 (자동 또는 수동)
[README.md의 "계획됨" 테이블에 추가]

# 3. 구현 시작
Chat: tdd 모드 선택
입력: #.github/plans/features/notification-system-plan.md 구현

AI 응답:
"계획을 분석했습니다. Phase 1부터 시작하겠습니다.
먼저 알림 모델 테스트를 작성하겠습니다..."

# 4. 진행 중 상태 업데이트
./scripts/plan-manager.sh update-status \
    .github/plans/features/notification-system-plan.md in-progress

# 5. 완료 후 상태 업데이트
./scripts/plan-manager.sh update-status \
    .github/plans/features/notification-system-plan.md completed

# 6. 아카이브
./scripts/plan-manager.sh archive \
    .github/plans/features/notification-system-plan.md

✅ 계획이 .github/plans/archived/2025-Q1/로 이동되었습니다.
```

---

## 📚 4가지 핵심 커스터마이제이션 도구 상세 설명

### 1️⃣ **Custom Instructions** (커스텀 지침)

프로젝트 전반에 걸쳐 지속적으로 적용되는 가이드라인과 규칙을 정의합니다.

#### **두 가지 유형**

1. **`.github/copilot-instructions.md`**

   - ✅ 워크스페이스의 **모든 채팅 요청에 자동 적용**
   - 워크스페이스 내에 저장
   - **모든 단계에서 자동으로 컨텍스트로 포함**

2. **`*.instructions.md` 파일들**

   - 특정 작업이나 파일에 대해 생성
   - `applyTo` frontmatter를 사용해 어떤 파일에 적용할지 정의
   - 워크스페이스 또는 사용자 프로필에 저장

#### **Instructions 파일 구조**

```markdown
---
applyTo: "**/*.py"
description: "Python 코딩 표준"
---

# 프로젝트 Python 코딩 표준

- PEP 8 스타일 가이드 준수
- 가독성과 명확성 우선
- 각 함수에 명확하고 간결한 주석 작성
- 함수 이름은 설명적이며 타입 힌트 포함
- 적절한 들여쓰기 유지 (레벨당 4칸)
```

#### **워크플로우에서의 역할**

```
1단계에서 생성 → 2단계와 3단계에서 자동 적용

.github/copilot-instructions.md
    ↓ (자동 주입)
plan.chatmode.md 실행 시
    ↓ (자동 주입)
tdd.chatmode.md 실행 시
```

#### **언제 사용하나?**

- ✅ 코딩 관행, 선호 기술, 프로젝트 요구사항 지정
- ✅ 커밋 메시지나 PR 제목/설명의 구조 가이드라인 제공
- ✅ 코드 리뷰 규칙 설정 (보안 취약점, 성능 문제, 코딩 표준 준수 확인)

#### **예시: 언어별 가이드라인**

```markdown
---
applyTo: "**/*.ts,**/*.tsx"
---

# TypeScript와 React를 위한 프로젝트 코딩 표준

[일반 코딩 가이드라인](./general-coding.instructions.md)을 모든 코드에 적용.

## TypeScript 가이드라인

- 모든 새 코드에 TypeScript 사용
- 가능한 경우 함수형 프로그래밍 원칙 따르기
- 데이터 구조와 타입 정의에 인터페이스 사용
- 불변 데이터 선호 (const, readonly)
- 옵셔널 체이닝(?.)과 nullish 병합(??) 연산자 사용

## React 가이드라인

- Hook을 사용한 함수형 컴포넌트 사용
- React hooks 규칙 준수 (조건부 hook 금지)
- children이 있는 컴포넌트에 React.FC 타입 사용
- 컴포넌트를 작고 집중되게 유지
- 컴포넌트 스타일링에 CSS 모듈 사용
```

#### **생성 방법**

```
1️⃣ Chat view → Configure Chat → Instructions → New instruction file
   또는
   Command Palette → "Chat: New Instructions File"

2️⃣ 위치 선택
   - Workspace: .github/instructions/
   - User Profile: 모든 워크스페이스에서 사용 가능

3️⃣ 파일 이름 입력

4️⃣ Markdown으로 커스텀 지침 작성

5️⃣ applyTo 메타데이터 속성으로 자동 적용 시기 구성
```

#### **팁**

- ✅ 지침을 짧고 자체 완결적으로 유지
- ✅ 작업이나 언어별 지침의 경우 여러 `*.instructions.md` 파일 사용하고 `applyTo` 속성으로 선택적 적용
- ✅ 프로젝트별 지침은 워크스페이스에 저장하여 팀원들과 공유하고 버전 관리에 포함

---

### 2️⃣ **Prompt Files** (프롬프트 파일)

공통 개발 작업을 위한 재사용 가능한 프롬프트를 정의합니다.

#### **Prompt Files vs Custom Instructions**

| 특징                | Prompt Files                       | Custom Instructions          |
| ------------------- | ---------------------------------- | ---------------------------- |
| **적용 방식**       | 온디맨드 (특정 작업 시)            | 자동 (모든 요청)             |
| **목적**            | 특정 반복 작업 자동화              | 일관된 가이드라인 제공       |
| **사용 방법**       | 슬래시 커맨드로 호출 (`/plan-qna`) | 백그라운드에서 자동 적용     |
| **워크플로우 역할** | 2단계에서 계획 프로세스 시작       | 1단계에서 전체 컨텍스트 제공 |

#### **워크플로우에서의 역할**

```
2단계: 구현 계획 생성

plan-qna.prompt.md
    ↓ (호출)
plan.chatmode.md
    ↓ (적용)
copilot-instructions.md (자동)
    ↓
명확화 질문 → 계획 생성 → 적절한 폴더에 저장
```

#### **Prompt 파일 구조**

```markdown
---
mode: "agent"
model: GPT-4o
tools: ["githubRepo", "codebase"]
description: "새 React 폼 컴포넌트 생성"
---

목표는 #githubRepo contoso/react-templates의 템플릿을 기반으로 새 React 폼 컴포넌트를 생성하는 것입니다.

제공되지 않은 경우 폼 이름과 필드를 요청하세요.

폼 요구사항:

- 폼 디자인 시스템 컴포넌트 사용: [design-system/Form.md](../docs/design-system/Form.md)
- 폼 상태 관리에 `react-hook-form` 사용
- 폼 데이터에 대해 항상 TypeScript 타입 정의
- register를 사용하는 _비제어_ 컴포넌트 선호
- 불필요한 리렌더링 방지를 위해 `defaultValues` 사용
- 검증에 `yup` 사용
```

#### **메타데이터 속성**

- `description`: 프롬프트의 짧은 설명
- `mode`: 프롬프트 실행에 사용할 채팅 모드 (`ask`, `edit`, `agent`)
- `model`: 사용할 언어 모델 (미지정시 현재 선택된 모델 사용)
- `tools`: 사용할 수 있는 도구(세트) 이름 배열

#### **변수 사용**

프롬프트 파일 내에서 `${variableName}` 구문으로 변수 참조:

```markdown
워크스페이스 변수:

- ${workspaceFolder}
- ${workspaceFolderBasename}

선택 변수:

- ${selection}
- ${selectedText}

파일 컨텍스트 변수:

- ${file}
- ${fileBasename}
- ${fileDirname}
- ${fileBasenameNoExtension}

입력 변수:

- ${input:variableName}
- ${input:variableName:placeholder}
```

#### **사용 방법**

```
방법 1: Chat view에서 슬래시 커맨드
→ /plan-qna 고객 상세 페이지 추가

방법 2: Command Palette
→ "Chat: Run Prompt" → plan-qna 선택

방법 3: 에디터에서 직접 실행
→ plan-qna.prompt.md 열기 → 재생 버튼 클릭
```

---

### 3️⃣ **Custom Chat Modes** (커스텀 채팅 모드)

특정 작업이나 역할에 맞게 채팅 동작을 조정하는 전문 AI 에이전트를 생성합니다.

#### **내장 Chat Modes**

| Mode      | 설명                                 | 워크플로우 역할 |
| --------- | ------------------------------------ | --------------- |
| **Ask**   | 일반적인 질문과 답변에 최적화        | 일반 대화       |
| **Edit**  | 에디터에서 직접 코드를 수정          | 빠른 수정       |
| **Agent** | 복잡한 다단계 작업을 자율적으로 실행 | 3단계 기본 모드 |

#### **워크플로우에서의 역할**

```
2단계: 계획 생성
plan.chatmode.md
- 읽기 전용 도구
- 계획만 생성
- 코드 수정 금지
- 자동 파일 저장

       ↓ (계획 완료)

3단계: 구현 생성
tdd.chatmode.md (또는 implement.chatmode.md)
- 읽기/쓰기 도구
- TDD 방식 구현
- 테스트 우선 작성
- 계획 파일 참조
```

#### **Custom Chat Modes는 언제 사용하나?**

- ✅ **계획 모드**: AI가 읽기 전용 도구만 사용하여 구현 계획만 생성하고 적절한 위치에 저장
- ✅ **연구 모드**: AI가 외부 리소스에 접근하여 새로운 기술 탐색
- ✅ **프론트엔드 개발자 모드**: AI가 프론트엔드 개발 관련 코드만 생성/수정
- ✅ **TDD 구현 모드**: AI가 테스트 주도 개발 방식으로 코드 생성

#### **도구 목록 우선순위**

채팅에서 사용 가능한 도구 목록은 다음 우선순위로 결정됩니다:

```
1. 프롬프트 파일에 지정된 도구 (최우선)
2. 프롬프트 파일에서 참조된 채팅 모드의 도구
3. 선택된 채팅 모드의 기본 도구

예시:
plan-qna.prompt.md (mode: plan)
    ↓
plan.chatmode.md (tools: ['fetch', 'githubRepo', ...])
    ↓
실제 사용 도구: plan.chatmode.md의 도구 목록
```

#### **생성 방법**

```
1️⃣ Chat view → Configure Chat → Modes → Create new custom chat mode file
   또는
   Command Palette → "Chat: Configure Chat Modes"

2️⃣ 위치 선택
   - Workspace: .github/chatmodes/
   - User Profile: 모든 워크스페이스에서 사용 가능

3️⃣ 채팅 모드 이름 입력
   예: plan, tdd, implement, review

4️⃣ .chatmode.md 파일에서 세부사항 제공
   - Front Matter: description, tools, model
   - Body: 채팅 모드 지침
```

---

### 4️⃣ **Language Models** (언어 모델)

다양한 작업에 최적화된 AI 모델 중에서 선택합니다.

#### **워크플로우에서의 역할**

```
1단계: 컨텍스트 큐레이션
→ 모델 선택 불필요 (문서 작성)

2단계: 계획 생성
→ Claude Sonnet 4 (추론에 강함)
   plan.chatmode.md에서 model 지정

3단계: 코드 구현
→ GPT-4o (코드 생성에 빠르고 정확)
   tdd.chatmode.md에서 model 지정
```

#### **언제 어떤 모델을 사용하나?**

| 작업 유형           | 추천 모델       | 이유                             |
| ------------------- | --------------- | -------------------------------- |
| **계획 생성**       | Claude Sonnet 4 | 깊은 추론, 복잡한 아키텍처 이해  |
| **코드 구현**       | GPT-4o          | 빠른 코드 생성, 명시적 지침 준수 |
| **간단한 리팩토링** | GPT-4o mini     | 빠른 속도, 비용 효율적           |
| **코드 리뷰**       | Claude Sonnet 4 | 상세한 분석, 패턴 인식           |
| **복잡한 디버깅**   | Claude Sonnet 4 | 깊은 문제 분석                   |

#### **Chat Mode별 모델 지정 예시**

```markdown
## plan.chatmode.md:

description: 구현 계획 생성
model: Claude Sonnet 4 ← 추론에 강함

---

## tdd.chatmode.md:

description: TDD 방식 구현
model: GPT-4o ← 코드 생성에 빠름

---

## review.chatmode.md:

description: 코드 리뷰
model: Claude Sonnet 4 ← 상세한 분석

---
```

#### **모델 선택 방법**

```
방법 1: Chat view의 모델 선택기 사용
→ 수동으로 모델 전환

방법 2: Chat Mode에서 자동 설정
→ .chatmode.md의 model 속성 사용

방법 3: Prompt File에서 지정
→ .prompt.md의 model 속성 사용
```

---

## 🎯 Best Practices (모범 사례)

### **컨텍스트 관리 원칙**

#### 1. **작게 시작하고 반복**

```
❌ 나쁜 예:
- 처음부터 100페이지 문서 작성
- 모든 가능한 시나리오 커버
- 과도하게 상세한 지침

✅ 좋은 예:
- 2페이지 ARCHITECTURE.md로 시작
- 핵심 패턴만 문서화
- AI 실수 관찰 후 규칙 추가
```

#### 2. **컨텍스트를 신선하게 유지**

```
정기적 감사 주기:

주간:
- AI 생성 코드 품질 확인
- 반복되는 실수 패턴 식별

월간:
- 프로젝트 문서 업데이트
- 아키텍처 변경 반영
- 오래된 규칙 제거
- 계획 아카이브 정리

분기별:
- 전체 컨텍스트 엔지니어링 설정 리뷰
- 팀 피드백 수집
- 워크플로우 개선
- 완료된 계획 통계 분석
```

#### 3. **점진적 컨텍스트 구축 사용**

```
레벨 1: 상위 레벨 (1단계)
.github/copilot-instructions.md
- 아키텍처 개요
- 핵심 디자인 원칙
- 프로젝트 목표

레벨 2: 중간 레벨 (추가 instructions)
backend.instructions.md
frontend.instructions.md
- 기술 스택별 가이드라인
- 프레임워크별 관례

레벨 3: 상세 레벨 (세부 instructions)
api-routes.instructions.md
react-components.instructions.md
- 특정 패턴 상세 설명
- 예제 코드
```

#### 4. **컨텍스트 격리 유지**

```
❌ 나쁜 예:
하나의 채팅에서 계획 + 구현 + 리뷰 + 디버깅
→ 컨텍스트 혼합, AI 혼란

✅ 좋은 예:

채팅 1: 계획 생성 (plan 모드)
- 구현 계획만 생성
- .github/plans/에 저장
- README.md 업데이트 제안

채팅 2: 구현 (tdd 모드)
- 계획 파일 참조
- 코드 생성
- 테스트 작성

채팅 3: 리뷰 (review 모드 또는 새 채팅)
- 계획 vs 구현 비교
- 개선 사항 제안
```

---

### **문서화 전략**

#### 1. **살아있는 문서 만들기**

```
초기 버전:
.github/copilot-instructions.md v1.0
- 기본 아키텍처
- 핵심 원칙

관찰된 문제:
- AI가 잘못된 DB 라이브러리 사용
- 테스트 파일 위치 일관성 없음

개선된 버전:
.github/copilot-instructions.md v1.1
+ PostgreSQL with TypeORM 사용
+ 테스트 파일은 __tests__/ 디렉토리에 위치
```

#### 2. **의사결정 컨텍스트에 집중**

```
❌ 너무 상세:
"useState Hook은 React 16.8에서 도입되었으며,
함수형 컴포넌트에서 상태를 관리할 수 있게 해주고,
클래스 컴포넌트의 this.state와 유사한 기능을 제공하며..."

✅ 의사결정에 집중:
"상태 관리:
- 로컬 상태: useState
- 복잡한 상태: useReducer
- 전역 상태: Zustand
- 서버 상태: React Query"
```

#### 3. **일관된 패턴 사용**

```
명명 관례:
// Components
UserProfile.tsx        (PascalCase)
UserProfileCard.tsx

// Hooks
useUserData.ts         (camelCase, use 접두사)
useAuthentication.ts

// Utils
formatDate.ts          (camelCase)
validateEmail.ts

// Types
user.types.ts          (camelCase, .types 접미사)
api.types.ts

// Plans
user-auth-plan.md      (kebab-case, -plan 접미사)
approval-expense-plan.md
```

#### 4. **외부 지식 참조**

```markdown
# API 통합 가이드

공식 문서 참조:

- [Stripe API](https://stripe.com/docs/api)
- [SendGrid Email](https://docs.sendgrid.com)
- [AWS S3](https://docs.aws.amazon.com/s3/)

내부 문서:

- [인증 플로우](./auth-flow.md)
- [에러 처리](./error-handling.md)

구현 계획:

- [결제 통합 계획](.github/plans/features/payment-integration-plan.md)
```

---

### **워크플로우 최적화**

#### 1. **피드백 루프 구현**

```
매 상호작용마다:

1. 요청 전 명확화
   "사용자 인증 기능 추가"
   ↓
   AI: "OAuth 2.0? JWT? 세션 기반?"

2. 중간 검증
   AI가 계획 생성
   ↓
   검토: "데이터베이스 마이그레이션 누락"
   ↓
   수정: "마이그레이션 단계 추가해줘"

3. 완료 후 리뷰
   AI가 코드 생성
   ↓
   검토: "테스트 커버리지 확인"
   ↓
   보완: "엣지 케이스 테스트 추가"
```

#### 2. **점진적 복잡성 사용**

```
단계별 구현:

Week 1: 기본 구조
- 사용자 모델
- 기본 CRUD
- 계획: basic-user-crud-plan.md

Week 2: 인증 추가
- 회원가입/로그인
- JWT 토큰
- 계획: user-authentication-plan.md

Week 3: 권한 부여
- 역할 기반 접근 제어
- 미들웨어
- 계획: role-based-auth-plan.md

Week 4: 고급 기능
- 비밀번호 재설정
- 이메일 인증
- 계획: advanced-auth-features-plan.md
```

#### 3. **관심사 분리**

```
Chat Mode별 책임:

plan.chatmode.md
✅ 구현 계획 생성
✅ 아키텍처 설계
✅ 작업 분해
✅ 계획 파일 저장
❌ 코드 작성 금지

tdd.chatmode.md
✅ 테스트 작성
✅ 코드 구현
✅ 리팩토링
✅ 계획 파일 참조
❌ 아키텍처 변경 금지

review.chatmode.md
✅ 코드 리뷰
✅ 품질 검증
✅ 개선 제안
✅ 계획 vs 구현 비교
❌ 코드 수정 금지
```

#### 4. **컨텍스트 버전 관리**

```bash
# Git으로 컨텍스트 엔지니어링 추적

git add .github/copilot-instructions.md
git commit -m "feat: Add database connection guidelines"

git add .github/chatmodes/plan.chatmode.md
git commit -m "refactor: Improve planning workflow with auto-save"

git add .github/plans/features/user-auth-plan.md
git commit -m "docs: Add user authentication implementation plan"

# 문제 발생 시 롤백
git revert HEAD
```

---

### **피해야 할 안티패턴**

#### ❌ 1. **컨텍스트 덤핑**

```markdown
나쁜 예 (.github/copilot-instructions.md):

# 프로젝트 가이드라인

우리 프로젝트는 Node.js 18.x를 사용하며 Express.js 4.18.2 프레임워크로 구축되었습니다.
데이터베이스는 PostgreSQL 14.5를 사용하고 TypeORM 0.3.17로 연결됩니다.
테스트는 Jest 29.3.1과 Supertest 6.3.3을 사용합니다.
린팅은 ESLint 8.28.0과 Prettier 2.8.0을 사용합니다.
CI/CD는 GitHub Actions를 사용하며...
[50줄 더...]

좋은 예:

# 프로젝트 가이드라인

## 기술 스택

- Backend: Node.js + Express
- Database: PostgreSQL + TypeORM
- Testing: Jest
- 자세한 버전: [package.json](../package.json)

## 핵심 원칙

1. API는 RESTful 관례 준수
2. 모든 엔드포인트 인증 필요
3. 에러는 중앙 집중식 처리

## 계획 관리

- 모든 구현 계획: [.github/plans/](.github/plans/README.md)
```

#### ❌ 2. **일관성 없는 가이드**

```markdown
나쁜 예:

copilot-instructions.md:
"비동기 작업에 async/await 사용"

backend.instructions.md:
"비동기 작업에 Promise 체인 사용"

→ AI 혼란!

좋은 예:

copilot-instructions.md:
"비동기 작업: [비동기 가이드](./async-guide.md) 참조"

async-guide.md:

# 비동기 작업 가이드

- **우선**: async/await
- **예외**: Promise.all() for 병렬 처리
- **금지**: 콜백 헬
```

#### ❌ 3. **검증 무시**

```
나쁜 예:

사용자: "사용자 인증 구현해줘"
AI: [코드 생성]
사용자: "배포"
→ 보안 취약점 발견!

좋은 예:

사용자: "사용자 인증 구현해줘"
AI: [코드 생성]
사용자: "#.github/plans/features/user-auth-plan.md 대비
        계획한 보안 요구사항 모두 충족했는지 확인해줘"
AI: [검증 및 누락 사항 보고]
사용자: [수정 후 배포]
```

#### ❌ 4. **계획 방치**

```
나쁜 예:
.github/plans/ 폴더에 수십 개의 계획 파일
→ 어떤 것이 진행 중인지 불명확
→ 완료된 계획과 미완료 계획 혼재
→ README.md 업데이트 안 됨

좋은 예:
- README.md에 모든 계획 상태 추적
- 정기적으로 완료된 계획 아카이브
- 스크립트로 자동화된 상태 관리
- 분기별 계획 리뷰
```

---

## 📊 성공 측정

### 정량적 지표

```
측정 항목:

1. 코드 생성 정확도
   - 첫 시도 성공률: 목표 80%+
   - 수정 필요 횟수: 평균 2회 이하

2. 시간 절약
   - 기능 구현 시간: 30-50% 감소
   - 코드 리뷰 시간: 20-40% 감소
   - 문서 작성 시간: 40-60% 감소
   - 계획 수립 시간: 50% 감소

3. 품질 지표
   - 버그 발생률: 감소
   - 테스트 커버리지: 증가
   - 코드 일관성: 증가

4. 계획 관리 지표
   - 계획 완료율: 목표 85%+
   - 평균 계획 구현 시간
   - 계획 대비 실제 구현 정확도
```

### 정성적 지표

```
✅ 성공적인 Context Engineering의 징후:

1. 왕복 감소
   Before: "아니야, 이게 아니라..."
   After: "정확해, 바로 이거야!"

2. 일관된 코드 품질
   Before: 각 기능마다 다른 스타일
   After: 프로젝트 전체 일관성

3. 더 빠른 구현
   Before: 30분 설명 + 1시간 구현
   After: 5분 계획 참조 + 30분 구현

4. 더 나은 아키텍처 결정
   Before: 기존 패턴 무시
   After: 프로젝트 아키텍처 준수

5. 효율적인 계획 관리
   Before: 계획 없이 즉흥 개발
   After: 체계적인 계획 → 구현 → 아카이브
```

### 측정 방법

```
주간 회고:
- 팀원 설문: "AI 지원이 도움되었나?"
- 코드 리뷰: "생성된 코드 품질은?"
- 시간 추적: "실제 시간 절약은?"
- 계획 리뷰: "계획 파일 활용도는?"

월간 분석:
- Pull Request 분석
- 버그 트래커 리뷰
- 개발 속도 추세
- 계획 완료율 통계
- 아카이브 현황

분기별 평가:
- ROI 계산
- 워크플로우 개선 점 식별
- 다음 분기 목표 설정
- 계획 프로세스 개선
```

---

## 🚀 Context Engineering 확장

### **팀의 경우**

#### 공유 및 협업

```
📁 프로젝트 루트/
├── 📁 .github/
│   ├── 📄 copilot-instructions.md     (팀 공통)
│   │
│   ├── 📁 instructions/
│   │   ├── 📄 backend.instructions.md
│   │   ├── 📄 frontend.instructions.md
│   │   └── 📄 mobile.instructions.md
│   │
│   ├── 📁 chatmodes/
│   │   ├── 📄 plan.chatmode.md
│   │   ├── 📄 tdd.chatmode.md
│   │   └── 📄 review.chatmode.md
│   │
│   ├── 📁 prompts/
│   │   ├── 📄 plan-qna.prompt.md
│   │   ├── 📄 security-review.prompt.md
│   │   └── 📄 performance-check.prompt.md
│   │
│   └── 📁 plans/                        (계획 중앙 관리)
│       ├── 📄 README.md                 (진행 상황 추적)
│       ├── 📁 features/
│       ├── 📁 modules/
│       ├── 📁 refactoring/
│       ├── 📁 bug-fixes/
│       └── 📁 archived/
│
├── 📁 scripts/
│   └── 📄 plan-manager.sh               (계획 관리 자동화)
│
└── 📄 README.md
    └── Context Engineering 사용 가이드 포함
```

#### 팀 관례 확립

```markdown
# TEAM_CONVENTIONS.md

## Context Engineering 규칙

### 1. 문서 업데이트

- 아키텍처 변경 시 ARCHITECTURE.md 즉시 업데이트
- 새로운 패턴 도입 시 instructions 파일 추가
- 월 1회 문서 리뷰 미팅

### 2. Chat Mode 사용

- 계획: plan.chatmode 필수
- 구현: tdd.chatmode 권장
- 리뷰: review.chatmode 사용

### 3. 계획 관리

- 새 기능 시작 전 계획 파일 생성 필수
- 계획 파일은 .github/plans/ 폴더에 저장
- README.md 진행 상황 주간 업데이트
- 완료된 계획은 분기별 아카이브

### 4. 피드백 공유

- 좋은 프롬프트: #ai-tips 채널 공유
- 문제 발생: GitHub Issue 생성
- 개선 아이디어: 주간 회의 제안
- 계획 템플릿 개선: Pull Request
```

---

### **대규모 프로젝트의 경우**

#### 컨텍스트 계층 구조

```
레벨 1: 프로젝트 전반
.github/copilot-instructions.md
- 최상위 아키텍처
- 핵심 원칙
- 회사 표준

레벨 2: 모듈별
.github/instructions/
├── auth-module.instructions.md
├── payment-module.instructions.md
└── reporting-module.instructions.md

레벨 3: 기능별
src/features/
├── user-management/
│   └── .instructions.md
├── billing/
│   └── .instructions.md
└── analytics/
    └── .instructions.md

계획 계층:
.github/plans/
├── features/          (공통 기능)
├── modules/           (모듈별 기능)
│   ├── auth/
│   ├── payment/
│   └── reporting/
└── archived/
    └── 2025-Q1/
```

#### 모듈별 Chat Mode

```markdown
.github/chatmodes/
├── plan.chatmode.md (공통)
├── tdd.chatmode.md (공통)
│
├── auth-plan.chatmode.md (인증 모듈 계획)
├── auth-dev.chatmode.md (인증 모듈 구현)
├── payment-plan.chatmode.md (결제 모듈 계획)
├── payment-dev.chatmode.md (결제 모듈 구현)
└── analytics-dev.chatmode.md (분석 모듈 구현)

## auth-plan.chatmode.md:

description: 인증 모듈 구현 계획 생성
tools: ['fetch', 'githubRepo', 'search']
model: Claude Sonnet 4

---

# 인증 모듈 계획 모드

관련 지침:

- [전역 가이드라인](../copilot-instructions.md)
- [인증 모듈 지침](../instructions/auth-module.instructions.md)
- [보안 표준](../instructions/security-standards.instructions.md)

계획 저장 위치:

- `.github/plans/modules/auth/<name>-plan.md`

특별 주의사항:

- 모든 패스워드 bcrypt 해싱
- JWT 토큰 7일 만료
- Refresh 토큰 30일 만료
```

---

### **장기 프로젝트의 경우**

#### 정기 컨텍스트 리뷰 주기

```
월간 리뷰 (첫째 주 월요일):
- [ ] AI 생성 코드 품질 평가
- [ ] 반복되는 실수 패턴 식별
- [ ] instructions 파일 업데이트
- [ ] 새로운 패턴 문서화
- [ ] 진행 중인 계획 상태 확인
- [ ] 지연된 계획 원인 분석

분기별 리뷰 (분기 첫 주):
- [ ] 전체 아키텍처 문서 리뷰
- [ ] 사용하지 않는 규칙 제거
- [ ] 팀 피드백 수집 및 반영
- [ ] Chat Mode 효과성 평가
- [ ] ROI 측정 및 보고
- [ ] 완료된 계획 아카이브
- [ ] 계획 완료율 분석
- [ ] 다음 분기 목표 설정

연간 리뷰 (1월):
- [ ] 전체 Context Engineering 전략 재평가
- [ ] 새로운 AI 도구/기능 도입 검토
- [ ] 차년도 목표 설정
- [ ] 베스트 프랙티스 문서 업데이트
- [ ] 연간 계획 통계 보고서 작성
```

#### 버전 관리 전략

```bash
# 메이저 변경 (아키텍처 재설계)
git tag -a ce-v2.0.0 -m "Microservices 아키텍처로 전환"

# 마이너 변경 (새로운 모듈 추가)
git tag -a ce-v2.1.0 -m "결제 모듈 Context Engineering 추가"

# 패치 변경 (오타 수정, 작은 개선)
git tag -a ce-v2.1.1 -m "코딩 가이드라인 명확화"

# CHANGELOG.md 유지
## [2.1.0] - 2025-01-15
### Added
- 결제 모듈 전용 Chat Mode
- PCI DSS 준수 지침
- 계획 파일 자동 저장 기능
- 계획 관리 스크립트

### Changed
- plan.chatmode에 GitHub Issues 통합
- tdd.chatmode 테스트 커버리지 목표 90%로 상향
- 계획 파일을 .github/plans/ 폴더로 중앙화

### Removed
- 더 이상 사용하지 않는 레거시 API 가이드라인
- 구식 계획 템플릿
```

---

### **여러 프로젝트의 경우**

#### 재사용 가능한 템플릿

```
📁 company-ai-templates/
├── 📁 base-templates/
│   ├── copilot-instructions.template.md
│   ├── plan.chatmode.template.md
│   ├── tdd.chatmode.template.md
│   ├── _plan-template.md
│   └── plans-README.template.md
│
├── 📁 tech-stack-templates/
│   ├── 📁 nodejs-express/
│   │   ├── copilot-instructions.md
│   │   ├── backend.instructions.md
│   │   └── tdd.chatmode.md
│   ├── 📁 react-nextjs/
│   │   ├── copilot-instructions.md
│   │   ├── frontend.instructions.md
│   │   └── component-dev.chatmode.md
│   └── 📁 python-django/
│       ├── copilot-instructions.md
│       └── django-dev.chatmode.md
│
├── 📁 domain-templates/
│   ├── 📁 ecommerce/
│   │   └── 📁 plans/
│   │       ├── features/
│   │       └── modules/
│   ├── 📁 saas/
│   └── 📁 fintech/
│
└── 📁 scripts/
    ├── plan-manager.sh
    └── setup-context-engineering.sh

사용 방법:
1. 프로젝트 유형 선택
2. 해당 템플릿 복사
3. 프로젝트별 커스터마이징
4. .github/ 디렉토리에 배치
5. 스크립트 실행 권한 부여
```

#### 조직 차원의 표준

```markdown
# COMPANY_AI_STANDARDS.md

## 필수 파일 구조

모든 프로젝트는 다음 구조를 따라야 함:

.github/
├── copilot-instructions.md (필수)
├── chatmodes/
│ ├── plan.chatmode.md (필수)
│ └── tdd.chatmode.md (권장)
├── plans/ (필수)
│ ├── README.md
│ ├── \_plan-template.md
│ ├── features/
│ ├── modules/
│ └── archived/
└── instructions/
└── security.instructions.md (필수)

## 공통 원칙

1. 보안 우선 설계
2. 테스트 커버리지 80% 이상
3. 코드 리뷰 필수
4. 문서화 자동화
5. 계획 기반 개발

## 계획 관리 표준

- 모든 새 기능은 계획 파일로 시작
- 계획 파일 명명 규칙 준수
- 주간 진행 상황 업데이트
- 분기별 아카이브

## 도구 통합

- GitHub Issues: 필수
- Slack: 권장
- Jira: 선택
```

---

## 💡 핵심 요약

### Context Engineering의 본질

Context Engineering은 단순히 AI에게 더 많은 정보를 제공하는 것이 아니라, **적절한 정보를 적절한 시간에 적절한 방식으로** 제공하는 것입니다.

### 3단계 워크플로우

```
1️⃣ 프로젝트 컨텍스트 큐레이션
→ 문서화 (PRODUCT.md, ARCHITECTURE.md, CONTRIBUTING.md)
→ Custom Instructions (.github/copilot-instructions.md)
→ 모든 단계에서 자동 적용

2️⃣ 구현 계획 생성
→ 계획 템플릿 (_plan-template.md)
→ 계획 Chat Mode (plan.chatmode.md)
→ 명확화 Prompt (plan-qna.prompt.md)
→ 컨텍스트 기반 계획 생성
→ .github/plans/ 폴더에 체계적 저장
→ README.md로 진행 상황 추적

3️⃣ 코드 구현
→ 구현 Chat Mode (tdd.chatmode.md)
→ 계획 파일 참조 (#.github/plans/...)
→ 코딩 가이드라인 자동 적용
→ 일관성 있는 코드 생성
```

### 4가지 커스터마이제이션 도구

```
Custom Instructions
→ 지속적 가이드라인
→ 자동 적용
→ 프로젝트 전반 일관성

Prompt Files
→ 재사용 가능한 작업
→ 온디맨드 실행
→ 특정 워크플로우
→ 명확화 단계 추가

Custom Chat Modes
→ 전문 페르소나
→ 도구 및 모델 지정
→ 역할별 분리
→ 계획 자동 저장

Language Models
→ 작업별 최적화
→ 속도 vs 품질 균형
→ 비용 효율성
```

### 계획 관리의 핵심

```
체계적 저장
→ .github/plans/ 폴더 구조
→ 카테고리별 분류 (features, modules, refactoring, bug-fixes)
→ 명명 규칙 준수

진행 상황 추적
→ README.md 인덱스
→ 상태별 테이블 (진행중, 계획됨, 완료됨)
→ 메타데이터 표준화

자동화
→ plan-manager.sh 스크립트
→ GitHub Actions 검증
→ 분기별 자동 아카이브

검색 및 재사용
→ 키워드 검색
→ 과거 계획 참조
→ 지식 베이스 구축
```

### 성공의 핵심

1. **작게 시작하고 반복**: 최소한의 컨텍스트로 시작하여 필요에 따라 확장
2. **컨텍스트 신선도 유지**: 프로젝트 진화에 맞춰 문서 정기 업데이트
3. **관심사 분리**: 계획, 구현, 리뷰를 별도 채팅 세션으로 분리
4. **피드백 루프**: 지속적인 검증과 개선
5. **계획 중심 개발**: 모든 기능은 계획 파일에서 시작

### 측정 가능한 성과

- ✅ **왕복 감소**: AI 응답 수정 필요성 감소
- ✅ **일관된 코드 품질**: 확립된 패턴과 관례 준수
- ✅ **더 빠른 구현**: 컨텍스트 설명 시간 감소
- ✅ **더 나은 아키텍처 결정**: 프로젝트 목표와 제약사항 부합
- ✅ **효율적인 계획 관리**: 체계적인 계획 → 구현 → 아카이브 프로세스

---

## 🎉 시작하기

### 빠른 설정 가이드

```bash
# 1. 기본 구조 생성
mkdir -p .github/{chatmodes,prompts,plans/{features,modules,refactoring,bug-fixes,archived},instructions}
mkdir -p scripts

# 2. 필수 파일 생성
touch .github/copilot-instructions.md
touch .github/plans/README.md
touch .github/plans/_plan-template.md
touch .github/chatmodes/plan.chatmode.md
touch .github/chatmodes/tdd.chatmode.md
touch .github/prompts/plan-qna.prompt.md

# 3. 스크립트 설정
touch scripts/plan-manager.sh
chmod +x scripts/plan-manager.sh

# 4. 프로젝트 문서 생성
touch PRODUCT.md ARCHITECTURE.md CONTRIBUTING.md

# 5. Git 추가
git add .github/ scripts/ *.md
git commit -m "feat: Initialize Context Engineering structure"
```

### 다음 단계

1. **문서 작성**: PRODUCT.md, ARCHITECTURE.md, CONTRIBUTING.md 채우기
2. **지침 설정**: .github/copilot-instructions.md 작성
3. **첫 계획**: plan 모드로 첫 구현 계획 생성
4. **구현 시작**: tdd 모드로 계획 기반 코드 생성
5. **팀 공유**: 팀원들에게 워크플로우 교육

---

이러한 관행을 따르고 접근 방식을 지속적으로 개선함으로써, 코드 품질과 프로젝트 일관성을 유지하면서 **AI 지원 개발을 극대화하고 체계적인 계획 관리를 통해 프로젝트를 효율적으로 운영하는 Context Engineering 워크플로우**를 구축할 수 있습니다.
