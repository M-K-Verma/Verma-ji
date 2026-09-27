# AI Coding Agent — Complete Requirements Specification

**Document:** `requirements.md`  
**Version:** 1.0  
**Date:** 2026-09-27  
**Status:** Product Requirements / Engineering Baseline

---

## 1. Purpose

This document defines the functional, technical, security, performance, usability, and operational requirements for a **local-first, open-source AI coding agent**.

The system will allow users to interact with an AI that can understand software projects, search and modify code, execute development tools in controlled environments, work with databases, manage Git workflows, run tests, diagnose errors, maintain project memory, and optionally assist with deployment.

The system must support a **free local mode** where users can run the agent with a locally available model and local project data without requiring mandatory paid AI APIs.

---

# 2. Product Goals

## 2.1 Primary Goals

The system MUST:

- Provide an AI-powered development assistant.
- Understand complete software projects rather than isolated files.
- Maintain project-specific context and memory.
- Read, create, modify, rename and delete files according to permissions.
- Search source code intelligently.
- Execute development commands in a sandbox.
- Analyze build and test failures.
- Generate and run tests.
- Integrate with Git.
- Support database inspection and controlled database operations.
- Support multiple AI model providers.
- Support local AI models.
- Provide human approval for high-risk operations.
- Keep a complete audit trail of important agent actions.
- Provide extensibility through tools, skills and MCP-compatible integrations.
- Work as a desktop-first local application.
- Provide a web/CLI interface as the architecture matures.

---

# 3. Non-Goals for Version 1

Version 1 SHOULD NOT attempt to:

- Train a foundation model from scratch.
- Automatically deploy to production without approval.
- Give unrestricted host-machine access to the model.
- Execute unrestricted shell commands.
- Automatically modify production databases.
- Store plaintext API keys in prompts, logs or normal database tables.
- Build dozens of independent microservices.
- Support every programming language initially.
- Guarantee perfect autonomous coding.

The first release should focus on **safe, reliable software-development workflows**.

---

# 4. Target Users

## 4.1 Individual Developers

The agent should help with:

- Coding
- Debugging
- Refactoring
- Testing
- Documentation
- Git
- Project architecture

## 4.2 Students

The system may provide:

- Code explanations
- Learning assistance
- Project guidance
- Debugging help

## 4.3 Freelancers

The agent should support:

- Multiple projects
- Client repositories
- Repetitive development tasks
- Deployment assistance

## 4.4 Development Teams

Future versions may support:

- Organizations
- Shared projects
- Shared agent instructions
- Team permissions
- Shared memory
- Audit logs

---

# 5. Supported Development Ecosystem

The architecture MUST be extensible.

Initial priority:

```text
Flutter / Dart
React
JavaScript
TypeScript
Node.js
Python
HTML
CSS
SQL
```

Future support:

```text
Java
Kotlin
C#
C++
Go
Rust
Swift
PHP
Ruby
```

The agent SHOULD detect the project's technology automatically.

---

# 6. Platform Requirements

## 6.1 Desktop

Primary platform:

```text
Windows
```

Target:

```text
macOS
Linux
```

## 6.2 Web

A web interface SHOULD be supported.

## 6.3 CLI

A command-line interface SHOULD be provided for advanced developers.

## 6.4 Mobile

Mobile applications are optional for Version 1.

A future mobile application may provide:

- Chat
- Task monitoring
- Approval requests
- Logs
- Project status

Mobile should not initially receive unrestricted local-machine execution capabilities.

---

# 7. Core System Components

The system MUST contain:

```text
Client Application
Agent Gateway
Agent Orchestrator
Model Manager
Context Engine
Code Indexer
Memory System
Tool Registry
Tool Execution Layer
Sandbox
Permission System
Approval System
Git Integration
Testing System
Database Integration
Observability
Audit Logging
Configuration System
```

---

# 8. User Interface Requirements

## 8.1 Main Interface

The UI SHOULD contain:

