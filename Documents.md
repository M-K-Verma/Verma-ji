# Documents Specification — Own AI Agent

## 1. Purpose

This document defines the complete documentation system required for building, maintaining, releasing, and operating the Own AI Agent.

The goal is to make the project understandable for:
- Core developers
- AI/agent developers
- Tool/plugin developers
- Security engineers
- Contributors
- QA/test engineers
- DevOps engineers
- End users
- Open-source maintainers

Documentation must be treated as a first-class engineering component.

---

# 2. Documentation Principles

The project documentation should follow these rules:

1. Every major system must have documentation.
2. Architecture decisions must be recorded.
3. Public APIs must be documented.
4. Agent behavior and tool permissions must be documented.
5. Security-sensitive behavior must be explicitly documented.
6. Configuration options must have examples.
7. Installation must be documented for supported platforms.
8. Development setup must be reproducible.
9. Breaking changes must be documented.
10. Documentation should be version-controlled with the source code.
11. Examples should be copy-paste friendly.
12. Documentation should distinguish stable APIs from experimental features.

---

# 3. Documentation Architecture

Recommended documentation structure:

```text
docs/
├── README.md
│
├── getting-started/
│   ├── installation.md
│   ├── quick-start.md
│   ├── first-project.md
│   ├── configuration.md
│   └── troubleshooting.md
│
├── architecture/
│   ├── overview.md
│   ├── system-architecture.md
│   ├── agent-architecture.md
│   ├── model-architecture.md
│   ├── tool-architecture.md
│   ├── code-intelligence.md
│   ├── memory-architecture.md
│   ├── database-architecture.md
│   ├── sandbox-architecture.md
│   └── deployment-architecture.md
│
├── agents/
│   ├── manager-agent.md
│   ├── coding-agent.md
│   ├── debugging-agent.md
│   ├── testing-agent.md
│   ├── database-agent.md
│   ├── git-agent.md
│   └── devops-agent.md
│
├── tools/
│   ├── filesystem.md
│   ├── terminal.md
│   ├── git.md
│   ├── github.md
│   ├── database.md
│   ├── browser.md
│   ├── docker.md
│   └── deployment.md
│
├── models/
│   ├── model-providers.md
│   ├── local-models.md
│   ├── model-routing.md
│   ├── context-management.md
│   └── model-evaluation.md
│
├── code-intelligence/
│   ├── indexing.md
│   ├── tree-sitter.md
│   ├── symbols.md
│   ├── dependency-graph.md
│   ├── embeddings.md
│   └── retrieval.md
│
├── memory/
│   ├── overview.md
│   ├── project-memory.md
│   ├── conversation-memory.md
│   ├── semantic-memory.md
│   └── memory-policies.md
│
├── security/
│   ├── security-overview.md
│   ├── permissions.md
│   ├── sandbox.md
│   ├── secrets.md
│   ├── threat-model.md
│   ├── audit-logging.md
│   └── incident-response.md
│
├── api/
│   ├── overview.md
│   ├── authentication.md
│   ├── projects.md
│   ├── agents.md
│   ├── tools.md
│   ├── tasks.md
│   ├── memory.md
│   └── streaming.md
│
├── development/
│   ├── setup.md
│   ├── repository-structure.md
│   ├── coding-standards.md
│   ├── testing.md
│   ├── debugging.md
│   ├── adding-an-agent.md
│   ├── adding-a-tool.md
│   ├── adding-a-skill.md
│   └── pull-requests.md
│
├── skills/
│   ├── flutter.md
│   ├── react.md
│   ├── node.md
│   ├── python.md
│   ├── postgres.md
│   ├── firebase.md
│   └── docker.md
│
├── deployment/
│   ├── local.md
│   ├── docker.md
│   ├── server.md
│   ├── desktop-release.md
│   └── production.md
│
├── contributing/
│   ├── contribution-guide.md
│   ├── code-of-conduct.md
│   ├── issue-guide.md
│   └── security-reporting.md
│
├── releases/
│   ├── release-process.md
│   ├── versioning.md
│   └── changelog.md
│
└── adr/
    ├── README.md
    └── ADR-0001-template.md
```

