# Own AI Coding Agent — Production Architecture Research & Build Specification

**Document:** Complete Architecture Research File  
**Version:** 1.0  
**Date:** 2026-09-27  
**Target:** Open-source, local-first AI coding/development agent  
**Primary goal:** Build an AI agent from our own application code that can understand projects, edit code, run tools, work with databases, Git, testing, documentation and deployment workflows, while keeping a genuinely free local mode for end users.

---

## 1. Executive Summary

The product should not begin by training a new foundation model. The practical architecture is to build an **agent platform** around interchangeable local/open-weight and optional cloud models.

The product's differentiation should live in:

- Agent orchestration
- Project/codebase intelligence
- Tool execution
- Memory and retrieval
- Sandboxed workspaces
- Database-aware development
- Git and deployment workflows
- Verification and test loops
- Permission/approval controls
- Extensible skills/plugins
- Local-first operation

The core pipeline is:

```text
User
  ↓
Web/Desktop/CLI/Voice UI
  ↓
Agent Gateway
  ↓
Orchestrator
  ↓
Planner → Context/Retrieval → Model → Tool Selection
  ↓
Specialist Agents / Tools
  ↓
Sandboxed Workspace
  ↓
Tests / Verification
  ↓
Diff + Results
  ↓
Human Approval
  ↓
Commit / Deploy
```

For a free mode, the preferred topology is:

```text
User Computer
├── Agent UI
├── Agent Runtime
├── Local Model Runtime
├── Project Workspace
├── Local Code Index
├── Local Vector/Full-text Search
└── Sandboxed Tools
```

Cloud services should be optional rather than mandatory.

---

# 2. Product Vision

Build a general-purpose AI development agent that can:

1. Understand a user's software project.
2. Index and search a large codebase.
3. Explain architecture and dependencies.
4. Plan multi-step coding tasks.
5. Edit files safely.
6. Run terminal commands in an isolated workspace.
7. Generate and run tests.
8. Diagnose build/runtime errors.
9. Work with SQL and application databases.
10. Manage Git branches, commits and diffs.
11. Integrate with Git hosting.
12. Assist with Docker and CI/CD.
13. Read project documentation.
14. Maintain project-specific memory.
15. Use external tools through a standardized protocol.
16. Ask for approval before high-risk actions.
17. Operate locally without mandatory per-request API billing.
18. Support optional cloud AI providers for users who want higher capability.

---

# 3. Key Design Principle

The project is **not an LLM project first**.

It is an **agent-runtime + developer-environment project**.

```text
Foundation Model
      +
Agent Runtime
      +
Context Engineering
      +
Code Intelligence
      +
Tools
      +
Memory
      +
Sandbox
      +
Verification
      +
Security
      =
AI Coding Agent
```

The model can change without rebuilding the whole product.

---

# 4. Research Conclusions

## 4.1 Agent frameworks

Modern agent frameworks generally model an agent as a model plus instructions, tools and runtime behavior. OpenAI's current agent documentation also describes tools, guardrails, handoffs, structured outputs and sandbox execution as core building blocks. This is useful as a reference architecture even if the final product implements its own runtime. 

Reference:
- OpenAI Agents SDK: https://developers.openai.com/api/docs/guides/agents/sdk
- OpenAI Agents concepts: https://openai.github.io/openai-agents-python/agents/

## 4.2 MCP / tool interoperability

The Model Context Protocol uses a host/client/server architecture for exchanging context and exposing tools/resources to AI applications. MCP is especially useful as an interoperability boundary: the agent can consume external capabilities without hard-coding every integration into the core runtime.

Recommended architecture:

```text
Agent Runtime
     │
     ├── Native Tools
     │
     └── MCP Client
            ├── GitHub MCP
            ├── Database MCP
            ├── Documentation MCP
            └── Custom User MCP
```

Reference:
- MCP architecture: https://modelcontextprotocol.io/docs/learn/architecture

## 4.3 Code parsing

Tree-sitter is an incremental parser that generates concrete syntax trees and is designed to update efficiently as source code changes. It has official bindings for multiple languages including JavaScript, Python, Rust, Go, Java, C#, Swift and others.

This makes Tree-sitter a strong component for code indexing, symbol extraction and structural code search.

Reference:
- Tree-sitter: https://tree-sitter.github.io/tree-sitter/

## 4.4 Sandbox execution