```text
Sidebar
 ├── Projects
 ├── Tasks
 ├── Agents
 ├── Memory
 ├── Settings
 └── Integrations

Main Area
 ├── Chat
 ├── Task Plan
 ├── Agent Activity
 └── Results

Bottom/Side Panel
 ├── Terminal
 ├── Git Diff
 ├── Tests
 ├── Logs
 └── Files
```

## 8.2 Chat

The user MUST be able to:

- Send natural-language requests.
- Attach files.
- Reference projects.
- Reference tasks.
- Cancel running tasks.
- Approve/reject actions.
- View tool calls.
- View agent progress.
- View final results.

## 8.3 Streaming

Agent execution SHOULD stream status events such as:

```text
Analyzing project...
Searching authentication code...
Planning changes...
Editing files...
Running tests...
Fixing failure...
Verification complete.
```

---

# 9. Project Management Requirements

Users MUST be able to:

- Add a local project.
- Open a project.
- Remove a project from the agent.
- Rescan a project.
- Re-index a project.
- View project technology.
- View Git status.
- View project configuration.
- Configure agent rules.

Project metadata SHOULD include:

```text
Project ID
Project Name
Root Path
Repository
Primary Language
Framework
Package Manager
Database
Build Commands
Test Commands
Lint Commands
Agent Configuration
Index Status
Last Scan
```

---

# 10. Project Configuration

Each project SHOULD support:

```text
.agent/
    project.yaml
    instructions.md
    permissions.yaml
```

Example:

```yaml
name: My Project

stack:
  frontend: Flutter
  backend: Node.js
  database: PostgreSQL

commands:
  test: flutter test
  lint: flutter analyze

rules:
  - Do not modify generated files.
  - Do not commit secrets.
  - Run tests after code changes.
```

The agent MUST treat project configuration as lower priority than system security policies.

---

# 11. Agent Orchestrator Requirements

The orchestrator MUST:

- Receive a user task.
- Classify the task.
- Determine whether context is required.
- Create a task plan.
- Select appropriate agents.
- Select tools.
- Execute steps.
- Track state.
- Handle errors.
- Retry within configured limits.
- Verify results.
- Request approval where necessary.
- Produce a final structured result.

Task lifecycle:

```text
CREATED
↓
PLANNING
↓
CONTEXT_RETRIEVAL
↓
EXECUTING
↓
VERIFYING
↓
APPROVAL
↓
COMPLETED
```

Failure states:

```text
FAILED
CANCELLED
REJECTED
TIMEOUT
```

---

# 12. Agent Requirements

## 12.1 Manager Agent

MUST:

- Understand high-level tasks.
- Decompose tasks.
- Assign specialist agents.
- Track progress.
- Combine results.
- Trigger verification.

## 12.2 Coding Agent

MUST:

- Read code.
- Search code.
- Understand project structure.
- Modify code.
- Generate code.
- Refactor code.
- Follow project conventions.
- Produce diffs.

## 12.3 Debugging Agent

MUST:

- Read errors.
- Analyze logs.
- Locate likely source.
- Propose fixes.
- Apply fixes when authorized.
- Re-run verification.

## 12.4 Testing Agent

MUST:

- Detect existing tests.
- Generate tests where appropriate.
- Run tests.
- Parse results.
- Report failures.

## 12.5 Database Agent

MUST:

- Inspect schemas.
- Generate SQL.
- Explain queries.
- Analyze migrations.
- Detect potentially destructive operations.

## 12.6 Git Agent

MUST support:

```text
status
diff
branch
checkout
commit
log
merge
pull
push
tag
```

Production or destructive Git operations SHOULD require approval.

## 12.7 DevOps Agent

Future versions SHOULD support:

```text
Docker
CI/CD
Cloud deployment
Environment configuration
Logs
Monitoring
Rollback
```

---

# 13. Model Requirements

The model layer MUST be provider-independent.

Required abstraction:

```text
ModelProvider
 ├── Local
 ├── Cloud Provider A
 ├── Cloud Provider B
 └── Custom Provider
```

The agent runtime MUST NOT depend directly on one model vendor.