---

# 4. Root Documentation

## 4.1 README.md

The root README must explain:

- What the AI Agent is
- Main capabilities
- Supported platforms
- Supported programming languages
- Local AI support
- Optional cloud AI support
- Installation
- Quick start
- Security model
- Open-source license
- Contribution process
- Documentation links

Recommended opening:

```text
Own AI Agent is a local-first, open-source AI development agent designed to
understand software projects, modify code, work with databases, run tests,
debug applications, manage Git repositories, and assist with deployment.
```

---

# 5. Getting Started Documentation

## 5.1 Installation

Document installation for:

- Windows
- Linux
- macOS
- Docker
- CLI
- Desktop application

Include:

- System requirements
- Node.js/runtime requirements
- Git requirement
- Docker requirement
- Local model runtime requirements
- Installation commands
- Verification commands
- Common installation errors

## 5.2 Quick Start

The quick-start guide should take a new user from:

```text
Install
  ↓
Launch
  ↓
Create Project
  ↓
Open Repository
  ↓
Index Codebase
  ↓
Select Model
  ↓
Ask Agent
  ↓
Review Changes
  ↓
Run Tests
  ↓
Accept/Reject Changes
```

## 5.3 First Project

Explain:

- Creating a project
- Importing an existing repository
- Project detection
- Project configuration
- Indexing
- Agent permissions
- First coding task

---

# 6. Architecture Documentation

## 6.1 System Architecture

Document:

```text
User
 ↓
Desktop/Web/CLI
 ↓
API / Agent Runtime
 ↓
Manager Agent
 ↓
Planner
 ↓
Context Engine
 ↓
Specialist Agent
 ↓
Tool Layer
 ↓
Sandbox
 ↓
Project
 ↓
Verification
 ↓
User Approval
```

Explain every component and its responsibilities.

## 6.2 Agent Architecture

Document:

- Agent lifecycle
- Task lifecycle
- Planning
- Context retrieval
- Tool calling
- Observation
- Verification
- Retry
- Cancellation
- Approval
- Completion

Core lifecycle:

```text
REQUEST
→ CLASSIFY
→ PLAN
→ RETRIEVE
→ EXECUTE
→ OBSERVE
→ VALIDATE
→ VERIFY
→ APPROVE
→ COMPLETE
```

---

# 7. Agent Documentation

Each agent must have a standard documentation template.

## Standard Agent Template

```markdown
# Agent Name

## Purpose

## Responsibilities

## Non-Responsibilities

## Inputs

## Outputs

## Available Tools

## Permissions

## Context Requirements

## Decision Process

## Failure Handling

## Verification

## Security Considerations

## Configuration

## Examples

## Evaluation

## Known Limitations
```

Required agent documentation:

- Manager Agent
- Coding Agent
- Debugging Agent
- Testing Agent
- Database Agent
- Git Agent
- DevOps Agent

Future agents:

- UI/UX Agent
- Security Agent
- Documentation Agent
- Research Agent
- Performance Agent
- Migration Agent

---

# 8. Tool Documentation

Every tool must document:

- Tool name
- Purpose
- Input schema
- Output schema
- Permissions
- Risk level
- Side effects
- Failure modes
- Timeout
- Sandbox behavior
- Audit logging
- Examples

Example:

```markdown
# Terminal Tool

Risk Level: High

## Purpose
Execute project commands inside a controlled environment.

## Input

{
  "command": "npm test",
  "cwd": "/workspace/project"
}

## Output

{
  "exitCode": 0,
  "stdout": "...",
  "stderr": "..."
}

## Security
- Workspace restriction
- Command policy
- Resource limits
- Timeout
- Audit log
```

---

# 9. Model Documentation

Document:

- Supported model providers
- Local model runtimes
- Cloud providers
- Model selection
- Model routing
- Context limits
- Token budgeting
- Tool calling
- Streaming
- Model fallback
- Error handling
- Cost controls for optional cloud models

