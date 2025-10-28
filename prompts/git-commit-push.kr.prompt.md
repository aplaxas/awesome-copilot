# YOUR TASK

`git commit` 및 `git push` 명령어를 사용하여 변경 사항을 커밋하고 원격 저장소에 푸시하십시오.

## 지침

1. **커밋 메시지 작성:**

   - 커밋 메시지는 간결하고 명확해야 합니다.
   - 첫 번째 줄은 50자 이내로 유지하고, 필요하면 추가 설명을 빈 줄 뒤에 작성하십시오.

2. **단계별 명령어:**

   - `git add .` 또는 특정 파일을 추가합니다.
   - `git commit -m "메시지"`를 사용하여 커밋합니다.
   - `git push origin <branch>`를 사용하여 변경 사항을 푸시합니다.

3. **에러 처리:**

   - 충돌이 발생하면 `git pull`을 사용하여 최신 변경 사항을 병합한 후 다시 시도하십시오.
   - 인증 문제가 발생하면 자격 증명을 확인하십시오.

4. **모범 사례:**
   - 관련 없는 변경 사항을 포함하지 마십시오.
   - 커밋 메시지에 작업 ID 또는 관련 이슈 번호를 포함하십시오.

## 예제

```bash
# 모든 변경 사항 추가
git add .

# 커밋 메시지 작성
git commit -m "Fix: resolve login issue (#123)"

# 변경 사항 푸시
git push origin main
```

## 참고 리소스

- [Git 공식 문서](https://git-scm.com/doc)
- [Git 커밋 메시지 작성 가이드](https://chris.beams.io/posts/git-commit/)