The model interface SHOULD support:

```text
Chat
Streaming
Tool Calling
Structured Output
Embeddings
Context Limits
Model Metadata
Cancellation
Timeout
```

---

# 14. Local AI Requirements

Local mode MUST be supported.

The architecture SHOULD support a local model runtime such as an Ollama-compatible environment.

Requirements:

- Detect available local models.
- Select a model.
- Configure model parameters.
- Stream responses.
- Handle unavailable models.
- Allow users to install/change models.
- Avoid mandatory cloud calls.

The application MUST clearly indicate whether a request is being processed locally or by a cloud provider.

---

# 15. Code Intelligence Requirements

The system MUST provide repository-level code intelligence.

The code intelligence pipeline SHOULD include:

```text
File Scanner
↓
Language Detector
↓
Parser
↓
AST
↓
Symbol Extraction
↓
Dependency Extraction
↓
Chunking
↓
Indexing
```

Tree-sitter SHOULD be evaluated as the primary structural parsing technology.

The system SHOULD identify:

```text
Files
Classes
Functions
Methods
Variables
Imports
Exports
Types
References
Dependencies
Routes
API endpoints
Database models
Tests
```

---

# 16. Code Search Requirements

The system MUST support:

### Text Search

```text
Exact text
Regex
Filename
Path
```

### Structural Search

```text
Class
Function
Method
Symbol
Reference
Import
```

### Semantic Search

The system SHOULD support embeddings-based retrieval.

### Hybrid Search

Preferred:

```text
Keyword
+
Symbol
+
Structural
+
Semantic
+
Git History
```

---

# 17. Context Engine Requirements

The agent MUST avoid sending the entire repository to the model by default.

Context should be selected using:

```text
User request
+
Project instructions
+
Relevant files
+
Relevant symbols
+
Dependencies
+
Recent changes
+
Tool output
+
Test failures
+
Task memory
```

The system SHOULD maintain configurable context budgets.

---

# 18. Memory Requirements

The memory system MUST distinguish:

```text
Conversation Memory
Task Memory
Project Memory
User Preferences
Technical Knowledge
Decision History
```

Project memory may contain:

```text
Architecture
Coding conventions
Database decisions
Known issues
Deployment information
Dependencies
Important constraints
```

Users MUST be able to inspect and manage project memory.

Sensitive information MUST NOT be automatically stored as normal memory.

---

# 19. RAG Requirements

The retrieval system SHOULD support:

```text
Documents
Source code
Documentation
Project configuration
Git history
Issues
Task results
```

Retrieval should return:

```text
Source
Path
Line/region
Relevance
Content
Metadata
```

The agent SHOULD cite internal project files when explaining decisions.

---

# 20. Tool System Requirements

All tools MUST have:

```text
Name
Description
Input Schema
Output Schema
Risk Level
Permissions
Timeout
Resource Limits
```

Example categories:

```text
filesystem.*
terminal.*
git.*
database.*
github.*
browser.*
docker.*
testing.*
deployment.*
memory.*
search.*
```

---

# 21. Filesystem Requirements

The filesystem tool MUST support:

```text
List
Read
Search
Create
Write
Edit
Rename
Move
Delete
```

Security requirements:

- Restrict access to authorized workspaces.
- Prevent path traversal.
- Prevent unintended access to credential directories.
- Log write/delete operations.
- Require approval for configured destructive operations.

---

# 22. Terminal Requirements

The terminal tool MUST:

- Run commands in a controlled workspace.
- Capture stdout.
- Capture stderr.
- Capture exit code.
- Capture duration.
- Support timeout.
- Support cancellation.
- Limit resources.
- Apply command policies.

The system MUST NOT allow unrestricted host execution by default.

---

# 23. Sandbox Requirements

The sandbox SHOULD use:

```text
Docker
+
OS-level isolation where available
+
Filesystem restrictions
+
Network policy
+
CPU limits
+
Memory limits
+
Timeouts
```

Each task SHOULD have an isolated workspace when practical.