The model layer must use a provider abstraction so the agent is not permanently tied to one AI provider.

---

# 10. Code Intelligence Documentation

Document the complete code understanding pipeline:

```text
Repository
 ↓
File Scanner
 ↓
Language Detection
 ↓
Parser
 ↓
AST
 ↓
Symbols
 ↓
Dependencies
 ↓
Chunks
 ↓
Embeddings
 ↓
Indexes
 ↓
Hybrid Retrieval
```

Document:

- Tree-sitter
- AST parsing
- Symbol extraction
- Function/class detection
- Imports
- Dependencies
- Call relationships
- File relationships
- Semantic indexing
- Keyword search
- Symbol search
- Structural search
- Git-history retrieval

---

# 11. Memory Documentation

Memory should be separated into:

```text
Conversation Memory
Project Memory
User Preferences
Technical Memory
Semantic Memory
Task History
```

Document:

- What is stored
- Why it is stored
- Retention
- Deletion
- Privacy
- Retrieval
- Context injection
- Memory conflicts
- User controls

Sensitive information must not be stored unnecessarily.

---

# 12. Security Documentation

Security documentation is mandatory.

Document:

- Threat model
- Permission model
- Tool authorization
- Sandbox
- Secret management
- Credential handling
- Repository isolation
- Prompt injection
- Malicious repository content
- Dependency risks
- Command execution
- Database write protection
- Production deployment protection
- Audit logs
- Incident response

Security principle:

```text
Model reasoning
      ≠
Tool execution
      ≠
Authorization
      ≠
Verification
```

The model must never receive unrestricted authority merely because it generated a valid-looking tool request.

---

# 13. API Documentation

Document every public API.

Minimum API areas:

```text
/api/projects
/api/projects/:id
/api/agents
/api/tasks
/api/tools
/api/models
/api/memory
/api/index
/api/git
/api/database
/api/approvals
/api/events
/api/health
```

For every endpoint document:

- Method
- URL
- Authentication
- Request schema
- Response schema
- Errors
- Permissions
- Example request
- Example response

---

# 14. Development Documentation

Developer documentation must explain:

- Repository structure
- Local development
- Environment variables
- Running services
- Running tests
- Debugging
- Code style
- Type checking
- Linting
- Building
- Packaging
- Adding features

---

# 15. Adding New Agents

Create a guide for developers who want to add an agent.

Required steps:

```text
1. Define responsibility
2. Define input/output
3. Define tools
4. Define permissions
5. Define context requirements
6. Implement agent
7. Register agent
8. Add tests
9. Add evaluation cases
10. Add documentation
```

---

# 16. Adding New Tools

Every tool must go through:

```text
Design
 ↓
Schema
 ↓
Permission Definition
 ↓
Sandbox Definition
 ↓
Implementation
 ↓
Unit Tests
 ↓
Security Tests
 ↓
Integration Tests
 ↓
Documentation
 ↓
Registration
```

High-risk tools require additional security review.

---

# 17. Skills Documentation

Skills provide domain-specific knowledge and workflows.

Initial skills:

- Flutter
- Dart
- React
- TypeScript
- Node.js
- Python
- PostgreSQL
- Firebase
- Supabase
- Docker
- Git
- REST APIs

Each skill should contain:

```text
skill/
├── README.md
├── instructions.md
├── patterns.md
├── examples/
├── templates/
└── tests/
```

---

# 18. Database Documentation

Document:

- Database architecture
- PostgreSQL
- SQLite
- Migrations
- Schema
- Indexes
- Transactions
- Connection management
- Backup
- Restore
- Security
- Agent database permissions

Database operations must clearly distinguish:

```text
READ
WRITE
MIGRATION
DESTRUCTIVE
PRODUCTION
```

---

# 19. Sandbox Documentation

Document:

- Workspace isolation
- Filesystem restrictions
- Network policy
- CPU limits
- Memory limits
- Process limits
- Command restrictions
- Timeouts
- Container lifecycle
- Cleanup
- Resource quotas