A coding agent must treat shell execution and file access as privileged capabilities. Modern agent runtimes increasingly use sandboxed workspaces for repository/file/shell operations. The same principle should be implemented independently in this product.

Reference example:
- OpenAI Agents SDK sandbox concepts: https://openai.github.io/openai-agents-js/

---

# 5. High-Level Architecture

```text
                         ┌─────────────────────┐
                         │       USER          │
                         └──────────┬──────────┘
                                    │
                                    ▼
                    ┌───────────────────────────┐
                    │       CLIENT LAYER        │
                    │ Web │ Desktop │ CLI │ Voice│
                    └─────────────┬─────────────┘
                                  │
                                  ▼
                    ┌───────────────────────────┐
                    │       AGENT GATEWAY       │
                    │ Auth / Sessions / Streams │
                    └─────────────┬─────────────┘
                                  │
                                  ▼
                    ┌───────────────────────────┐
                    │    AGENT ORCHESTRATOR     │
                    │ Plan / Route / Execute    │
                    └─────────────┬─────────────┘
                                  │
          ┌───────────────────────┼────────────────────────┐
          ▼                       ▼                        ▼
 ┌────────────────┐      ┌──────────────────┐      ┌───────────────┐
 │ Context Engine │      │ Specialist Agents│      │ Tool Registry │
 └───────┬────────┘      └────────┬─────────┘      └───────┬───────┘
         │                        │                         │
         ▼                        ▼                         ▼
 ┌──────────────┐        ┌─────────────────┐      ┌────────────────┐
 │ Code Index    │        │ Coding          │      │ Filesystem     │
 │ RAG / Search  │        │ Debugging       │      │ Terminal       │
 └──────────────┘        │ Database        │      │ Git            │
                          │ Testing         │      │ Browser/API    │
                          │ DevOps          │      │ Docker         │
                          └─────────────────┘      └───────┬────────┘
                                                           │
                                                           ▼
                                                ┌────────────────────┐
                                                │ Sandbox Workspace  │
                                                └─────────┬──────────┘
                                                          │
                                                          ▼
                                                ┌────────────────────┐
                                                │ Verify / Test / Diff│
                                                └─────────┬──────────┘
                                                          │
                                                          ▼
                                                ┌────────────────────┐
                                                │ Human Approval Gate │
                                                └─────────┬──────────┘
                                                          │
                                                          ▼
                                                Commit / Deploy
```

---

# 6. Recommended Technology Stack

## 6.1 Primary recommendation

For the first production architecture:

| Layer | Recommended technology |
|---|---|
| Desktop | Tauri + React/TypeScript |
| Web | React + TypeScript |
| CLI | TypeScript |
| Agent runtime | TypeScript/Node.js |
| API | Fastify or equivalent |
| Validation | Zod |
| Database | PostgreSQL |
| Local metadata DB | SQLite |
| Search | PostgreSQL FTS + vector extension or dedicated vector DB |
| Code parser | Tree-sitter |
| Local model runtime | Ollama-compatible runtime |
| Model abstraction | Provider interface |
| Sandbox | Docker + OS-level isolation |
| Git | Native Git CLI/library |
| Containers | Docker |
| Cache/queue | Redis-compatible system |
| Object storage | Local filesystem initially; S3-compatible later |
| Observability | OpenTelemetry-compatible stack |
| Packaging | Tauri installers + Docker |
| CI | GitHub Actions or equivalent |

### Why TypeScript?

The product is fundamentally a developer tool. TypeScript allows the same language ecosystem across:

- Desktop UI
- Web UI
- CLI
- Agent runtime
- Tool schemas
- API
- Shared types
- MCP integration

Python can be introduced later for specialized ML/data-processing services.

---

# 7. Model Layer

Never hard-code the product to one model.

Create:

```text
ModelProvider
├── LocalProvider
├── OpenAIProvider
├── AnthropicProvider
├── GeminiProvider
└── CustomProvider
```

Common interface:

```ts
interface ModelProvider {
  chat(input: ChatInput): Promise<ModelResponse>;
  stream(input: ChatInput): AsyncIterable<ModelEvent>;
  embed(input: EmbeddingInput): Promise<EmbeddingResponse>;
}
```

The agent runtime should not know which model is underneath.

---

# 8. Local-First AI

A free mode should work approximately like:

```text
                 LOCAL MODE
                     │
       ┌─────────────┼─────────────┐
       ▼             ▼             ▼
 Local Model     Local DB      Local Files
       │             │             │
       └─────────────┼─────────────┘
                     ▼
                Agent Runtime
```