The sandbox MUST prevent unauthorized access to:

```text
SSH keys
Browser credentials
OS credentials
Cloud credentials
Unrelated user files
```

---

# 24. Permission Requirements

Permission categories:

```text
filesystem.read
filesystem.write
filesystem.delete

terminal.read
terminal.execute

git.read
git.write

database.read
database.write
database.delete

network.read
network.write

deployment.execute
secret.read
```

Permission modes:

```text
READ_ONLY
ASSISTED
AUTONOMOUS
```

High-risk capabilities MUST NOT automatically inherit permission from low-risk capabilities.

---

# 25. Approval Requirements

Approval MUST be supported for:

```text
Production deployment
Production database write
Database deletion
File deletion
Force push
Credential operations
Destructive shell commands
External side effects
```

The approval UI SHOULD display:

```text
Action
Reason
Tool
Exact command/query
Affected files/resources
Risk level
Expected result
```

The user MUST be able to:

```text
Approve
Reject
Edit
Approve once
Approve for task
```

---

# 26. Git Requirements

The system MUST support repository detection.

Git features:

```text
Status
Diff
History
Branch
Checkout
Commit
Merge
Pull
Push
Tag
```

The agent SHOULD create a task-specific branch for significant changes.

Example:

```text
agent/task-123
```

Before commit, the UI SHOULD show:

```text
Changed files
Diff
Tests
Warnings
```

---

# 27. Database Requirements

The database subsystem SHOULD initially prioritize:

```text
PostgreSQL
SQLite
Supabase/PostgreSQL
```

Future:

```text
MySQL
MongoDB
Firestore
Redis
```

The database agent MUST be able to:

- Inspect schema.
- Inspect tables.
- Inspect indexes.
- Inspect relationships.
- Generate queries.
- Explain queries.
- Generate migrations.
- Run safe read-only queries.

Destructive queries MUST require approval.

---

# 28. Testing Requirements

The agent MUST support:

```text
Test discovery
Test execution
Result parsing
Failure analysis
Test generation
Regression verification
```

The system SHOULD detect common tools such as:

```text
flutter test
npm test
pnpm test
pytest
jest
vitest
cargo test
go test
```

The project configuration should override automatic detection when explicitly defined.

---

# 29. Self-Repair Requirements

The system SHOULD support controlled self-repair:

```text
Modify
↓
Build/Test
↓
Failure
↓
Analyze
↓
Patch
↓
Test
↓
Repeat
```

Limits MUST exist for:

```text
Maximum retries
Maximum execution time
Maximum token/context budget
Maximum file modifications
```

The agent MUST stop and report when limits are reached.

---

# 30. GitHub / Git Hosting Requirements

Future integration SHOULD support:

```text
Repository listing
Issue reading
Pull request creation
Pull request review
Branch management
Commit/push
Release information
```

Credentials MUST be scoped and securely stored.

---

# 31. MCP Requirements

The system SHOULD support MCP-compatible integrations.

MCP capabilities should be treated as external tools and subjected to the same:

```text
Permission
Risk
Approval
Audit
Timeout
Network
```

policies as native tools.

---

# 32. Skills Requirements

The system SHOULD support reusable skills.

Example:

```text
skills/
├── flutter/
├── react/
├── node/
├── python/
├── postgres/
├── firebase/
├── supabase/
├── docker/
└── github/
```

Each skill can define:

```text
Detection
Instructions
Commands
Patterns
Validation
Examples
```

Users SHOULD be able to add custom skills.

---

# 33. Browser Requirements

A future browser tool MAY support:

```text
Documentation research
Web testing
API documentation
Developer portals
```

Browser access MUST be isolated and subject to network policy.

The agent MUST NOT treat arbitrary web content as trusted instructions.

---

# 34. Deployment Requirements

Future DevOps functionality SHOULD support:

```text
Docker
Vercel
Railway
Firebase
Supabase
Google Cloud
AWS
Azure
GitHub Actions
```

Deployment workflow:

