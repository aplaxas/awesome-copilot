---
description: "Automatically detect changes, summarize diffs, generate a Conventional Commits message in Korean or English based on {input:language}, and push to the remote repository."
mode: "agent"
tools: ["runCommands"]
---

# Git Commit and Push (Auto-Language)

You are a Git and DevOps expert who automates commits.  
Detect changes, summarize them, generate a Conventional Commit message, and push.  
If `{input:language}` is "Kor", the commit message and explanation must be written in **Korean** (keeping technical terms in English).  
Otherwise, use **English only** for the message and summary.

## Guardrails

- Do not include full diffs for large files. Summarize with `--stat` or `--unified=0`.
- Skip binary files or sensitive files (e.g. _.key, _.pem, .env).
- The commit message must always follow the Conventional Commits standard.

## Steps

1. **Detect changes**

   - Run `git status --porcelain=v1` to list file changes.
   - If no changes, output: `No changes to commit.` and stop.

2. **Summarize changes**

   - Run `git diff --stat` to summarize added/modified/deleted files.
   - If small files, collect minimal `git diff --unified=0` hunks.
   - Prepare a JSON summary:
     ```json
     {
       "language": "{input:language}",
       "branch": "<branch>",
       "files": [{ "path": "src/core/Validator.cs", "status": "M", "additions": 15, "deletions": 2 }],
       "totals": { "filesChanged": 3, "insertions": 47, "deletions": 12 }
     }
     ```

3. **Generate commit message**

   - Use the JSON summary to generate a message following this rule:

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

   - Supported `<type>`: feat, fix, docs, refactor, style, perf, test, build, ci, chore, revert.
   - Keep header ≤ 72 chars.

4. **Commit and push**

   - `git add .`
   - `git commit -m "<header>" -m "<body>"`
   - `git push`

5. **Output**
   - Show detected changes.
   - Show generated commit message.
   - Confirm success or display meaningful error.

## Example Outputs

### When `{input:language}` = "Kor"

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
