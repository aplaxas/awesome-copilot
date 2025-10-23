---
mode: "agent"
description: "Interactive planning with clarification questions before generating detailed implementation plans"
model: "Claude Sonnet 4.5"
tools: ["fetch", "githubRepo", "problems", "usages", "search", "todos", "get_issue", "get_issue_comments", "list_issues"]
---

# Plan with Q&A

Analyze my feature request briefly, then ask **3 clarifying questions** to understand requirements better. Only after receiving answers, proceed with the planning workflow.

## Clarification Process

Before creating the implementation plan, ask questions to clarify:

1. **Scope & Requirements**: What are the core requirements vs. nice-to-haves?
2. **Technical Constraints**: Any specific technologies, patterns, or architectural constraints?
3. **Success Criteria**: How will we measure successful completion?

## Planning Workflow

After clarification, create a detailed implementation plan following the project's plan template:

1. **Analyze Context**: Review codebase, existing patterns, and architecture
2. **Determine Category**: feature | module | refactoring | bug-fix
3. **Structure Plan**: Use the implementation plan template from `.github/plans/_plan-template.md`
4. **Save to Appropriate Location**:
   - Features: `.github/plans/features/<name>-plan.md`
   - Modules: `.github/plans/modules/<module>-<name>-plan.md`
   - Refactoring: `.github/plans/refactoring/<name>-plan.md`
   - Bug fixes: `.github/plans/bug-fixes/<name>-fix-plan.md`
5. **Suggest Index Update**: Recommend updating `.github/plans/README.md`

## Plan File Requirements

All plans must include:

- **Metadata**: title, category, priority, status, dates, version
- **Overview**: Business value and success criteria
- **Architecture**: System design, components, data models, API design
- **Task Breakdown**: Phased approach with time estimates
- **Testing Strategy**: Unit, integration, E2E with coverage goals
- **Security Considerations**: Auth, validation, vulnerabilities
- **Performance Considerations**: Load expectations and optimization
- **Deployment Plan**: Migrations, rollback, feature flags
- **Unresolved Questions**: Key decisions needed

## Naming Conventions

- Use lowercase with hyphens: `user-authentication-plan.md`
- Always end with `-plan.md`
- Be descriptive and concise

## Example Usage

```prompt
User: "Add customer detail page that displays and edits customer info"

```
