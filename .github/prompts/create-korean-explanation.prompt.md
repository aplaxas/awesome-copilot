---
mode: "agent"
description: "Creates Korean ‘explanation’ versions of .chatmode.md, .prompt.md, and .instructions.md files (not translations). YAML headers must be copied exactly as-is. The new files are named .kr.chatmode.md, .kr.prompt.md, and .kr.instructions.md respectively, and are generated in the same folder. The explanation should keep technical and programming terms in English, preserve the structure and meaning of the original, but describe the content naturally in Korean."
tools: ["edit/editFiles", "edit/createFile", "fetch", "todos"]
model: GPT-4o
---

# YOUR TASK

Explain `${file}` in Korean (not a translation) and create a `.kr.md` version in the same folder.  
The new file must mirror the structure of the original (headings, lists, tables, code blocks, and order).

## Explanation Rules

1. **YAML Header Copy:**

   - The YAML header block (the part between `---` lines at the top) must be **copied exactly** without modification or explanation.
   - The explanation begins **immediately after** the header.

2. **Explanation, not translation:**

   - Write natural Korean explanations that describe each section’s purpose, context, usage, and important notes.
   - Do **not** translate sentences directly; use short English quotes (≤10 words) only when necessary.

3. **Mirror structure:**

   - Reproduce the same Markdown hierarchy (headings, lists, tables, notes, etc.) in the same order.
   - For each original element, provide an explanatory paragraph in Korean right below it.
   - Keep code blocks, examples, links, file paths, API endpoints, and configuration keys in **English**.
   - Add subheadings like “Concept Summary,” “Intent & Context,” “Cautions,” or “Recommended Pattern” if needed for clarity.

4. **KEEP (do not translate):**  
   Keep the following in English:

   - Technical and programming terms
   - Class/method/property/interface names
   - File, folder, registry, and environment variable names
   - CLI commands, configuration keys, error messages/codes
   - HTTP methods/status codes, SQL statements, and code snippets

5. **File Naming Convention:**

   - `_*.chatmode.md` → `_*.kr.chatmode.md`
   - `_*.instructions.md` → `_*.kr.instructions.md`
   - `_*.prompt.md` → `_*.kr.prompt.md`

6. **Folder Rule:**

   - The `.kr.md` file must be created in the **same directory** as the original.

7. **Formatting Fidelity:**

   - Preserve Markdown formatting (tables, lists, inline code, footnotes, quotes).
   - Do not alter URLs or anchors; you may add Korean explanation after the link text if needed.

8. **Scope & Brevity:**

   - Avoid overly long or unrelated background explanations.
   - Focus on the original’s intent, process, constraints, and exceptions.

9. **Idempotency:**
   - If a `.kr.*` version already exists in the target path, **overwrite** it.
   - Optionally, append a short “revision history” section at the bottom.