The model runtime can be replaceable. Ollama is one practical local model-serving option.

Important:

- Local inference is not necessarily free in electricity/hardware terms.
- Large models require significant RAM/VRAM.
- Performance varies dramatically by hardware.
- A small local model may be sufficient for simple tool routing but weaker for complex code reasoning.
- Optional cloud providers can be offered without making them mandatory.

---

# 9. Agent Orchestrator

The orchestrator is the heart of the product.

Responsibilities:

1. Understand intent.
2. Build a task plan.
3. Select required context.
4. Select agent/tool.
5. Execute.
6. Observe result.
7. Recover from errors.
8. Verify output.
9. Request approval if required.
10. Finish with a structured result.

Core loop:

```text
REQUEST
  ↓
CLASSIFY
  ↓
PLAN
  ↓
RETRIEVE CONTEXT
  ↓
MODEL DECISION
  ↓
TOOL CALL
  ↓
OBSERVE RESULT
  ↓
VALIDATE
  │
  ├── failure → repair/retry
  │
  └── success
        ↓
      VERIFY
        ↓
   APPROVAL NEEDED?
     │          │
    YES         NO
     │          │
     ▼          ▼
  APPROVE     CONTINUE
     │
     └───────→ COMPLETE
```

---

# 10. Task State Machine

Every task should have a durable state.

```text
CREATED
  ↓
PLANNING
  ↓
WAITING_FOR_CONTEXT
  ↓
EXECUTING
  ↓
VERIFYING
  ↓
WAITING_FOR_APPROVAL
  ↓
COMMITTING
  ↓
COMPLETED
```

Failure paths:

```text
EXECUTING → FAILED → RETRY
VERIFYING → FAILED → REPAIR
APPROVAL → REJECTED → REPLAN
```

---

# 11. Specialist Agents

Start with six.

```text
1. Manager Agent
2. Coding Agent
3. Debugging Agent
4. Testing Agent
5. Database Agent
6. DevOps/Git Agent
```

Later:

```text
7. UI/UX Agent
8. Security Agent
9. Documentation Agent
10. Research Agent
11. Performance Agent
12. Migration Agent
```

Do not create an independent agent for every tiny operation. Prefer tools for simple deterministic tasks and agents for reasoning-heavy tasks.

---

# 12. Manager Agent

Responsibilities:

- Understand user objective.
- Break large tasks into subtasks.
- Assign specialists.
- Maintain overall context.
- Merge results.
- Decide when verification is sufficient.

Example:

```text
"Add subscription support to my Flutter application."

Manager
 ├── Architecture analysis
 ├── Database schema
 ├── API design
 ├── Flutter UI
 ├── Payment integration
 ├── Tests
 └── Documentation
```

---

# 13. Coding Agent

Responsibilities:

- Read relevant code.
- Understand existing conventions.
- Make minimal changes.
- Preserve unrelated functionality.
- Produce patches.
- Explain modifications.

Required tools:

```text
read_file
write_file
edit_file
search_code
find_symbol
find_references
list_directory
get_project_structure
```

---

# 14. Code Intelligence System

This is one of the most important differentiators.

## Pipeline

```text
Repository
    ↓
File Discovery
    ↓
Language Detection
    ↓
Ignore Rules
    ↓
Tree-sitter Parsing
    ↓
Symbol Extraction
    ↓
Dependency Extraction
    ↓
Chunk Generation
    ↓
Embeddings
    ↓
Index
```

Store:

```text
Project
File
Language
Symbol
Function
Class
Import
Reference
Dependency
Chunk
Embedding
Git commit
```

---

# 15. Hybrid Retrieval

Do not rely on vector search alone.

Use:

```text
Keyword Search
      +
Symbol Search
      +
Path Search
      +
AST Search
      +
Vector Search
      +
Git History
      =
Context Retrieval
```

Example:

```text
User:
"Why is student login failing?"

Retriever searches:

1. auth/
2. login symbols
3. API endpoints
4. Firebase/Supabase auth code
5. recent Git changes
6. error logs
7. related tests
```

---

# 16. Context Engineering

The agent should not send the whole repository to the model.

Build context in layers:

```text
Layer 1: User request
Layer 2: Project instructions
Layer 3: Relevant architecture
Layer 4: Relevant files
Layer 5: Relevant symbols
Layer 6: Tool outputs
Layer 7: Test/error information
Layer 8: Previous task state
```

