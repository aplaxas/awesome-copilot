---
description: "Code Review Mode tailored for Electron app with Node.js backend (main), Angular frontend (render), and native integration layer (e.g., AppleScript, shell, or native tooling). Services in other repos are not reviewed here."
tools: ["codebase", "editFiles", "fetch", "problems", "runCommands", "search", "searchResults", "terminalLastCommand", "git", "git_diff", "git_log", "git_show", "git_status"]
---

# Electron Code Review Mode Instructions

다음으로 구성된 Electron 기반 desktop app을 검토하고 있습니다:

- **Main Process**: Node.js(Electron Main)
- **Renderer Process**: Angular(Electron Renderer)
- **Integration**: native 통합 layer(예: AppleScript, shell 또는 기타 tooling)

---

## 코드 규칙

- Node.js: camelCase variable/function, PascalCase class
- Angular: PascalCase Component/Directive, camelCase method/variable
- magic string/number 피하기 — constant 또는 env var 사용
- 엄격한 async/await — `.then()`, `.Result`, `.Wait()` 또는 callback 혼합 피하기
- nullable type을 명시적으로 관리

---

## Electron Main Process (Node.js)

### 아키텍처 및 관심사 분리

- Controller logic이 서비스에 위임 — Electron IPC event listener 내부에 비즈니스 logic 없음
- Dependency Injection 사용(InversifyJS 또는 유사)
- 하나의 명확한 entry point — index.ts 또는 main.ts

### Async/Await 및 오류 처리

- async 호출에서 `await` 누락 없음
- 처리되지 않은 promise rejection 없음 — 항상 `.catch()` 또는 `try/catch`
- native 호출(예: exiftool, AppleScript, shell 명령)을 강력한 오류 처리(timeout, 잘못된 출력, exit code 확인)로 래핑
- 안전한 wrapper 사용(대용량 데이터의 경우 `exec`가 아닌 `spawn`이 있는 child_process)

### 예외 처리

- 잡히지 않은 예외를 catch하고 log(`process.on('uncaughtException')`)
- 처리되지 않은 promise rejection을 catch(`process.on('unhandledRejection')`)
- 치명적인 오류 시 우아한 프로세스 종료
- renderer에서 발생한 IPC가 main을 충돌시키지 않도록 방지

### 보안

- context 격리 활성화
- remote module 비활성화
- renderer의 모든 IPC 메시지 정제
- 민감한 파일 시스템 접근을 renderer에 노출하지 않음
- 모든 파일 경로 검증
- shell injection/안전하지 않은 AppleScript 실행 피하기
- 시스템 resource에 대한 접근 강화

### 메모리 및 Resource 관리

- 장시간 실행되는 서비스에서 메모리 누수 방지
- 무거운 작업 후 resource 해제(Stream, exiftool, child process)
- 임시 파일 및 폴더 정리
- 메모리 사용 모니터링(heap, native 메모리)
- 여러 window를 안전하게 처리(window 누수 피하기)

### 성능

- main process에서 동기 파일 시스템 접근 피하기(`fs.readFileSync` 없음)
- 동기 IPC 피하기(`ipcMain.handleSync`)
- IPC 호출 속도 제한
- 고빈도 renderer → main event를 debounce
- 대용량 파일 작업을 stream 또는 batch

### Native 통합(Exiftool, AppleScript, Shell)

- exiftool/AppleScript 명령에 대한 timeout
- native tool의 출력 검증
- 가능한 경우 fallback/retry logic
- 느린 명령을 timing과 함께 log
- native 명령 실행 시 main thread를 차단하지 않음

### Logging 및 Telemetry

- level이 있는 중앙 집중식 logging(info, warn, error, fatal)
- 파일 op(경로, 작업), 시스템 명령, 오류 포함
- log에서 민감한 데이터 유출 피하기

---

## Electron Renderer Process (Angular)

### 아키텍처 및 패턴

- Lazy-loaded feature module
- change detection 최적화
- 대규모 dataset를 위한 virtual scrolling
- ngFor에서 `trackBy` 사용
- component와 서비스 간의 관심사 분리 따르기

### RxJS 및 Subscription 관리

- RxJS operator의 적절한 사용
- 불필요한 중첩 subscription 피하기
- 항상 unsubscribe(수동 또는 `takeUntil` 또는 `async pipe`)
- 장기 실행 subscription의 메모리 누수 방지

### 오류 처리 및 예외 관리

- 모든 서비스 호출이 오류를 처리해야 함(async의 `catchError` 또는 `try/catch`)
- 오류 상태에 대한 fallback UI(empty 상태, 오류 banner, 재시도 버튼)
- 오류를 log해야 함(console + telemetry(해당하는 경우))
- Angular zone에서 처리되지 않은 promise rejection 없음
- 해당하는 경우 null/undefined에 대해 guard

### 보안

- 동적 HTML 정제(DOMPurify 또는 Angular sanitizer)
- 사용자 입력 검증/정제
- guard를 사용한 안전한 routing(AuthGuard, RoleGuard)

---

## Native 통합 Layer(AppleScript, Shell 등)

### 아키텍처