```text
Build
↓
Test
↓
Security checks
↓
Preview
↓
Approval
↓
Deploy
↓
Health check
↓
Report
```

Production deployment MUST require explicit approval by default.

---

# 35. Authentication Requirements

Local mode:

```text
No mandatory account
```

Cloud mode:

```text
Email/password
OAuth
GitHub
Google
```

Future:

```text
Organizations
Teams
RBAC
SSO
```

---

# 36. Secrets Requirements

Secrets MUST NOT be:

- Included in prompts.
- Included in normal logs.
- Stored in plaintext.
- Sent to an AI model unnecessarily.
- Exposed through tool output.

Possible storage:

```text
OS Keychain
Encrypted local vault
Cloud secret manager
Environment injection
```

---

# 37. Security Requirements

The system MUST defend against:

```text
Prompt injection
Command injection
Path traversal
Secret leakage
Malicious repositories
Malicious dependencies
Tool abuse
Unauthorized network access
Privilege escalation
Data exfiltration
```

Repository files MUST be treated as untrusted content.

---

# 38. Audit Requirements

The system MUST record important events:

```text
Agent run
Task
Tool call
File modification
Command execution
Database query
Approval
Rejection
Git operation
Deployment
Error
```

Audit records SHOULD include:

```text
Timestamp
User
Project
Task
Tool
Action
Risk
Approval status
Result
```

Sensitive output SHOULD be redacted or excluded.

---

# 39. Privacy Requirements

Local mode SHOULD keep project source code on the user's machine.

Cloud processing MUST clearly disclose that project content may be sent to the selected provider.

The user SHOULD be able to configure:

```text
Local only
Cloud allowed
Specific provider allowed
Never send selected paths
```

Sensitive directories SHOULD support exclusion rules.

---

# 40. Performance Requirements

The application SHOULD:

- Start quickly.
- Stream responses.
- Avoid blocking the UI during indexing.
- Perform indexing incrementally.
- Cache repeated retrieval.
- Cancel obsolete tasks.
- Limit unnecessary model calls.

Code indexing SHOULD be incremental rather than rebuilding the entire index after every edit.

---

# 41. Scalability Requirements

The architecture MUST support two modes.

## Local

```text
Single-user
Single-machine
Local model
Local database
Local sandbox
```

## Cloud

```text
Multiple users
Organizations
Remote workers
Cloud model providers
Central database
Remote sandbox workers
```

The core agent protocol SHOULD remain consistent between modes.

---

# 42. Reliability Requirements

The system SHOULD recover from:

```text
Model timeout
Tool timeout
Network failure
Process crash
Agent cancellation
Sandbox failure
Database connection failure
Partial task completion
```

Tasks MUST have durable states.

The agent SHOULD be able to resume interrupted tasks where safe.

---

# 43. Cancellation Requirements

Users MUST be able to cancel:

```text
Agent run
Tool call
Terminal process
Indexing job
Deployment task
```

Cancellation MUST propagate to child processes where possible.

---

# 44. Observability Requirements

Metrics SHOULD include:

```text
Task duration
Model latency
Tool latency
Token usage
Retrieval latency
Sandbox duration
Test duration
Error rate
Retry rate
Success rate
Approval rate
```

Tracing SHOULD connect:

```text
User request
→ Agent run
→ Model call
→ Retrieval
→ Tool calls
→ Tests
→ Final result
```

---

# 45. Evaluation Requirements

The project MUST have an automated benchmark suite.

Example benchmark categories:

```text
Bug fixing
Feature implementation
Refactoring
Testing
Database changes
API development
UI changes
Build debugging
Documentation
```

Metrics:

```text
Task success
Test pass
Regression rate
Patch quality
Human intervention
Tool efficiency
Latency
Resource consumption
```

Evaluation results SHOULD be versioned.

---

# 46. API Requirements

The backend API SHOULD expose:

```text
POST /projects
GET /projects
GET /projects/:id

POST /tasks
GET /tasks/:id
POST /tasks/:id/cancel
POST /tasks/:id/approve
POST /tasks/:id/reject

GET /projects/:id/files
GET /projects/:id/search

GET /projects/:id/git/status
GET /projects/:id/git/diff

GET /tasks/:id/events

GET /models
POST /models/configure
```