Use a context budget.

---

# 17. Project Instructions

Every repository can have an agent configuration file.

Example:

```yaml
name: Shaurya

stack:
  frontend: Flutter
  backend: Node
  database: PostgreSQL

rules:
  - Do not modify generated files.
  - Run tests after backend changes.
  - Never commit secrets.
  - Use existing architecture before introducing new dependencies.

commands:
  test: flutter test
  build: flutter build apk
  lint: flutter analyze
```

Possible filename:

```text
.agent/project.yaml
```

---

# 18. Memory Architecture

Use separate memory types.

```text
Conversation Memory
Project Memory
Task Memory
User Preferences
Technical Knowledge
Tool History
Decision History
```

Example:

```text
Project Memory
├── architecture decisions
├── database decisions
├── coding conventions
├── known issues
├── deployment configuration
├── dependencies
└── important constraints
```

Memory should be editable and auditable.

---

# 19. Database Architecture

PostgreSQL is recommended for cloud/server deployments.

Core entities:

```text
users
organizations
projects
repositories
workspaces
sessions
messages
tasks
task_steps
agents
agent_runs
tool_calls
approvals
files
symbols
chunks
embeddings
memories
git_commits
test_runs
deployments
audit_logs
secrets_metadata
```

Do not store raw secrets in ordinary application tables.

---

# 20. Workspace Architecture

Each active task should have a workspace.

```text
workspace/
├── repository/
├── task/
├── patches/
├── logs/
├── test-results/
└── artifacts/
```

For higher isolation:

```text
Host
 │
 ├── Agent Runtime
 │
 └── Sandbox Container
       ├── repo
       ├── terminal
       ├── dependencies
       └── test environment
```

---

# 21. Terminal Tool

The terminal is powerful and dangerous.

Never simply expose:

```text
exec("whatever model says")
```

Instead:

```text
Agent
 ↓
Command Policy
 ↓
Risk Classifier
 ↓
Sandbox
 ↓
Command
 ↓
Output
```

Classify commands:

### Low risk

```text
ls
pwd
git status
git diff
npm test
flutter analyze
```

### Medium risk

```text
npm install
docker build
git checkout
database migrations
```

### High risk

```text
rm
git push --force
DROP DATABASE
production deployment
credential operations
```

High-risk operations should require explicit approval.

---

# 22. Git Architecture

Support:

```text
Repository discovery
Clone
Status
Diff
Branch
Commit
Checkout
Merge
Pull
Push
Tag
History
PR preparation
```

Recommended workflow:

```text
main
  │
  └── agent/task-123
        │
        ├── changes
        ├── tests
        └── diff
             ↓
        Human review
             ↓
          merge
```

---

# 23. Database Agent

The database agent should understand:

```text
Schema
Tables
Relations
Indexes
Constraints
Migrations
Queries
Performance
Security
```

Safe workflow:

```text
Natural language request
       ↓
Schema inspection
       ↓
SQL generation
       ↓
SQL validation
       ↓
Dry run / EXPLAIN
       ↓
Approval if destructive
       ↓
Execution
       ↓
Verification
```

Never let a model silently execute destructive production SQL.

---

# 24. Testing Architecture

Testing should happen at multiple levels.

```text
Static Analysis
     ↓
Unit Tests
     ↓
Integration Tests
     ↓
Build
     ↓
Smoke Tests
     ↓
Agent Verification
```

The agent should capture:

```text
command
exit code
stdout
stderr
duration
changed files
test result
```

---

# 25. Self-Repair Loop

A major capability:

```text
WRITE
 ↓
BUILD
 ↓
FAIL
 ↓
READ ERROR
 ↓
LOCATE CODE
 ↓
GENERATE FIX
 ↓
APPLY PATCH
 ↓
TEST
 ↓
PASS?
 ├── NO → retry with limit
 └── YES → continue
```

Always impose retry limits to prevent runaway loops.

---

# 26. Tool Registry

Each tool should have metadata.

```json
{
  "name": "filesystem.read",
  "description": "Read a text file",
  "risk": "low",
  "requiresApproval": false,
  "permissions": ["filesystem.read"],
  "timeout": 10000
}
```

Tool categories:

```text
filesystem.*
terminal.*
git.*
github.*
database.*
browser.*
http.*
docker.*
testing.*
deployment.*
memory.*
search.*
```

