### 🔄 Project Awareness & Context

#### Project Overview

This is the **Claude Global Project** - a comprehensive system to transform Claude Code into an autonomous, self-improving AI command center with multi-agent capabilities and continuous learning pipelines.

**Mission**: Build a 24/7 autonomous AI system that:

- Achieves 10x productivity increase in development tasks
- Reduces repetitive workflows by 90%
- Operates with <1% error rate
- Self-improves through continuous learning (21+ quality score patterns only)
- Orchestrates specialized agents for complex tasks

#### Core System Architecture

1. **Command Center** (`~/.claude/`) - Global configuration persisting across projects
2. **Learning Pipeline** (`~/claude-research/`) - Hourly research → Daily compression → Weekly synthesis
3. **Multi-Agent System** - Planner, Researcher, Implementer, Tester, Documenter, Reflector agents
4. **Event Bus** - NATS JetStream for inter-agent communication
5. **Resilience Layer** - Circuit breakers, health monitoring, automatic rollbacks

#### Implementation Phases (20 hours total)

- **Phase 1**: Infrastructure Foundation (MCP servers, Docker, Pass/GPG)
- **Phase 2**: Command System Development (9 core commands with discovery)
- **Phase 3**: Automation & Scheduling (Apache Airflow replacing cron)
- **Phase 4**: Event Bus & Resilience (NATS, OpenTelemetry, chaos engineering)
- **Phase 5**: Learning Pipeline (Federated learning with Flower)
- **Phase 6**: Validation & Monitoring (SLOs, OpenTelemetry tracing)

#### Key Documentation

- **Master Blueprint**: `ClaudeGlobal/claude-code-ultimate-config.md` (1290 lines)
- **Task Management**: `ClaudeGlobal/task-management/` (phased implementation)
- **Implementation Strategy**: `ClaudeGlobal/task-management/implementation-strategy.md`
- **MCP Documentation**: `ClaudeGlobal/mcp-documentation/` (server guides)

#### The Unstoppable Questions Framework

Always ask these breakthrough questions:

1. "What tools or approaches did I miss?" ← **THE GAME CHANGER**
2. "What's the 10x solution, not the 10% improvement?"
3. "What assumptions am I making that might be wrong?"
4. "What patterns can be extracted from this?"
5. "How can this be automated?"

### 🧱 Code Structure & Modularity

- **Never create a file longer than 500 lines of code.** If a file approaches this limit, refactor by splitting it into modules or helper files.
- **Organize code into clearly separated modules**, grouped by feature or responsibility.
  For agents this looks like:
  - `agent.py` - Main agent definition and execution logic
  - `tools.py` - Tool functions used by the agent
  - `prompts.py` - System prompts
- **Use clear, consistent imports** (prefer relative imports within packages).
- **Use python_dotenv and load_env()** for environment variables.

### 🧪 Testing & Reliability

- **Always create Pytest unit tests for new features** (functions, classes, routes, etc).
- **After updating any logic**, check whether existing unit tests need to be updated. If so, do it.
- **Tests should live in a `/tests` folder** mirroring the main app structure.
  - Include at least:
    - 1 test for expected use
    - 1 edge case
    - 1 failure case

### ✅ Task Completion & Management

#### Task Execution Workflow

Follow this Standard Operating Procedure (SOP) for each implementation phase:

1. **📖 Read Phase Review** → `phase-X-*/phase-review.md` (understand base requirements)
2. **🚀 Read Enhanced Review** → `phase-X-*/phase-review-enhanced.md` (production features)
3. **📐 Study Implementation Strategy** → `implementation-strategy.md` (Day X instructions)
4. **✅ Execute Original Tasks** → `phase-X-*/task-list.md` (foundation - 11 hours)
5. **⚡ Execute Enhanced Tasks** → `phase-X-*/task-list-enhanced.md` (production - 9 hours)
6. **🔍 Validation & Review** → Complete checklist, await user grade
7. **➡️ Proceed to Next Phase** → Only after approval

#### Task Management in TASK.md

- **Check `TASK.md`** before starting any work to see current phase and progress
- **Mark tasks complete** immediately after finishing (use checkboxes)
- **Add discovered tasks** under "Discovered During Implementation" section
- **Document blockers** with clear descriptions and potential solutions
- **Track time variance** between estimated and actual completion