Recommended execution hierarchy:

```text
Safe read
 ↓
Safe write
 ↓
Test execution
 ↓
Build
 ↓
Network operation
 ↓
Database mutation
 ↓
Production operation
```

Higher-risk operations require stronger authorization.

---

# 20. Git Documentation

Document:

- Repository detection
- Status
- Diff
- Branches
- Commits
- Checkout
- Merge
- Rebase
- Conflict resolution
- Stash
- Tags
- Remote operations
- Pull requests

Dangerous operations such as force push must require explicit approval.

---

# 21. Testing Documentation

Testing documentation must cover:

```text
Unit Tests
Integration Tests
Agent Tests
Tool Tests
Security Tests
Sandbox Tests
End-to-End Tests
Evaluation Tests
Regression Tests
```

Every new agent/tool should have automated tests.

---

# 22. Evaluation Documentation

The AI agent needs a dedicated evaluation system.

Evaluate:

- Code correctness
- Task completion
- Tool selection
- Context retrieval
- Regression rate
- Test success
- Security behavior
- Permission behavior
- Recovery from failures
- Token efficiency
- Latency

Example evaluation task:

```text
Task:
Fix a failing authentication test.

Expected:
- Locate relevant files
- Identify root cause
- Modify minimal code
- Run tests
- Explain change
- Produce clean diff
```

---

# 23. Deployment Documentation

Document deployment modes:

### Local

```text
Desktop
+
Local Agent Runtime
+
Local Database
+
Local Model
```

### Server

```text
Web UI
+
API
+
Agent Runtime
+
Worker
+
Database
+
Object Storage
```

### Docker

Document:

- Docker build
- Compose
- Volumes
- Networks
- Environment
- Health checks

---

# 24. Configuration Documentation

Document all configuration variables.

Example:

```env
APP_ENV=development

DATABASE_URL=
REDIS_URL=

MODEL_PROVIDER=
MODEL_NAME=

WORKSPACE_ROOT=

ENABLE_DOCKER_SANDBOX=true
ENABLE_BROWSER_TOOLS=false

LOG_LEVEL=info
```

Never commit real secrets.

---

# 25. Contribution Documentation

Open-source contribution documentation should include:

- How to fork
- How to clone
- Development setup
- Branch naming
- Commit conventions
- Pull request rules
- Testing requirements
- Documentation requirements
- Security reporting
- Code review process

---

# 26. Architecture Decision Records

All major technical decisions should use ADRs.

Example:

```text
docs/adr/ADR-0001-local-first-architecture.md
docs/adr/ADR-0002-typescript-agent-runtime.md
docs/adr/ADR-0003-tree-sitter-code-intelligence.md
docs/adr/ADR-0004-sandboxed-tool-execution.md
```

ADR template:

```markdown
# ADR-XXXX: Decision Title

## Status

Proposed / Accepted / Rejected / Superseded

## Context

## Problem

## Options Considered

## Decision

## Consequences

## Security Impact

## Performance Impact

## Alternatives Rejected

## Date
```

---

# 27. Release Documentation

Document:

- Semantic versioning
- Release branches
- Release candidates
- Changelog
- Migration notes
- Breaking changes
- Security releases
- Desktop installers
- Docker images
- Package publishing

Version example:

```text
MAJOR.MINOR.PATCH

1.0.0
1.1.0
1.1.1
```

---

# 28. Documentation Quality Requirements

Documentation should be:

- Accurate
- Current
- Searchable
- Version-controlled
- Example-driven
- Security-aware
- Beginner-friendly
- Developer-friendly

Every documentation page should have:

```text
Title
Purpose
Prerequisites
Main Content
Examples
Security Notes
Troubleshooting
Related Documentation
```

---

# 29. Documentation Testing

Documentation must also be tested.

Automate:

- Broken-link checking
- Code-example validation
- API schema validation
- Configuration validation
- Command validation
- Version consistency
- Screenshot freshness where applicable

