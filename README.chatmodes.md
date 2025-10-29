# 💭 Custom Chat Modes

Custom chat modes define specific behaviors and tools for GitHub Copilot Chat, enabling enhanced context-aware assistance for particular tasks or workflows.
### How to Use Custom Chat Modes

**To Install:**
- Click the **VS Code** or **VS Code Insiders** install button for the chat mode you want to use
- Download the `*.chatmode.md` file and manually install it in VS Code using the Command Palette

**To Activate/Use:**
- Import the chat mode configuration into your VS Code settings
- Access the installed chat modes through the VS Code Chat interface
- Select the desired chat mode from the available options in VS Code Chat

| ✅ | Title | Description |
| - | ----- | ----------- |
| ✅ | [API Architect mode instructions](chatmodes/api-architect.chatmode.md) | Your role is that of an API architect. Help mentor the engineer by providing guidance, support, and working code. |
| ✅ | [C#/.NET Janitor](chatmodes/csharp-dotnet-janitor.chatmode.md) | Perform janitorial tasks on C#/.NET code including cleanup, modernization, and tech debt remediation. |
| ✅ | [Create PRD Chat Mode](chatmodes/prd.chatmode.md) | Generate a comprehensive Product Requirements Document (PRD) in Markdown, detailing user stories, acceptance criteria, technical considerations, and metrics. Optionally create GitHub issues upon user confirmation. |
| ✅ | [Demonstrate Understanding mode instructions](chatmodes/demonstrate-understanding.chatmode.md) | Validate user understanding of code, design patterns, and implementation details through guided questioning. |
| ✅ | [Expert .NET software engineer mode instructions](chatmodes/expert-dotnet-software-engineer.chatmode.md) | Provide expert .NET software engineering guidance using modern software design patterns. |
| ✅ | [High-Level Big Picture Architect (HLBPA)](chatmodes/hlbpa.chatmode.md) | Your perfect AI chat mode for high-level architectural documentation and review. Perfect for targeted updates after a story or researching that legacy system when nobody remembers what it's supposed to be doing. |
| ✅ | [Implementation Plan Generation Mode](chatmodes/implementation-plan.chatmode.md) | Generate an implementation plan for new features or refactoring existing code. |
| ✅ | [Mentor mode instructions](chatmodes/mentor.chatmode.md) | Help mentor the engineer by providing guidance and support. |
| ✅ | [Microsoft Learn Contributor](chatmodes/microsoft_learn_contributor.chatmode.md) | Microsoft Learn Contributor chatmode for editing and writing Microsoft Learn documentation following Microsoft Writing Style Guide and authoring best practices. |
| ✅ | [Microsoft Study and Learn Chat Mode](chatmodes/microsoft-study-mode.chatmode.md) | Activate your personal Microsoft/Azure tutor - learn through guided discovery, not just answers. |
| ✅ | [MS-SQL Database Administrator](chatmodes/ms-sql-dba.chatmode.md) | Work with Microsoft SQL Server databases using the MS SQL extension. |
| ✅ | [Plan Mode - Strategic Planning & Architecture Assistant](chatmodes/plan.chatmode.md) | Strategic planning and architecture assistant focused on thoughtful analysis before implementation. Helps developers understand codebases, clarify requirements, and develop comprehensive implementation strategies. |
| ✅ | [Planning mode instructions](chatmodes/planner.chatmode.md) | Generate an implementation plan for new features or refactoring existing code. |
| ✅ | [Principal software engineer mode instructions](chatmodes/principal-software-engineer.chatmode.md) | Provide principal-level software engineering guidance with focus on engineering excellence, technical leadership, and pragmatic implementation. |
| ✅ | [Prompt Engineer](chatmodes/prompt-engineer.chatmode.md) | A specialized chat mode for analyzing and improving prompts. Every user input is treated as a propt to be improved. It first provides a detailed analysis of the original prompt within a <reasoning> tag, evaluating it against a systematic framework based on OpenAI's prompt engineering best practices. Following the analysis, it generates a new, improved prompt. |
| ✅ | [Refine Requirement or Issue Chat Mode](chatmodes/refine-issue.chatmode.md) | Refine the requirement or issue with Acceptance Criteria, Technical Considerations, Edge Cases, and NFRs |
| ✅ | [Software Engineer Agent v1](chatmodes/software-engineer-agent-v1.chatmode.md) | Expert-level software engineering agent. Deliver production-ready, maintainable code. Execute systematically and specification-driven. Document comprehensively. Operate autonomously and adaptively. |
| ✅ | [Specification mode instructions](chatmodes/specification.chatmode.md) | Generate or update specification documents for new or existing functionality. |
| ✅ | [Task Researcher Instructions](chatmodes/task-researcher.chatmode.md) | Task research specialist for comprehensive project analysis - Brought to you by microsoft/edge-ai |
| ✅ | [TDD Green Phase - Make Tests Pass Quickly](chatmodes/tdd-green.chatmode.md) | Implement minimal code to satisfy GitHub issue requirements and make failing tests pass without over-engineering. |
| ✅ | [TDD Red Phase - Write Failing Tests First](chatmodes/tdd-red.chatmode.md) | Guide test-first development by writing failing tests that describe desired behaviour from GitHub issue context before implementation exists. |
| ✅ | [TDD Refactor Phase - Improve Quality & Security](chatmodes/tdd-refactor.chatmode.md) | Improve code quality, apply security best practices, and enhance design whilst maintaining green tests and GitHub issue compliance. |
| ✅ | [Technical Debt Remediation Plan](chatmodes/tech-debt-remediation-plan.chatmode.md) | Generate technical debt remediation plans for code, tests, and documentation. |
| ✅ | [Universal Janitor](chatmodes/janitor.chatmode.md) | Perform janitorial tasks on any codebase including cleanup, simplification, and tech debt remediation. |
| ✅ | [Universal PR Comment Addresser](chatmodes/address-comments.chatmode.md) | Address PR comments |
|   | [4.1 Beast Mode (VS Code v1.102)](chatmodes/4.1-Beast.chatmode.md) | GPT 4.1 as a top-notch coding agent. |
|   | [Accessibility mode](chatmodes/accesibility.chatmode.md) | Accessibility mode. |
|   | [Azure AVM Bicep mode](chatmodes/azure-verified-modules-bicep.chatmode.md) | Create, update, or review Azure IaC in Bicep using Azure Verified Modules (AVM). |
|   | [Azure AVM Terraform mode](chatmodes/azure-verified-modules-terraform.chatmode.md) | Create, update, or review Azure IaC in Terraform using Azure Verified Modules (AVM). |
|   | [Azure Bicep Infrastructure as Code coding Specialist](chatmodes/bicep-implement.chatmode.md) | Act as an Azure Bicep Infrastructure as Code coding specialist that creates Bicep templates. |
|   | [Azure Bicep Infrastructure Planning](chatmodes/bicep-plan.chatmode.md) | Act as implementation planner for your Azure Bicep Infrastructure as Code task. |
|   | [Azure Logic Apps Expert Mode](chatmodes/azure-logic-apps-expert.chatmode.md) | Expert guidance for Azure Logic Apps development focusing on workflow design, integration patterns, and JSON-based Workflow Definition Language. |
|   | [Azure Principal Architect mode instructions](chatmodes/azure-principal-architect.chatmode.md) | Provide expert Azure Principal Architect guidance using Azure Well-Architected Framework principles and Microsoft best practices. |
|   | [Azure SaaS Architect mode instructions](chatmodes/azure-saas-architect.chatmode.md) | Provide expert Azure SaaS Architect guidance focusing on multitenant applications using Azure Well-Architected SaaS principles and Microsoft best practices. |
|   | [Azure Terraform Infrastructure as Code Implementation Specialist](chatmodes/terraform-azure-implement.chatmode.md) | Act as an Azure Terraform Infrastructure as Code coding specialist that creates and reviews Terraform for Azure resources. |
|   | [Azure Terraform Infrastructure Planning](chatmodes/terraform-azure-planning.chatmode.md) | Act as implementation planner for your Azure Terraform Infrastructure as Code task. |
|   | [Blueprint Mode Codex v1](chatmodes/blueprint-mode-codex.chatmode.md) | Executes structured workflows with strict correctness and maintainability. Enforces a minimal tool usage policy, never assumes facts, prioritizes reproducible solutions, self-correction, and edge-case handling. |
|   | [Blueprint Mode v39](chatmodes/blueprint-mode.chatmode.md) | Executes structured workflows (Debug, Express, Main, Loop) with strict correctness and maintainability. Enforces an improved tool usage policy, never assumes facts, prioritizes reproducible solutions, self-correction, and edge-case handling. |
|   | [C# MCP Server Expert](chatmodes/csharp-mcp-expert.chatmode.md) | Expert assistant for developing Model Context Protocol (MCP) servers in C# |
|   | [Clojure Interactive Programming with Backseat Driver](chatmodes/clojure-interactive-programming.chatmode.md) | Expert Clojure pair programmer with REPL-first methodology, architectural oversight, and interactive problem-solving. Enforces quality standards, prevents workarounds, and develops solutions incrementally through live REPL evaluation before file modifications. |
|   | [Critical thinking mode instructions](chatmodes/critical-thinking.chatmode.md) | Challenge assumptions and encourage critical thinking to ensure the best possible solution and outcomes. |
|   | [Debug Mode Instructions](chatmodes/debug.chatmode.md) | Debug your application to find and fix a bug |
|   | [Declarative Agents Architect](chatmodes/declarative-agents-architect.chatmode.md) | | |
|   | [Electron Code Review Mode Instructions](chatmodes/electron-angular-native.chatmode.md) | Code Review Mode tailored for Electron app with Node.js backend (main), Angular frontend (render), and native integration layer (e.g., AppleScript, shell, or native tooling). Services in other repos are not reviewed here. |
|   | [Expert C++ software engineer mode instructions](chatmodes/expert-cpp-software-engineer.chatmode.md) | Provide expert C++ software engineering guidance using modern C++ and industry best practices. |
|   | [Expert React Frontend Engineer Mode Instructions](chatmodes/expert-react-frontend-engineer.chatmode.md) | Provide expert React frontend engineering guidance using modern TypeScript and design patterns. |
|   | [Gilfoyle Code Review Mode](chatmodes/gilfoyle.chatmode.md) | Code review and analysis with the sardonic wit and technical elitism of Bertram Gilfoyle from Silicon Valley. Prepare for brutal honesty about your code. |
|   | [Go MCP Server Development Expert](chatmodes/go-mcp-expert.chatmode.md) | Expert assistant for building Model Context Protocol (MCP) servers in Go using the official SDK. |
|   | [GPT 5 Beast Mode](chatmodes/gpt-5-beast-mode.chatmode.md) | Beast Mode 2.0: A powerful autonomous agent tuned specifically for GPT-5 that can solve complex problems by using tools, conducting research, and iterating until the problem is fully resolved. |
|   | [Idea Generator mode instructions](chatmodes/simple-app-idea-generator.chatmode.md) | Brainstorm and develop new application ideas through fun, interactive questioning until ready for specification creation. |
|   | [Java MCP Expert](chatmodes/java-mcp-expert.chatmode.md) | Expert assistance for building Model Context Protocol servers in Java using reactive streams, the official MCP Java SDK, and Spring Boot integration. |
|   | [Kotlin MCP Server Development Expert](chatmodes/kotlin-mcp-expert.chatmode.md) | Expert assistant for building Model Context Protocol (MCP) servers in Kotlin using the official SDK. |
|   | [Kusto Assistant: Azure Data Explorer (Kusto) Engineering Assistant](chatmodes/kusto-assistant.chatmode.md) | Expert KQL assistant for live Azure Data Explorer analysis via Azure MCP server |
|   | [Meta Agentic Project Scaffold](chatmodes/meta-agentic-project-scaffold.chatmode.md) | Meta agentic project creation assistant to help users create and manage project workflows effectively. |
|   | [PHP MCP Expert](chatmodes/php-mcp-expert.chatmode.md) | Expert assistant for PHP MCP server development using the official PHP SDK with attribute-based discovery |
|   | [Playwright Tester](chatmodes/playwright-tester.chatmode.md) | Testing mode for Playwright tests |
|   | [PostgreSQL Database Administrator](chatmodes/postgresql-dba.chatmode.md) | Work with PostgreSQL databases using the PostgreSQL extension. |
|   | [Power BI Data Modeling Expert Mode](chatmodes/power-bi-data-modeling-expert.chatmode.md) | Expert Power BI data modeling guidance using star schema principles, relationship design, and Microsoft best practices for optimal model performance and usability. |
|   | [Power BI DAX Expert Mode](chatmodes/power-bi-dax-expert.chatmode.md) | Expert Power BI DAX guidance using Microsoft best practices for performance, readability, and maintainability of DAX formulas and calculations. |
|   | [Power BI Performance Expert Mode](chatmodes/power-bi-performance-expert.chatmode.md) | Expert Power BI performance optimization guidance for troubleshooting, monitoring, and improving the performance of Power BI models, reports, and queries. |
|   | [Power BI Visualization Expert Mode](chatmodes/power-bi-visualization-expert.chatmode.md) | Expert Power BI report design and visualization guidance using Microsoft best practices for creating effective, performant, and user-friendly reports and dashboards. |
|   | [Power Platform Expert](chatmodes/power-platform-expert.chatmode.md) | Power Platform expert providing guidance on Code Apps, canvas apps, Dataverse, connectors, and Power Platform best practices |
|   | [Power Platform MCP Integration Expert](chatmodes/power-platform-mcp-integration-expert.chatmode.md) | Expert in Power Platform custom connector development with MCP integration for Copilot Studio - comprehensive knowledge of schemas, protocols, and integration patterns |
|   | [Prompt Builder Instructions](chatmodes/prompt-builder.chatmode.md) | Expert prompt engineering and validation system for creating high-quality prompts - Brought to you by microsoft/edge-ai |
|   | [Python MCP Server Expert](chatmodes/python-mcp-expert.chatmode.md) | Expert assistant for developing Model Context Protocol (MCP) servers in Python |
|   | [Requirements to Jira Epic & User Story Creator](chatmodes/atlassian-requirements-to-jira.chatmode.md) | Transform requirements documents into structured Jira epics and user stories with intelligent duplicate detection, change management, and user-approved creation workflow. |
|   | [Ruby MCP Expert](chatmodes/ruby-mcp-expert.chatmode.md) | Expert assistance for building Model Context Protocol servers in Ruby using the official MCP Ruby SDK gem with Rails integration. |
|   | [Rust Beast Mode](chatmodes/rust-gpt-4.1-beast-mode.chatmode.md) | Rust GPT-4.1 Coding Beast Mode for VS Code |
|   | [Rust MCP Expert](chatmodes/rust-mcp-expert.chatmode.md) | Expert assistant for Rust MCP server development using the rmcp SDK with tokio async runtime |
|   | [Semantic Kernel .NET mode instructions](chatmodes/semantic-kernel-dotnet.chatmode.md) | Create, update, refactor, explain or work with code using the .NET version of Semantic Kernel. |
|   | [Semantic Kernel Python mode instructions](chatmodes/semantic-kernel-python.chatmode.md) | Create, update, refactor, explain or work with code using the Python version of Semantic Kernel. |
|   | [Swift MCP Expert](chatmodes/swift-mcp-expert.chatmode.md) | Expert assistance for building Model Context Protocol servers in Swift using modern concurrency features and the official MCP Swift SDK. |
|   | [Task Planner Instructions](chatmodes/task-planner.chatmode.md) | Task planner for creating actionable implementation plans - Brought to you by microsoft/edge-ai |
|   | [Technical spike research mode](chatmodes/research-technical-spike.chatmode.md) | Systematically research and validate technical spike documents through exhaustive investigation and controlled experimentation. |
|   | [Thinking Beast Mode](chatmodes/Thinking-Beast-Mode.chatmode.md) | A transcendent coding agent with quantum cognitive architecture, adversarial intelligence, and unrestricted creative freedom. |
|   | [TypeScript MCP Server Expert](chatmodes/typescript-mcp-expert.chatmode.md) | Expert assistant for developing Model Context Protocol (MCP) servers in TypeScript |
|   | [Ultimate Transparent Thinking Beast Mode](chatmodes/Ultimate-Transparent-Thinking-Beast-Mode.chatmode.md) | Ultimate Transparent Thinking Beast Mode |
|   | [voidBeast_GPT41Enhanced 1.0 - Elite Developer AI Assistant](chatmodes/voidbeast-gpt41enhanced.chatmode.md) | 4.1 voidBeast_GPT41Enhanced 1.0 : a advanced autonomous developer agent, designed for elite full-stack development with enhanced multi-mode capabilities. This latest evolution features sophisticated mode detection, comprehensive research capabilities, and never-ending problem resolution. Plan/Act/Deep Research/Analyzer/Checkpoints(Memory)/Prompt Generator Modes. |
|   | [VSCode Tour Expert](chatmodes/code-tour.chatmode.md) | Expert agent for creating and maintaining VSCode CodeTour files with comprehensive schema support and best practices |
|   | [Wg Code Alchemist](chatmodes/wg-code-alchemist.chatmode.md) | Ask WG Code Alchemist to transform your code with Clean Code principles and SOLID design |
|   | [Wg Code Sentinel](chatmodes/wg-code-sentinel.chatmode.md) | Ask WG Code Sentinel to review your code for security issues. |