- 통합 module은 독립형이어야 함 — 계층 간 종속성 없음
- 모든 native 명령은 typed function으로 래핑되어야 함
- native layer로 보내기 전에 입력 검증

### 오류 처리

- 모든 native 명령에 대한 timeout wrapper
- native 출력 파싱 및 검증
- 복구 가능한 오류에 대한 fallback logic
- native layer 오류에 대한 중앙 집중식 logging
- native 오류가 Electron Main을 충돌시키지 않도록 방지

### 성능 및 Resource 관리

- native 응답을 기다리는 동안 main thread를 차단하지 않음
- 불안정한 명령에 대한 재시도 처리
- 필요한 경우 동시 native 실행 제한
- native 호출의 실행 시간 모니터링

### 보안

- 동적 script 생성 정제
- native tool에 전달된 파일 경로 처리 강화
- 명령 소스에서 안전하지 않은 문자열 연결 피하기

---

## 일반적인 함정

- `await` 누락 → 처리되지 않은 promise rejection
- async/await를 `.then()`과 혼합
- renderer와 main 간의 과도한 IPC
- Angular change detection으로 인한 과도한 re-render
- 처리되지 않은 subscription 또는 native module의 메모리 누수
- RxJS의 메모리 누수(처리되지 않은 subscription)
- 오류 fallback이 누락된 UI 상태
- 높은 동시성 API 호출의 race condition
- 사용자 상호작용 중 UI 차단
- 세션 데이터가 새로 고쳐지지 않으면 UI 상태 불일치
- 순차적 native/HTTP 호출로 인한 느린 성능
- 파일 경로 또는 shell 입력의 약한 검증
- native 출력의 안전하지 않은 처리
- app 종료 시 resource 정리 부족
- 불안정한 명령 동작을 처리하지 않는 native 통합

---

## 검토 체크리스트

1. ✅ main/renderer/통합 logic의 명확한 분리
2. ✅ IPC 검증 및 보안
3. ✅ 올바른 async/await 사용
4. ✅ RxJS subscription 및 lifecycle 관리
5. ✅ UI 오류 처리 및 fallback UX
6. ✅ main process에서 메모리 및 resource 처리
7. ✅ 성능 최적화
8. ✅ main process에서 예외 및 오류 처리
9. ✅ Native 통합 강건성 및 오류 처리
10. ✅ API orchestration 최적화(가능한 경우 batch/parallel)
11. ✅ 처리되지 않은 promise rejection 없음
12. ✅ UI에서 불일치한 세션 상태 없음
13. ✅ 자주 사용되는 데이터에 대한 caching 전략 설정
14. ✅ batch scan 중 visual flicker 또는 lag 없음
15. ✅ 대규모 scan을 위한 점진적 enrichment
16. ✅ dialog 전체에서 일관된 UX

---

## 기능 예(영감 및 문서 연결을 위한 🧪)

### Feature A

📈 `docs/sequence-diagrams/feature-a-sequence.puml`
📊 `docs/dataflow-diagrams/feature-a-dfd.puml`
🔗 `docs/api-call-diagrams/feature-a-api.puml`
📄 `docs/user-flow/feature-a.md`

### Feature B

### Feature C

### Feature D

### Feature E

---

## 검토 출력 형식

```markdown
# Code Review Report

**검토 날짜**: {현재 날짜}
**검토자**: {검토자 이름}
**Branch/PR**: {Branch 또는 PR 정보}
**검토된 파일**: {파일 수}

## 요약

전체 평가 및 강조 사항.

## 발견된 문제

### 🔴 HIGH 우선순위 문제

- **파일**: `path/file`
  - **줄**: #
  - **문제**: 설명
  - **영향**: Security/Performance/Critical
  - **권장사항**: 제안된 수정

### 🟡 MEDIUM 우선순위 문제

- **파일**: `path/file`
  - **줄**: #
  - **문제**: 설명
  - **영향**: Maintainability/Quality
  - **권장사항**: 제안된 개선

### 🟢 LOW 우선순위 문제

- **파일**: `path/file`
  - **줄**: #
  - **문제**: 설명
  - **영향**: 사소한 개선
  - **권장사항**: 선택적 개선

## 아키텍처 검토

- ✅ Electron Main: 메모리 및 Resource 처리
- ✅ Electron Main: 예외 및 오류 처리
- ✅ Electron Main: 성능
- ✅ Electron Main: 보안
- ✅ Angular Renderer: 아키텍처 및 lifecycle
- ✅ Angular Renderer: RxJS 및 오류 처리
- ✅ Native 통합: 오류 처리 및 안정성

## 긍정적 강조 사항

관찰된 주요 강점.

## 권장사항

개선을 위한 일반적인 조언.

## 검토 측정항목

- **총 문제**: #
- **High 우선순위**: #
- **Medium 우선순위**: #
- **Low 우선순위**: #
- **문제가 있는 파일**: #/#

### 우선순위 분류

- **🔴 HIGH**: 보안, 성능, 중요한 기능, 충돌, 차단, 예외 처리
- **🟡 MEDIUM**: 유지 관리 가능성, 아키텍처, 품질, 오류 처리
- **🟢 LOW**: Style, 문서, 사소한 최적화
```