---

# 27. MCP Integration

Use MCP as an integration boundary where appropriate.

```text
                    Agent
                      │
                 MCP Client
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
   GitHub Server   DB Server    Custom Server
```

Your native tools remain under your direct control.

MCP should be an extension mechanism, not a replacement for the core security model.

---

# 28. API Architecture

Suggested services:

```text
API Gateway
    │
    ├── Auth Service
    ├── Project Service
    ├── Agent Service
    ├── Task Service
    ├── Memory Service
    ├── Indexing Service
    ├── Tool Service
    ├── Git Service
    ├── Database Service
    └── Deployment Service
```

For MVP, these can remain one modular monolith rather than separate microservices.

Do **not** start with dozens of microservices.

---

# 29. Recommended Backend Structure

```text
backend/
├── src/
│   ├── app/
│   ├── api/
│   ├── agents/
│   │   ├── manager/
│   │   ├── coding/
│   │   ├── debugging/
│   │   ├── testing/
│   │   ├── database/
│   │   └── devops/
│   │
│   ├── orchestrator/
│   ├── models/
│   ├── tools/
│   │   ├── filesystem/
│   │   ├── terminal/
│   │   ├── git/
│   │   ├── database/
│   │   ├── browser/
│   │   └── docker/
│   │
│   ├── context/
│   ├── retrieval/
│   ├── memory/
│   ├── indexer/
│   ├── sandbox/
│   ├── permissions/
│   ├── approvals/
│   ├── security/
│   ├── observability/
│   └── config/
│
├── tests/
└── package.json
```

---

# 30. Desktop Architecture

For a local-first product:

```text
Tauri
│
├── React UI
│
└── Local Agent Runtime
      ├── Model Runtime
      ├── Indexer
      ├── SQLite
      ├── Git
      ├── Sandbox
      └── Tools
```

This reduces cloud dependency.

---

# 31. Web Architecture

Web mode:

```text
Browser
   ↓
HTTPS
   ↓
API Gateway
   ↓
Agent Runtime
   ↓
Sandbox Worker
   ↓
Workspace
```

Never expose host terminal access directly to browser JavaScript.

---

# 32. Authentication

Support:

```text
Local-only mode
Email/password
OAuth
GitHub OAuth
Google OAuth
Organization accounts
```

For local-only mode, users can operate without creating a cloud account.

---

# 33. Secrets Management

Never put:

```text
API keys
Database passwords
OAuth secrets
Cloud credentials
SSH private keys
```

into prompts or normal logs.

Use:

```text
OS keychain
Encrypted local vault
Environment injection
Secret manager
```

The model should receive a logical reference such as:

```text
SECRET_REF:GITHUB_TOKEN
```

not the secret value.

---

# 34. Security Threat Model

Threats include:

### Prompt injection

A repository file could contain malicious instructions.

Treat repository content as **untrusted data**, not system instructions.

### Command injection

Model-generated shell commands can be dangerous.

Use sandbox + command policy.

### Secret exfiltration

Prevent tools from freely reading credential stores.

### Dependency attacks

Package installation can execute scripts.

Run dependency operations inside controlled environments.

### Data leakage

Do not send local source code to cloud models unless the user has explicitly enabled cloud processing.

### Tool abuse

Each tool needs scoped permissions.

---

# 35. Permission Model

Example:

```text
Project
 ├── filesystem.read
 ├── filesystem.write
 ├── terminal.execute
 ├── git.read
 ├── git.write
 ├── database.read
 ├── database.write
 └── deployment.execute
```

Permission modes:

```text
READ_ONLY
ASSISTED
AUTONOMOUS
```

Recommended default:

```text
READ_ONLY → analysis
ASSISTED   → coding
APPROVAL   → destructive actions
```

---

# 36. Audit Logging

Every significant action should create an audit event.

```json
{
  "event": "tool_call",
  "userId": "...",
  "projectId": "...",
  "taskId": "...",
  "tool": "terminal.execute",
  "command": "npm test",
  "risk": "low",
  "approved": true,
  "exitCode": 0
}
```

For privacy, don't automatically store sensitive command output forever.

---

# 37. Observability

Track:

```text
Agent run
Task duration
Model latency
Tool latency
Token usage
Errors
Retries
Test success
Tool failures
Human approvals
Context retrieval quality
```

Use tracing concepts compatible with OpenTelemetry.

---

