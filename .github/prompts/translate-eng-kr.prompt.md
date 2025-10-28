---
mode: "agent"
description: "영어로 작성된 .chatmode.md, .prompt.md, .instructions.md 파일을 한국어로 번역하여 동일 폴더 내에 .kr.chatmode.md, .kr.prompt.md, .kr.instructions.md 파일로 생성한다. 기술 및 프로그래밍 용어는 영어로 유지하며, 원본의 의미와 서식을 유지해야 한다."
tools: ["edit/editFiles", "edit/createFile", "fetch", "todos"]
model: GPT-4o
---

# YOUR TASK

Translate `${file}` from English to Korean and create a `.kr.md` version in the same folder.

## Translation Rules

1. **Translation:** Translate all text into natural Korean.
2. **KEEP:** Keep technical and coding terms in English.
3. **File Naming:**
   - `_*.chatmode.md` → `_*.kr.chatmode.md`
   - `_*.instructions.md` → `_*.kr.instructions.md`
   - `_*.prompt.md` → `_*.kr.prompt.md`
4. **Folder:** The new `.kr.md` file must be created in the same directory as the original.