API design may evolve; internal APIs SHOULD remain typed.

---

# 47. Data Model Requirements

Core entities:

```text
User
Project
Repository
Workspace
Session
Message
Task
TaskStep
Agent
AgentRun
Tool
ToolCall
Approval
File
Symbol
CodeChunk
Embedding
Memory
GitCommit
TestRun
Deployment
AuditEvent
```

Every project/task-related entity SHOULD have stable IDs.

---

# 48. Configuration Requirements

Configuration MUST support:

```text
Model
Model provider
Workspace
Sandbox
Permissions
Tools
Network
Memory
Indexing
Git
Database
Logging
Telemetry
```

Configuration precedence:

```text
System Security Policy
>
User Security Policy
>
Project Policy
>
Task Configuration
>
Model Suggestion
```

The model MUST never override security policy.

---

# 49. Logging Requirements

Logs SHOULD have levels:

```text
ERROR
WARN
INFO
DEBUG
TRACE
```

Logs MUST avoid secrets.

Tool execution SHOULD record metadata without unnecessarily storing sensitive command output.

---

# 50. Backup Requirements

Cloud mode SHOULD support backups of:

```text
Project metadata
Task history
Memory
Configuration
Audit logs
```

Source code should not automatically be copied to cloud storage unless the user chooses a cloud workspace.

---

# 51. Plugin Requirements

Future plugin system SHOULD support:

```text
New model providers
New tools
New skills
New databases
New deployment providers
New UI extensions
```

Plugins MUST declare:

```text
Permissions
Capabilities
Network requirements
Data access
Version
```

---

# 52. Licensing Requirements

The project should be released under a license selected according to the desired open-source/commercial model.

Candidates:

```text
MIT
Apache-2.0
GPL-3.0
AGPL-3.0
```

All third-party dependencies MUST be checked for license compatibility.

---

# 53. Documentation Requirements

The repository MUST contain:

```text
README
Installation Guide
Architecture Guide
Security Guide
Configuration Guide
Agent Development Guide
Tool Development Guide
Skill Development Guide
Plugin Guide
API Documentation
Troubleshooting Guide
Contribution Guide
License
```

---

# 54. CI/CD Requirements

CI SHOULD run:

```text
Formatting
Linting
Type checking
Unit tests
Integration tests
Security scanning
Dependency checks
Build
Packaging
```

Pull requests SHOULD NOT be merged when mandatory checks fail.

---

# 55. Versioning Requirements

Use semantic versioning where practical:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
0.1.0
0.2.0
1.0.0
```

Agent protocol changes SHOULD be versioned independently where needed.

---

# 56. MVP Acceptance Criteria

Version 1 is acceptable when a user can:

1. Install the application.
2. Add a local code project.
3. Ask the AI about the project.
4. Search the codebase.
5. Ask the AI to modify code.
6. Review the generated diff.
7. Run tests through the agent.
8. See test results.
9. Ask the agent to repair a failed test.
10. View Git status/diff.
11. Approve/reject risky actions.
12. Use a local AI model.
13. Maintain project-specific instructions.
14. Recover from common task failures.
15. See agent activity and logs.

---

# 57. Production Acceptance Criteria

Before public production release:

```text
[ ] Security review complete
[ ] Sandbox tested
[ ] Prompt injection tests complete
[ ] Secret leakage tests complete
[ ] Permission tests complete
[ ] Path traversal tests complete
[ ] Command policy tests complete
[ ] Database safety tests complete
[ ] Agent benchmark established
[ ] Crash recovery tested
[ ] Cancellation tested
[ ] Audit logging tested
[ ] Dependency licenses reviewed
[ ] Documentation complete
[ ] Windows build tested
[ ] macOS build tested
[ ] Linux build tested
[ ] Local model mode tested
[ ] Cloud model mode tested
```

---

# 58. Priority Matrix

## P0 — Must Have

```text
Agent runtime
Model abstraction
Project management
Code search
File tools
Coding agent
Terminal sandbox
Git
Testing
Permissions
Approvals
Security
Local model
Project memory
```

## P1 — High Priority

```text
Tree-sitter
Hybrid retrieval
Debugging agent
Database agent
MCP
GitHub
Docker
Evaluation system
```

## P2 — Future

```text
Browser agent
DevOps automation
Cloud workers
Team collaboration
Plugin marketplace
Mobile application
Advanced model routing
```

---

# 59. Recommended Initial Architecture

```text
Desktop UI
    │
    ▼