# 38. Evaluation System

This is essential.

Do not judge the agent only by demos.

Create benchmark tasks:

```text
Task 001: Fix simple bug
Task 002: Add API endpoint
Task 003: Modify database schema
Task 004: Refactor module
Task 005: Fix failing tests
Task 006: Implement UI feature
Task 007: Diagnose build failure
```

Metrics:

```text
Task success rate
Test pass rate
Patch correctness
Regression rate
Tool-call efficiency
Average retries
Human intervention rate
Latency
Cost
```

---

# 39. Agent Evaluation Loop

```text
Benchmark Task
     ↓
Agent Run
     ↓
Patch
     ↓
Automated Tests
     ↓
Static Checks
     ↓
Regression Tests
     ↓
Score
     ↓
Store Trace
     ↓
Improve
```

Build evaluation before large-scale autonomous operation.

---

# 40. Prompt Architecture

Avoid one giant system prompt.

Use layered instructions:

```text
Base Agent Policy
       +
Security Policy
       +
Project Policy
       +
Agent Role
       +
Task Context
       +
Retrieved Code
       +
Tool Results
```

This makes behavior easier to maintain.

---

# 41. Skills System

Instead of hard-coding every workflow, implement reusable skills.

```text
skills/
├── flutter/
├── react/
├── node/
├── postgres/
├── firebase/
├── supabase/
├── docker/
├── github/
└── testing/
```

A skill can contain:

```text
instructions
commands
patterns
validation rules
examples
```

---

# 42. Example Flutter Skill

```yaml
name: flutter-development

detect:
  files:
    - pubspec.yaml

commands:
  analyze: flutter analyze
  test: flutter test
  build_android: flutter build apk

rules:
  - Follow existing project architecture.
  - Do not edit generated files.
  - Run flutter analyze after Dart changes.
```

---

# 43. Project Detection

When opening a project:

```text
Scan
 ↓
Detect files
 ↓
Detect languages
 ↓
Detect frameworks
 ↓
Detect package managers
 ↓
Detect databases
 ↓
Detect Git
 ↓
Detect tests
 ↓
Generate project profile
```

Example:

```json
{
  "frontend": "Flutter",
  "language": ["Dart"],
  "backend": "Node",
  "database": "PostgreSQL",
  "hosting": "Vercel"
}
```

---

# 44. Artifact System

The agent should produce structured artifacts.

```text
Patch
Diff
Test Report
Architecture Diagram
SQL Migration
Deployment Plan
Documentation
Commit
Release Notes
```

Do not make every result plain chat text.

---

# 45. Streaming UI

Agent activity should stream:

```text
Analyzing project...
✓ Found Flutter project

Searching authentication...
✓ Found auth_service.dart

Planning changes...
✓ 4 files affected

Editing...
✓ Updated auth service

Running tests...
✗ 1 test failed

Repairing...
✓ Fixed token parsing

Final verification...
✓ 42 tests passed
```

This builds user trust.

---

# 46. Complete Repository Structure

```text
own-ai-agent/
│
├── apps/
│   ├── desktop/
│   ├── web/
│   └── cli/
│
├── packages/
│   ├── core/
│   ├── agent-runtime/
│   ├── agent-protocol/
│   ├── tool-protocol/
│   ├── shared-types/
│   ├── model-providers/
│   ├── code-intelligence/
│   ├── memory/
│   ├── retrieval/
│   ├── sandbox/
│   └── observability/
│
├── services/
│   ├── api/
│   ├── worker/
│   ├── indexer/
│   └── deployment/
│
├── agents/
│   ├── manager/
│   ├── coding/
│   ├── debugging/
│   ├── testing/
│   ├── database/
│   ├── git/
│   └── devops/
│
├── tools/
│   ├── filesystem/
│   ├── terminal/
│   ├── git/
│   ├── github/
│   ├── database/
│   ├── browser/
│   ├── docker/
│   └── deployment/
│
├── skills/
│   ├── flutter/
│   ├── react/
│   ├── node/
│   ├── python/
│   ├── postgres/
│   ├── firebase/
│   └── docker/
│
├── infrastructure/
│   ├── docker/
│   ├── k8s/
│   ├── terraform/
│   └── monitoring/
│
├── database/
│   ├── migrations/
│   └── seeds/
│
├── docs/
│   ├── architecture/
│   ├── security/
│   ├── api/
│   ├── agents/
│   ├── tools/
│   └── development/
│
├── evals/
│   ├── benchmarks/
│   ├── fixtures/
│   └── reports/
│
├── scripts/
├── tests/
├── .env.example
├── docker-compose.yml
├── LICENSE
└── README.md
```

