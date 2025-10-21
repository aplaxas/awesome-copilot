# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is the **Awesome GitHub Copilot** repository - a curated collection of prompts, instructions, chat modes, and collections that enhance the GitHub Copilot experience. The repository provides reusable customizations organized by domain, language, and use case.

## Key Architecture Concepts

### Content Types & File Structure

The repository contains four primary content types, each with specific formats and purposes:

1. **Prompts** (`prompts/*.prompt.md`)
   - Task-specific templates with frontmatter defining `mode`, `tools`, `description`, and optionally `model`
   - Executed via `/prompt-name` or the Command Palette in VS Code
   - Mode can be `agent` (autonomous execution) or `ask` (interactive)

2. **Instructions** (`instructions/*.instructions.md`)
   - File-pattern-based coding standards with frontmatter defining `description` and `applyTo`
   - Automatically apply to files matching the `applyTo` glob pattern
   - Used in `.github/copilot-instructions.md` or `.github/instructions/` directory

3. **Chat Modes** (`chatmodes/*.chatmode.md`)
   - Specialized AI personas with frontmatter defining `description`, optionally `tools` and `model`
   - Define expert roles (architects, DBAs, language specialists)
   - Activated through VS Code Chat interface

4. **Collections** (`collections/*.collection.yml` + `collections/*.md`)
   - YAML manifests grouping related prompts, instructions, and chat modes
   - Schema validated against `.schemas/collection.schema.json`
   - Auto-generated markdown documentation from YAML via `update-readme.js`

### Frontmatter Requirements

All `.prompt.md`, `.instructions.md`, and `.chatmode.md` files require YAML frontmatter:

- `description`: Single-quoted string describing the content
- Prompts: `mode` (required), `tools`, `model` (strongly encouraged)
- Instructions: `applyTo` (required, glob pattern like `'**.js, **.ts'`)
- Chat modes: `tools`, `model` (strongly encouraged)

### File Naming Conventions

- Lowercase with hyphens: `python-django.instructions.md`
- Extensions: `.prompt.md`, `.instructions.md`, `.chatmode.md`, `.collection.yml`
- Collection IDs: `[a-z0-9-]+` pattern (e.g., `azure-cloud-development`)

## Common Development Commands

### Build & Validation

```bash
# Update all README files from source content
node update-readme.js

# Validate collection manifests against schema
node validate-collections.js

# Create a new collection scaffold
node create-collection.js <collection-id>
```

### Testing & Quality

```bash
# Run contributor checks
npm run contributors:check

# Add new contributor
npm run contributors:add <username> <contribution-type>

# Generate contributor list
npm run contributors:generate
```

### Git Workflow

The repository uses automated GitHub Actions for:
- README validation (ensures `update-readme.js` was run)
- Line ending checks (LF enforcement)
- Contributor list maintenance
- Collection schema validation

## Development Workflow

### Adding New Content

1. **Create the file** in appropriate directory with correct extension
2. **Add frontmatter** following the type-specific requirements (see `.github/copilot-instructions.md`)
3. **Run build script**: `node update-readme.js` to update README tables
4. **Validate**: Ensure README changes are included in commit
5. **Submit PR** with clear description

### Collections Workflow

1. **Create YAML manifest**: `collections/<id>.collection.yml`
2. **Reference existing items** only (prompts/instructions/chatmodes must exist first)
3. **Run validation**: `node validate-collections.js`
4. **Run build**: `node update-readme.js` (auto-generates `.md` documentation)
5. **Commit both** `.yml` and auto-generated `.md` files

### Code Review Checklist

The copilot-instructions.md defines review criteria:
- Frontmatter presence and completeness
- Description field wrapped in single quotes
- Lowercase hyphenated filenames
- Correct `applyTo` patterns for instructions
- Correct `mode` values for prompts (`agent` or `ask`)
- Encouraged use of `tools` and `model` fields

## Important Scripts

### update-readme.js

The core build script that:
- Scans all content files for frontmatter and titles
- Generates README tables with install buttons
- Creates collection markdown from YAML manifests
- Must be run before committing changes (CI validates this)

Key functions:
- `extractTitle()`: Parses frontmatter and markdown headings
- `extractDescription()`: Extracts description from frontmatter
- `generateInstallButtons()`: Creates VS Code install links

### validate-collections.js

Validates collection YAML against JSON schema:
- Checks required fields (`id`, `name`, `description`, `items`)
- Validates path patterns and item kinds
- Ensures referenced files exist
- Verifies unique item paths

### yaml-parser.js

Custom YAML parser for collection manifests:
- Handles multi-line strings with proper indentation
- Preserves usage field formatting
- Exported `parseCollectionYaml()` used by other scripts

## Repository Patterns

### Install Button Generation

All content items get VS Code install buttons:
- Pattern: `https://aka.ms/awesome-copilot/install/{type}?url=vscode%3Achat-{type}%2Finstall%3F...`
- Types: `prompt`, `instructions`, `chatmode`
- Supports both VS Code and VS Code Insiders

### README Structure

Each README follows a template pattern:
- Usage instructions section
- Sortable table with Title, Description, Install buttons
- Auto-generated from source files by `update-readme.js`

### Quality Standards

From CONTRIBUTING.md and copilot-instructions.md:
- **Be specific**: Avoid generic guidance
- **Test content**: Verify with GitHub Copilot
- **Promote best practices**: Secure, maintainable, ethical code
- **No harmful content**: Reject content violating Responsible AI principles
- **Complete implementations**: No templates, comments, or stubs in code examples

## Notes for Claude Code

- **Never manually edit README files** - they are auto-generated
- **Always run `update-readme.js`** after adding/modifying content files
- **Validate collections** with `validate-collections.js` before committing
- **Follow frontmatter format** exactly as specified in copilot-instructions.md
- **Use single quotes** for description field values
- **Lowercase hyphenated filenames** for all content files
- **Reference existing files only** when creating collections
- **Commit both .yml and .md** when working with collections