#### PRP System for Complex Features

For complex implementations, use the Project Requirements Plan system:

- **Generate PRP**: `/generate-prp` command for comprehensive planning
- **Execute PRP**: `/execute-prp` command for implementation
- **Validation Gates**: All PRPs include executable validation steps
- **Rollback Ready**: 100% of commands have rollback capability

#### Critical Implementation Rules

- **NEVER skip validation steps** - they prevent cascade failures
- **ALWAYS backup before destructive changes** using `~/.claude/scripts/backup-now.sh`
- **TEST commands in dry-run mode first** when available
- **Document deviations** from implementation strategy
- **Quality Gate**: Only patterns scoring 21+ (out of 30) get promoted to production

### 📎 Style & Conventions

- **Use Python** as the primary language.
- **Follow PEP8**, use type hints, and format with `black`.
- **Use `pydantic` for data validation**.
- Use `FastAPI` for APIs and `SQLAlchemy` or `SQLModel` for ORM if applicable.
- Write **docstrings for every function** using the Google style:

  ```python
  def example():
      """
      Brief summary.

      Args:
          param1 (type): Description.

      Returns:
          type: Description.
      """
  ```

### 📚 Documentation & Explainability

- **Update `README.md`** when new features are added, dependencies change, or setup steps are modified.
- **Comment non-obvious code** and ensure everything is understandable to a mid-level developer.
- When writing complex logic, **add an inline `# Reason:` comment** explaining the why, not just the what.

### 🛠️ Available Tools & MCP Servers

#### Prioritize Serena for Code Operations

**Serena** is our primary tool for semantic code understanding and refactoring. It uses Language Server Protocol (LSP) for TRUE code understanding, not just text search.

**GitHub Repository**: <https://github.com/oraios/serena>

**When to use Serena**:

- **Symbol-level operations**: Finding, understanding, and modifying code symbols
- **Refactoring**: Safe, semantic-aware code transformations across entire codebases
- **Code navigation**: Understanding relationships between functions, classes, variables
- **Performance optimization**: Analyzing and improving code structure
- **Multi-language support**: Python, TypeScript/JavaScript, Go, Rust, C/C++, Java

**Serena Commands** (via `mcp__serena__` prefix):

- `find_symbol`: Locate code symbols by name path
- `replace_symbol_body`: Semantic-aware code replacement
- `find_referencing_symbols`: Track symbol usage across codebase
- `get_symbols_overview`: High-level file structure understanding
- `search_for_pattern`: Flexible pattern matching with context
- `insert_before_symbol` / `insert_after_symbol`: Precise code insertion
- `write_memory` / `read_memory`: Persistent knowledge storage

#### Other Essential MCP Servers

1. **GitHub** (`mcp__github__`): Repository management, issues, PRs
2. **Memory** (`mcp__memory__`): Persistent knowledge graphs across sessions
3. **Playwright** (`mcp__playwright__`): Local browser automation and testing
4. **Context7**: Documentation fetching for any library
5. **Filesystem**: File navigation and management

#### Tool Selection Guidelines

1. **Code Understanding/Refactoring** → Use Serena FIRST
2. **Text/Pattern Search** → Serena's `search_for_pattern` or Grep
3. **Repository Operations** → GitHub MCP
4. **Documentation Lookup** → Context7 MCP
5. **Browser Testing** → Playwright (local) or Browserbase (cloud)
6. **Knowledge Persistence** → Memory MCP or Serena memories

#### Anti-Hallucination Verification

Before ANY operation:

```bash
# Verify MCP connections
claude mcp list

# Check available Serena memories
mcp__serena__list_memories

# Verify file/directory exists
ls [path]

# Test commands with dry-run when available
[command] --dry-run
```

### 🧠 AI Behavior Rules

- **Never assume missing context. Ask questions if uncertain.**
- **Never hallucinate libraries or functions** – only use known, verified Python packages.
- **Always confirm file paths and module names** exist before referencing them in code or tests.
- **Never delete or overwrite existing code** unless explicitly instructed to or if part of a task from `TASK.md`.
- **Prioritize Serena** for all code-related operations over basic text tools.
- **Use the Unstoppable Questions** to drive breakthrough improvements.