Local Agent Runtime
    │
    ├── Orchestrator
    ├── Model Manager
    ├── Context Engine
    ├── Coding Agent
    ├── Debug Agent
    ├── Testing Agent
    ├── Git Agent
    ├── Tool Registry
    ├── Permission Engine
    ├── Approval Engine
    ├── Project Memory
    ├── Code Indexer
    └── Sandbox
          │
          ├── Filesystem
          ├── Terminal
          ├── Git
          └── Test Runner
```

This should be the initial implementation target.

---

# 60. Recommended Build Sequence

## Milestone 1 — Foundation

```text
Repository
Monorepo
Desktop shell
Shared types
Configuration
Logging
```

## Milestone 2 — AI

```text
Model provider
Chat
Streaming
Conversation state
```

## Milestone 3 — Project Intelligence

```text
Project scanner
File search
Code parser
Context engine
```

## Milestone 4 — Coding

```text
File editing
Patch generation
Diff viewer
Coding agent
```

## Milestone 5 — Execution

```text
Terminal
Sandbox
Test runner
Verification
```

## Milestone 6 — Git

```text
Git status
Diff
Branch
Commit
Approval
```

## Milestone 7 — Memory

```text
Project memory
RAG
Code index
Hybrid search
```

## Milestone 8 — Multi-Agent

```text
Manager
Coding
Debugging
Testing
Database
DevOps
```

## Milestone 9 — Integrations

```text
MCP
GitHub
Docker
Cloud providers
```

## Milestone 10 — Production

```text
Security audit
Evaluation
Packaging
Documentation
Release
```

---

# 61. Definition of Done for Every Agent Task

A task should not be marked complete merely because the model generated code.

A task is complete only when:

```text
Plan complete
+
Required code changes applied
+
No unexpected modifications
+
Build/test completed
+
Relevant tests pass
+
Known errors resolved
+
Diff reviewed/generated
+
Required approval obtained
+
Final result recorded
```

If verification cannot be completed, the status should be:

```text
PARTIALLY_VERIFIED
```

rather than falsely reporting success.

---

# 62. Core Engineering Rules

1. **Model output is untrusted.**
2. **Repository content is untrusted.**
3. **Tools are privileged capabilities.**
4. **Permissions are separate from reasoning.**
5. **Execution is separate from authorization.**
6. **Verification is mandatory for code changes where practical.**
7. **Production actions require explicit approval by default.**
8. **Secrets never belong in model context unless absolutely necessary.**
9. **Local-first mode should minimize data leaving the user's machine.**
10. **Every autonomous loop needs limits.**
11. **The agent should make minimal, reversible changes.**
12. **The user must be able to inspect important actions.**
13. **The system must fail safely rather than silently guessing.**
14. **Every important agent behavior should be testable and measurable.**

---

# 63. Final Requirement

The finished system should behave less like:

> "A chatbot that writes code"

and more like:

> "A controlled AI development environment that understands a software project, plans work, uses developer tools, modifies code, verifies results, remembers project decisions, and asks the human when authorization is required."

The architecture must preserve this separation:

```text
UNDERSTAND
    ↓
PLAN
    ↓
RETRIEVE
    ↓
REASON
    ↓
EXECUTE
    ↓
VERIFY
    ↓
AUTHORIZE
    ↓
COMMIT / DEPLOY
```

This sequence is the central product requirement for the AI coding agent.