---

# 47. MVP Scope

Do NOT build everything in Version 1.

### MVP must contain:

```text
✓ Desktop UI
✓ Chat
✓ Project selection
✓ Local model provider
✓ Codebase scanner
✓ Code search
✓ File read
✓ File edit
✓ Terminal sandbox
✓ Git status/diff
✓ Basic coding agent
✓ Test runner
✓ Approval system
✓ Project memory
```

That is enough for a real first product.

---

# 48. Version 2

Add:

```text
✓ Multi-agent orchestration
✓ Tree-sitter indexing
✓ Vector retrieval
✓ Database agent
✓ GitHub integration
✓ Docker
✓ Debugging loops
✓ Skills
✓ MCP
✓ Evaluation framework
```

---

# 49. Version 3

Add:

```text
✓ Autonomous long-running tasks
✓ Cloud workers
✓ Deployment agent
✓ Browser agent
✓ Organization accounts
✓ Team collaboration
✓ Plugin ecosystem
✓ Remote sandboxes
✓ Advanced observability
```

---

# 50. Version 4

Potential advanced research:

```text
✓ Fine-tuned coding models
✓ Specialized code model
✓ Local model routing
✓ Reinforcement/evaluation loops
✓ Distributed agent execution
✓ Multi-machine workspaces
✓ Advanced repository graph
```

Training a foundation model from scratch should be considered a separate research program, not an MVP feature.

---

# 51. Example User Workflow

User:

> "Add a subscription system to my Shaurya Flutter app."

Agent:

```text
1. Detect project
2. Read project configuration
3. Build project map
4. Inspect existing authentication
5. Inspect database
6. Inspect API architecture
7. Ask requirements if genuinely ambiguous
8. Create implementation plan
9. Ask approval for major changes
10. Create branch
11. Implement database changes
12. Implement backend
13. Implement Flutter UI
14. Generate tests
15. Run tests
16. Diagnose failures
17. Repair
18. Run complete verification
19. Show diff
20. Ask for commit/push/deployment approval
```

---

# 52. Example Agent Task Object

```json
{
  "id": "task_123",
  "projectId": "project_001",
  "goal": "Add subscription support",
  "status": "executing",
  "plan": [
    {
      "step": 1,
      "agent": "database",
      "status": "completed"
    },
    {
      "step": 2,
      "agent": "coding",
      "status": "executing"
    },
    {
      "step": 3,
      "agent": "testing",
      "status": "pending"
    }
  ],
  "approvalRequired": true
}
```

---

# 53. Cost Strategy

## Local mode

Potential recurring cloud cost:

```text
Required:
$0 software license for open-source components
$0 mandatory AI API fee
$0 mandatory cloud account
```

User supplies hardware.

## Optional cloud mode

Potential costs:

```text
Cloud inference
Cloud storage
Database
Compute
Embeddings
Observability
Bandwidth
```

The architecture should isolate these behind provider interfaces.

---

# 54. Open-Source Strategy

Recommended repository:

```text
GitHub
   │
   ├── core agent runtime
   ├── desktop app
   ├── CLI
   ├── tools
   ├── skills
   └── documentation
```

Possible licensing choices:

- Apache-2.0
- MIT
- GPL-3.0
- AGPL-3.0

Choose based on whether you want permissive commercial reuse, strong copyleft, or network-service copyleft. Have legal counsel review the final license strategy.

---

# 55. What Should Be Your Proprietary Differentiator?

If the project is open source, the strongest differentiation can still be:

```text
Project Intelligence
+
Agent Runtime
+
Developer Workflow
+
Memory
+
Safety
+
Excellent UX
```

The model itself does not have to be proprietary.

---

# 56. Final Recommended Architecture

