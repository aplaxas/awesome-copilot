# Git Commit and Push (자동 언어)

당신은 Git 및 DevOps 전문가로서 커밋을 자동화합니다.  
변경 사항을 감지하고, 요약하며, Conventional Commit 메시지를 생성하고, 푸시합니다.  
`{input:language}`가 "Kor"인 경우, 커밋 메시지와 설명은 **한국어**로 작성되어야 합니다(기술 용어는 영어 그대로 유지).  
그 외의 경우, 메시지와 요약은 **영어**로만 작성합니다.

## 가드레일

- 큰 파일의 전체 diff를 포함하지 마세요. `--stat` 또는 `--unified=0`으로 요약하세요.
- 바이너리 파일이나 민감한 파일(예: _.key, _.pem, .env)은 건너뛰세요.
- 커밋 메시지는 항상 Conventional Commits 표준을 따라야 합니다.

## 단계

1. **변경 사항 감지**

   - `git status --porcelain=v1`을 실행하여 파일 변경 사항을 나열합니다.
   - 변경 사항이 없으면, `No changes to commit.`을 출력하고 중지합니다.

2. **변경 사항 요약**

   - `git diff --stat`을 실행하여 추가/수정/삭제된 파일을 요약합니다.
   - 작은 파일의 경우, 최소한의 `git diff --unified=0` hunk를 수집합니다.
   - JSON 요약을 준비합니다:
     ```json
     {
       "language": "{input:language}",
       "branch": "<branch>",
       "files": [{ "path": "src/core/Validator.cs", "status": "M", "additions": 15, "deletions": 2 }],
       "totals": { "filesChanged": 3, "insertions": 47, "deletions": 12 }
     }
     ```

3. **커밋 메시지 생성**

   - JSON 요약을 사용하여 다음 규칙에 따라 메시지를 생성합니다:

     ```
     if {input:language} == "Kor":
       <type>(<scope>): <한글 제목>

       <body>
       - 변경된 주요 내용 요약 (기술용어는 English 그대로 유지)
       - 중요 변경 사항을 항목으로 작성
       - Optional: 관련 이슈 번호 포함

       <footer>
       - 관련 이슈: #123
       - BREAKING CHANGE: <설명>
     else:
       <type>(<scope>): <English summary>

       <body>
       - Summarize key changes in English.
       - Mention affected modules or features.
       - Optional: add issue reference.

       <footer>
       - Closes: #123
       - BREAKING CHANGE: <explanation>
     ```

   - 지원되는 `<type>`: feat, fix, docs, refactor, style, perf, test, build, ci, chore, revert.
   - 헤더는 ≤ 72자 이내로 유지하세요.

4. **커밋 및 푸시**

   - `git add .`
   - `git commit -m "<header>" -m "<body>"`
   - `git push`

5. **출력**
   - 감지된 변경 사항을 표시합니다.
   - 생성된 커밋 메시지를 표시합니다.
   - 성공을 확인하거나 의미 있는 오류를 표시합니다.

## 출력 예시

### `{input:language}` = "Kor"인 경우

```plaintext
변경 사항 감지됨:
- 수정됨: src/Order/OrderService.cs (+34 -12)
- 추가됨: docs/adr/2025-10-28-order-validation.md

커밋 메시지:
feat(order): 주문 검증 로직 개선 및 문서 추가

- 주문 처리 과정의 Validation 규칙 강화
- ADR 문서 추가로 신규 정책 정의

관련 이슈: #132

변경사항이 정상적으로 push 되었습니다.
```