CI should fail for critical documentation errors.

---

# 30. Documentation Website

Future public documentation site:

```text
docs.own-ai-agent.example
```

Recommended sections:

```text
Introduction
Getting Started
User Guide
Agent Guide
Tools
Models
Architecture
Security
API
Development
Skills
Deployment
Contributing
Changelog
```

The documentation site should support:

- Full-text search
- Version selection
- Dark mode
- Code copy button
- API examples
- Architecture diagrams
- Search indexing

---

# 31. Documentation Ownership

Suggested ownership:

| Documentation | Owner |
|---|---|
| Product docs | Product/Core |
| Architecture | Core Engineering |
| Agent docs | AI Engineering |
| Tool docs | Platform Engineering |
| Security | Security Engineering |
| API docs | Backend Engineering |
| Deployment | DevOps |
| Skills | Skill Maintainers |
| User docs | Documentation Team |
| Changelog | Release Maintainer |

---

# 32. Documentation Lifecycle

Documentation lifecycle:

```text
Feature Proposal
 ↓
Architecture Decision
 ↓
Implementation
 ↓
Documentation
 ↓
Tests
 ↓
Review
 ↓
Release
 ↓
Maintenance
```

A feature should not be considered complete until its required documentation is updated.

---

# 33. Minimum Documentation Required for MVP

MVP must include:

```text
README.md
docs/README.md

docs/getting-started/installation.md
docs/getting-started/quick-start.md
docs/getting-started/configuration.md

docs/architecture/overview.md
docs/architecture/agent-architecture.md
docs/architecture/tool-architecture.md

docs/agents/manager-agent.md
docs/agents/coding-agent.md

docs/tools/filesystem.md
docs/tools/terminal.md
docs/tools/git.md

docs/security/security-overview.md
docs/security/permissions.md
docs/security/sandbox.md

docs/development/setup.md
docs/development/testing.md
docs/development/repository-structure.md

docs/adr/README.md

CHANGELOG.md
CONTRIBUTING.md
LICENSE
```

---

# 34. Production Documentation Checklist

Before production release:

- [ ] Installation documented
- [ ] Quick start documented
- [ ] Architecture documented
- [ ] Agent behavior documented
- [ ] Tool schemas documented
- [ ] Permissions documented
- [ ] Security model documented
- [ ] Sandbox documented
- [ ] API documented
- [ ] Database documented
- [ ] Configuration documented
- [ ] Environment variables documented
- [ ] Deployment documented
- [ ] Backup/restore documented
- [ ] Troubleshooting documented
- [ ] Contribution guide documented
- [ ] License documented
- [ ] Changelog maintained
- [ ] ADRs created for major decisions
- [ ] Documentation links tested
- [ ] Examples tested

---

# 35. Recommended Final Documentation Ecosystem

The final project should contain:

```text
README.md
requirements.md
LICENSE
CONTRIBUTING.md
SECURITY.md
CHANGELOG.md

docs/
├── getting-started/
├── user-guide/
├── architecture/
├── agents/
├── tools/
├── models/
├── code-intelligence/
├── memory/
├── security/
├── api/
├── database/
├── development/
├── skills/
├── deployment/
├── contributing/
├── releases/
└── adr/
```

---

# 36. Definition of Done

Documentation is complete for a feature when:

- The feature's purpose is documented.
- Configuration is documented.
- Public interfaces are documented.
- Security implications are documented.
- Usage examples exist.
- Failure behavior is documented.
- Tests are documented.
- Related architecture decisions are recorded.
- The documentation has been reviewed.
- Links and examples work.

---

# 37. Final Documentation Strategy

The documentation system should evolve together with the AI agent.

The project should never become a collection of undocumented agents, tools, APIs, or hidden permissions.

The target is:

```text
Code
 +
Architecture
 +
Requirements
 +
Documentation
 +
Tests
 +
Security
 +
Evaluation
```

all maintained as one engineering system.

The documentation itself should be open-source, version-controlled, searchable, and understandable by both contributors and end users.