```text
                         USER
                           │
              ┌────────────┴────────────┐
              │                         │
           Desktop                    Web/CLI
              │                         │
              └────────────┬────────────┘
                           │
                    AGENT GATEWAY
                           │
                    SESSION MANAGER
                           │
                  ┌────────┴────────┐
                  │  ORCHESTRATOR  │
                  └────────┬────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
     CONTEXT            AGENTS              TOOLS
        │                  │                  │
        │          ┌───────┼───────┐          │
        │          │       │       │          │
        │       Coding  Debug   Database      │
        │          │       │       │          │
        ▼          └───────┼───────┘          ▼
 Code Index               │             Tool Registry
        │                  │                  │
 ┌──────┴──────┐           │        ┌─────────┼─────────┐
 │ AST/Vector  │           │        │         │         │
 │ Search      │           │      Files     Shell      Git
 └─────────────┘           │        │         │         │
                           ▼        └─────────┼─────────┘
                       MODEL LAYER            │
                           │                  │
                  ┌────────┴────────┐         │
                  │                 │         │
              Local Model      Cloud Model    │
                  │                 │         │
                  └────────┬────────┘         │
                           │                  │
                           └────────┬─────────┘
                                    │
                              SANDBOX
                                    │
                             TEST / VERIFY
                                    │
                             APPROVAL GATE
                                    │
                           COMMIT / DEPLOY
```

---

# 57. Build Order

The recommended implementation sequence is:

```text
STEP 01
Repository + monorepo

STEP 02
Desktop UI

STEP 03
Agent runtime

STEP 04
Model abstraction

STEP 05
Project scanner

STEP 06
Filesystem tools

STEP 07
Terminal sandbox

STEP 08
Coding agent

STEP 09
Git tools

STEP 10
Testing system

STEP 11
Tree-sitter code intelligence

STEP 12
Hybrid retrieval

STEP 13
Project memory

STEP 14
Multi-agent orchestrator

STEP 15
Database agent

STEP 16
MCP integration

STEP 17
Docker isolation

STEP 18
GitHub integration

STEP 19
DevOps/deployment

STEP 20
Evaluation platform

STEP 21
Security audit

STEP 22
Public open-source release
```

---

# 58. Definition of "Production Ready"

The system should not be called production-ready merely because it can generate code.

It should satisfy:

```text
[ ] Sandboxed execution
[ ] Permission system
[ ] Approval gates
[ ] Secret protection
[ ] Audit logging
[ ] Durable task state
[ ] Retry limits
[ ] Test verification
[ ] Regression testing
[ ] Code indexing
[ ] Context retrieval
[ ] Model fallback
[ ] Offline/local mode
[ ] Crash recovery
[ ] Observability
[ ] Evaluation benchmarks
[ ] Backup/recovery
[ ] Dependency security
[ ] Documentation
[ ] License compliance
[ ] Security review
```

---

# 59. Final Engineering Principle

The most important principle for this project is:

> **The AI should propose and execute software changes through controlled tools, not directly control the user's computer without boundaries.**

A high-quality architecture therefore separates:

```text
Reasoning
   ≠
Execution
   ≠
Authorization
   ≠
Verification
```

The model reasons.

The orchestrator plans.

Tools execute.

The permission system authorizes.

The sandbox contains.

Tests verify.

The user approves high-impact actions.

That separation is the foundation for a trustworthy coding agent.

---

# 60. Research References

1. OpenAI Agents SDK documentation  
   https://developers.openai.com/api/docs/guides/agents/sdk

2. OpenAI Agents documentation  
   https://developers.openai.com/api/docs/guides/agents

3. OpenAI Agents SDK Python documentation  
   https://openai.github.io/openai-agents-python/

4. OpenAI Agents SDK TypeScript documentation  
   https://openai.github.io/openai-agents-js/

5. Model Context Protocol architecture  
   https://modelcontextprotocol.io/docs/learn/architecture

6. Tree-sitter documentation  
   https://tree-sitter.github.io/tree-sitter/

7. Tree-sitter GitHub  
   https://github.com/tree-sitter/tree-sitter

---

# 61. Recommended Next Deliverable

After this architecture document, the next engineering document should be:

**`AI_AGENT_TECH_STACK_AND_IMPLEMENTATION_PLAN.md`**

It should define:

1. Exact technologies and versions.
2. Monorepo setup.
3. Every package.
4. Every folder.
5. Database schema.
6. API endpoints.
7. Agent protocols.
8. Tool schemas.
9. Permission schemas.
10. MCP architecture.
11. Code-indexing implementation.
12. RAG implementation.
13. Local model setup.
14. Sandbox implementation.
15. Desktop application architecture.
16. Authentication.
17. Testing.
18. Evaluation benchmarks.
19. CI/CD.
20. Development milestones.

This should become the actual engineering blueprint from which Version 1 can be implemented.
