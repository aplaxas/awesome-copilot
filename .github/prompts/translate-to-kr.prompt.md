---
mode: "agent"
description: "Translates English .chatmode.md, .prompt.md, and .instructions.md files into Korean while preserving the exact original Markdown structure and layout. The YAML header must be copied without any change or translation. The rest of the content is translated line-by-line into Korean, keeping technical and programming terms in English. Each translated `.kr.md` file must map 1:1 to the original file structure."
tools: ["edit/editFiles", "edit/createFile", "fetch", "todos"]
model: Claude Sonnet 4.5
---

# YOUR TASK

Translate selected `${file}` from English to Korean and create a `.kr.md` version in the same folder.

## Core Translation Principles

1. **Exact Structural Mapping (1:1 correspondence):**

   - The translated file must **mirror the structure of the original exactly** — including headings, subheadings, lists, code blocks, tables, notes, and blank lines.
   - Every section, heading, and block must appear in **the same order and hierarchy** as the original.
   - There should be a **1:1 correspondence** between the original and translated lines or paragraphs.
   - Never merge, split, or omit sections or sentences.

2. **Do Not Translate YAML Header:**

   - The YAML front matter (the block between `---` at the top) must be **copied verbatim**.
   - Do **not** add explanations, translations, or comments inside or around it.
   - Translation begins **only after** the closing `---`.

3. **Faithful Translation of Content:**

   - Translate all textual content into **natural, professional Korean**, maintaining the meaning and tone.
   - Keep punctuation, emphasis (bold, italic), and formatting (backticks, code blocks) exactly as in the source.
   - Preserve all whitespace, indentation, and Markdown symbols.

4. **KEEP (Do not translate):**  
   Keep all technical and programming elements in **English**, including:

   - Code snippets, variable names, class/method/property/interface names
   - CLI commands, configuration keys, environment variables
   - File/folder names, URLs, paths
   - HTTP methods, status codes, SQL queries, error codes
   - Inline code wrapped in backticks (`` ` ``)

5. **File Naming Convention:**

   - `_*.chatmode.md` → `_*.kr.chatmode.md`
   - `_*.instructions.md` → `_*.kr.instructions.md`
   - `_*.prompt.md` → `_*.kr.prompt.md`

6. **Folder Rule:**

   - The new `.kr.md` file must be created in the **same directory** as the original file.
   - If a `.kr.*` version already exists in the target path, **overwrite** it.

7. **Quality Requirements:**
   - Translation must be accurate, fluent, and contextually consistent.
   - Do not translate word-for-word — prioritize clarity and readability in Korean.
   - The translated document should allow a Korean reader to understand the same meaning, tone, and intent as the English source.
   - Review for 1:1 structure mapping before finalizing.
