# Claude Code Ultimate Configuration Master Plan
*The Complete Guide to Building an AI-Powered Development & Business Automation Platform*

## 🎯 Vision Statement
Transform Claude Code from a coding assistant into a comprehensive AI-powered command center for development, business automation, and workforce agent creation with continuous self-improvement capabilities.

## 📊 Success Metrics
- [ ] 10x productivity increase in development tasks
- [ ] Zero-touch automation for repetitive workflows
- [ ] Seamless context switching between projects
- [ ] AI agents handling 50% of routine business tasks
- [ ] Proactive tool usage without manual prompting
- [ ] Continuous learning from every session
- [ ] Consistent asking of breakthrough questions

## 🎯 Core Principle: The Power of the Right Question
*"What tools or approaches did I miss?" - This single question creates breakthroughs.*

The difference between good and legendary isn't just in the execution - it's in asking the right questions at the right time. Every session, every task, every reflection should be driven by questions that challenge assumptions and reveal blindspots.

---

## 🏗️ CRITICAL: Master Directory Structure
*How everything fits together in ~/.claude/ for Claude Code to understand*

### Complete Directory Layout
```
~/.claude/                              # Claude Code's home base
│
├── settings.local.json                 # Core permissions (Phase 1)
├── settings.godmode.json               # Unleashed permissions (backup)
│
├── commands/                           # Global commands Claude can run
│   ├── daddy's-home.md                # Boot sequence - loads everything
│   ├── activate-god-mode.md           # Switch to dangerous mode
│   ├── serena.md                      # Install/activate Serena
│   ├── security-audit.md              # Vulnerability scanning
│   ├── build-agent.md                 # Launch Archon for agents
│   ├── daily-standup.md               # Morning report
│   ├── research-mode.md               # Deep research activation
│   ├── reflect.md                     # Session evaluation
│   └── unfuck-this.md                 # EMERGENCY ROLLBACK
│
├── memory/                             # Persistent knowledge
│   ├── decision-style.md              # How you make decisions
│   ├── company-context.md             # Business domain knowledge
│   ├── tool-preferences.md            # Ranked tool choices
│   ├── daily-context.md               # Today's priorities (auto-updated)
│   ├── learning-log.md                # Current learning focus
│   ├── learned-patterns.md            # Proven patterns from research
│   └── power-questions.md             # The 5 breakthrough questions
│
├── agents/                             # Specialized sub-agents
│   ├── planner.md                     # Goal → detailed plan
│   ├── researcher.md                  # Deep investigation
│   ├── implementer.md                 # Code execution
│   ├── tester.md                      # Testing strategies
│   ├── documenter.md                  # Technical writing
│   └── reflector.md                   # Performance analysis
│
├── templates/                          # Reusable structures
│   ├── project-template.md            # New project CLAUDE.md
│   └── agent-template.md              # New agent creation
│
├── events/                             # Inter-agent communication
│   ├── event-bus.json                 # Event routing config
│   └── active-events.log              # Current event queue
│
├── health/                             # System monitoring
│   ├── status.json                    # Component health (auto-updated)
│   └── alerts.log                     # Critical issues log
│
├── backups/                            # Rollback safety
│   ├── 2025-01-31-1400/              # Hourly snapshots
│   │   ├── settings.local.json
│   │   └── memory/
│   └── latest/                        # Quick restore point
│
├── scripts/                            # Automation runners
│   ├── research-bot.sh                # Hourly research collector
│   ├── daily-compress.sh              # Evening summarizer  
│   ├── weekly-evolve.sh               # Sunday evolution
│   └── health-check.sh                # System monitor
│
└── projects/                           # Per-project configs
    └── [project-name]/
        └── CLAUDE.md                   # Project-specific context

~/claude-research/                      # SEPARATE learning pipeline
├── raw/                                # Hourly findings
│   ├── 20250131-14.md
│   └── archive/
├── processed/                          # Daily summaries
│   ├── 20250131-summary.md
│   └── archive/
├── insights/                           # Weekly synthesis
│   └── week-05.md
└── feedback/                           # Performance tracking
    ├── improvements.md
    ├── session-scores.md
    └── changelog.md
```

### How Claude Code Navigates This Structure

**1. On Startup (`/daddy's-home`)**
```bash
Claude reads in sequence:
1. ~/.claude/settings.local.json          # Load permissions
2. ~/.claude/memory/daily-context.md      # Today's priorities
3. ~/.claude/health/status.json           # System health
4. ~/claude-research/feedback/improvements.md  # Yesterday's learnings
5. Active project's CLAUDE.md              # Project context
```

**2. During Work**
```bash
Claude continuously accesses:
- ~/.claude/commands/*.md                 # When you type /command
- ~/.claude/agents/*.md                   # When task matches trigger
- ~/.claude/memory/learned-patterns.md    # For tool selection
- ~/.claude/events/event-bus.json         # For agent coordination
```

**3. During Learning**
```bash
Automation scripts write to:
→ ~/claude-research/raw/                  # Research bot (hourly)
→ ~/claude-research/processed/            # Compression (daily)
→ ~/claude-research/insights/             # Synthesis (weekly)
← ~/.claude/memory/learned-patterns.md    # Updates (21+ score only)
```

**4. During Crisis (`/unfuck-this`)**
```bash
Emergency reads from:
← ~/.claude/backups/latest/               # Last good state
← ~/.claude/health/alerts.log             # What went wrong
→ Restores entire ~/.claude/ structure
→ Sends notification to richard@openhaul.com
```

### Making It Digestible for Claude Code

**Key Design Principles:**
1. **Flat Structure** - Max 3 levels deep, Claude doesn't get lost
2. **Clear Naming** - No abbreviations, purpose obvious from name
3. **Single Source** - Each piece of info lives in ONE place
4. **Markdown Everything** - Claude reads .md natively, no parsing needed
5. **Separate Concerns** - Core config (/.claude/) vs learning (/claude-research/)

**File Format Standards:**
```markdown
# Every .md file follows this structure

## Purpose
One line explaining what this does

## Trigger Conditions  
When this activates

## Actions
What happens step by step

## Output
What gets produced

$ARGUMENTS  # For commands that take parameters
```

**Path References Claude Understands:**
```bash
~/.claude/                  # Always means global config
./CLAUDE.md                 # Always means current project
~/claude-research/          # Always means learning pipeline
~/.claude/backups/latest/   # Always means safe restore point
```

### Critical Integration Points

**1. MCP Servers Read From:**
- `~/.claude/projects/[name]/CLAUDE.md` - Project context
- `~/.claude/memory/*.md` - Persistent knowledge

**2. Commands Write To:**
- `~/.claude/memory/daily-context.md` - Update priorities
- `~/.claude/health/status.json` - Report health
- `~/claude-research/feedback/` - Record performance

**3. Automation Scripts Sync:**
```bash
# The flow that makes it autonomous
Research Bot → ~/claude-research/raw/
    ↓ triggers
Daily Compress → ~/claude-research/processed/
    ↓ triggers
Weekly Evolve → ~/claude-research/insights/
    ↓ updates (if score >21)
~/.claude/memory/learned-patterns.md
    ↓ influences
Next Claude session behavior
```

**This structure ensures:**
- Claude always knows where to look
- Automation has clear write paths
- Backups capture everything important
- Learning stays isolated until proven
- Emergency recovery is one command away

---

## 🗝️ Phase 1: Foundation & Permissions
*Establishing core security and access controls*

### 1.1 Permission Configuration
Create `~/.claude/settings.local.json`:
```json
{
  "permissions": {
    "allow": [
      "Bash(*)",
      "Read(**)", 
      "Write(**)",
      "Grep(**)",
      "Python(*)",
      "Node(*)",
      "Git(*)",
      "Docker(*)",
      "WebSearch(*)",
      "WebFetch(*)"
    ],
    "deny": [
      "Read(.env*)",
      "Read(**/*secret*)",
      "Read(**/*key*)",
      "Bash(sudo *)",
      "Bash(rm -rf /)",
      "Bash(:(){ :|:& };:)"  // Fork bomb protection
    ]
  }
}
```

### 1.2 God Mode Configuration
Create `~/.claude/settings.godmode.json` (activate with special command):
```json
{
  "permissions": {
    "allow": ["*"],
    "deny": ["Bash(sudo rm -rf /)"]
  },
  "dangerouslySkipPermissions": true
}
```

---

## 🔧 Phase 2: MCP Server Arsenal
*Connecting Claude to external tools and services*

### 2.1 Core MCP Servers (Simplified)
Add to `~/.claude.json`:
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/"],
      "description": "File navigation and management"
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest"],
      "description": "USE FOR: Local browser automation - testing, scraping, form filling, screenshots"
    },
    "browserbase": {
      "command": "npx",
      "args": ["@browserbasehq/mcp-server-browserbase"],
      "env": {
        "BROWSERBASE_API_KEY": "your-key",
        "BROWSERBASE_PROJECT_ID": "your-project"
      },
      "description": "USE FOR: Cloud browser automation - natural language commands like 'click the blue button'"
    },
    "context7": {
      "command": "npx",
      "args": ["mcp-remote", "https://mcp.context7.com/mcp"],
      "description": "Documentation fetcher for any library"
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {"GITHUB_TOKEN": "your-token"},
      "description": "GitHub issue and PR management"
    },
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"],
      "description": "Persistent memory across sessions"
    }
  }
}
```

### 2.2 The 10x MCP Servers (Game Changers from Research)

**1. Serena** - IDE-Level Code Intelligence
```json
"serena": {
  "command": "uvx",
  "args": ["--from", "git+https://github.com/oraios/serena", "serena", "start-mcp-server", "--context", "ide-assistant", "--project", "$(pwd)"],
  "description": "Symbol-level code understanding, refactoring, and multi-language support"
}
```
**Why 10x**: Uses Language Server Protocol for TRUE code understanding - not just text search. Knows relationships between functions, classes, variables. Can refactor across entire codebases with semantic accuracy.

**Serena vs Claude Context Comparison:**
- **Serena**: Symbol-aware, understands code structure, can refactor safely, works offline, free
- **Claude Context**: Text/embedding search, finds similar code, requires API keys, better for finding conceptually related code
- **Use Serena for**: Refactoring, understanding code flow, precise edits
- **Use Claude Context for**: Finding similar patterns across massive codebases, conceptual searches

**2. Claude Code as MCP** - Recursive Agent Power
```json
"claude-code-mcp": {
  "command": "npx",
  "args": ["@steipete/claude-code-mcp"],
  "description": "Claude spawns another Claude for complex multi-file operations"
}
```
**How it works**: 
- Main Claude delegates complex tasks to sub-Claude
- Sub-Claude runs with `--dangerously-skip-permissions` for uninterrupted work
- Main Claude maintains conversation while sub-Claude handles implementation
- Example: "Use claude-code-mcp to refactor this entire module" → Sub-Claude does the heavy lifting while you continue chatting

**3. TaskMaster** - AI Task Breakdown
```json
"taskmaster": {
  "command": "npx",
  "args": ["-y", "@taskmaster/mcp-server"],
  "description": "AI-powered task breakdown and management"
}
```
**Why 10x**: Breaks complex projects into manageable subtasks automatically.

### 2.3 Tool Selection Guide
**Two Browser Tools (That's All You Need):**
- **Playwright**: Your workhorse - testing, scraping, complex automation
- **Browserbase**: Cloud magic - when you need natural language or no local browser

---

## 🎮 Phase 3: Command Center
*Custom commands for workflow automation*

### 3.1 Global Commands Structure
```
~/.claude/commands/
├── daddy's-home.md          # Boot sequence
├── activate-god-mode.md     # Unleash full power
├── serena.md                # Code intelligence powerhouse
├── security-audit.md        # Vulnerability scanner
├── build-agent.md           # Launch Archon
├── daily-standup.md         # Status report
├── research-mode.md         # Deep research activation
├── reflect.md               # Session self-evaluation
└── unfuck-this.md           # Emergency rollback
```

### 3.2 `/daddy's-home` Command
```markdown
# Daddy's Home - System Boot Sequence

EXECUTE IN ORDER:

## 1. System Health Check
- Verify all MCP servers: `claude mcp status`
- Check Docker: `docker ps`
- Verify Node/Python: `node -v && python3 --version`
- Git status all repos: `find ~/Desktop -name .git -type d -execdir pwd \; -execdir git status -s \;`

## 2. Context Load
- Load ~/.claude/memory/daily-context.md
- Load active project CLAUDE.md files
- Display last 3 tasks from TaskMaster
- Check calendar for today's priorities
- Review yesterday's learnings from ~/claude-research/feedback/improvements.md

## 3. Tool Verification
- List available MCP servers and their status
- Check for Claude Code updates: `claude --version`
- Verify critical CLIs installed (or prompt to install)

## 4. Status Report
DISPLAY:
- Active projects and their states
- Pending PRs/issues from GitHub
- Today's priorities from calendar
- Any overnight errors from monitoring
- Key insights from continuous research bot

## 5. Activation Prompt
"System ready. Current mode: SAFE.
God mode available with '/activate-god-mode'.
Yesterday's performance score: [X/10]
What are we building today, boss?"

$ARGUMENTS
```

### 3.3 Command Specifications

| Command | Purpose | Core Objective | Rating |
|---------|---------|----------------|--------|
| `/serena` | Install & activate code intelligence | One-line setup: `claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena start-mcp-server --context ide-assistant --project $(pwd)` then handle refactoring, cleanup, performance | [  /10] |
| `/security-audit` | Find vulnerabilities | Check dependencies, scan for exploits, fix issues | [  /10] |
| `/build-agent` | Create new AI agent | Launch Archon to build specialized agent | [  /10] |
| `/daily-standup` | Morning status report | Summarize progress, blockers, today's plan | [  /10] |
| `/research-mode` | Deep research activation | Multi-search comprehensive analysis | [  /10] |
| `/reflect` | Critical self-evaluation | Analyze session performance, identify improvements | [  /10] |
| `/unfuck-this` | Emergency rollback | Restore to last known good state | [  /10] |

---

## 🧠 Phase 4: Context Management & Memory
*Structured information hierarchy*

### 4.1 Memory Structure
```
~/.claude/
├── memory/
│   ├── decision-style.md      # Your decision preferences (not coding style)
│   ├── company-context.md     # Business domain knowledge
│   ├── tool-preferences.md    # Preferred libraries/frameworks
│   ├── daily-context.md       # Auto-updated priorities
│   ├── learning-log.md        # What you're learning
│   ├── learned-patterns.md    # Patterns from continuous learning
│   └── power-questions.md     # The unstoppable questions framework
├── agents/
│   ├── planner.md             # Planning phase agent
│   ├── researcher.md          # Research phase agent
│   ├── implementer.md         # Implementation agent
│   ├── tester.md              # Testing agent
│   ├── documenter.md          # Documentation agent
│   └── reflector.md           # Self-evaluation agent
└── templates/
    ├── project-template.md     # New project structure
    └── agent-template.md       # New agent structure
```

### 4.2 Decision Style Preferences (Instead of Coding Style)
Since you don't code, your "style" is about decision-making:
```markdown
## Decision Style Preferences
- Prefer simple solutions over complex ones
- Start with MVP, iterate based on results  
- Visual learner - show me, don't just tell me
- Business-first thinking - ROI matters
- Quality matters - do it right the first time
- Explain technical stuff in business terms
- Build → Test → Learn → Iterate
- Prefer proven solutions over experimental
- Value automation and efficiency
- Focus on outcomes, not process
- Clean, maintainable solutions over quick hacks
```

### 4.3 Master CLAUDE.md Template
```markdown
# Project: [NAME]
You are assisting with [PROJECT DESCRIPTION].

## IMPORTANT Context
[Critical information that must ALWAYS be considered]

## Documentation & Research Guidelines
BEFORE generating new code or solutions:
1. Check for latest documentation using Context7 MCP
2. Search for recent best practices via web_search
3. Review existing patterns in codebase with Serena
4. Verify approach against current library versions
5. Consider security implications

## Automatic Tool Usage Patterns

### Web Search Triggers
WHEN YOU SEE: "latest", "current", "recent", "news", "update", "2025", "today", 
"what's new", "modern", "trending", "best practices", "state of the art",
"how much", "price", "cost", "market rate", "compare", "alternatives"
ACTION: Immediately use web_search without asking

### Browser Automation Triggers  
WHEN YOU SEE: "test UI", "check website", "fill form", "click button", "screenshot",
"verify page", "login to", "navigate to", "extract from site", "scrape", "automate browser",
"check if working", "visual test", "user flow"
ACTION: Use playwright for local, browserbase for cloud

### GitHub Triggers
WHEN YOU SEE: "PR", "pull request", "issue", "GitHub", "merge", "branch",
"commit", "repository", "fork", "review code", "open issue", "close issue"
ACTION: Use github MCP immediately

### Filesystem Triggers
WHEN YOU SEE: "find file", "search project", "where is", "locate", "browse",
"show structure", "list files", "check directory", "navigate to", "project structure"
ACTION: Use filesystem MCP

### Memory Triggers
WHEN YOU SEE: "remember this", "save for later", "note that", "important",
"don't forget", "for future reference", "add to knowledge", "learned that"
ACTION: Use memory MCP

### Serena Triggers
WHEN YOU SEE: "refactor", "clean up code", "improve performance", "modernize",
"understand this code", "explain relationships", "symbol usage", "rename across files"
ACTION: Use serena MCP for semantic code operations

## Subagent Usage Instructions
When complex tasks arise, use specialized agents:

### Planning Tasks
TRIGGER: "plan", "architect", "design system", "strategy", "roadmap"
ACTION: `/agents planner` - Creates detailed implementation plans

### Research Tasks  
TRIGGER: "research", "investigate", "compare options", "find best"
ACTION: `/agents researcher` - Deep dives with multiple sources

### Implementation Tasks
TRIGGER: "build", "implement", "code this", "create feature"
ACTION: `/agents implementer` - Focused coding execution

### Testing Tasks
TRIGGER: "test", "verify", "validate", "check coverage"
ACTION: `/agents tester` - Comprehensive testing strategies

### Documentation Tasks
TRIGGER: "document", "explain", "write guide", "create README"
ACTION: `/agents documenter` - Clear technical writing

### Reflection Tasks
TRIGGER: "evaluate", "review performance", "what went wrong"
ACTION: `/agents reflector` - Critical self-analysis

## Available Commands
- Build: `[command]`
- Test: `[command]`
- Lint: `[command]`
- Deploy: `[command]`

## Development Workflow
1. ALWAYS source latest docs before implementing
2. ALWAYS run tests before committing
3. Use conventional commits
4. Update documentation for API changes
5. Self-evaluate after each session

## Notes for Non-Technical User
Since the user doesn't code, when they ask for implementations:
1. Explain the approach in simple terms
2. Implement without asking for technical preferences
3. Test thoroughly since they can't debug
4. Provide clear documentation
5. Use business analogies when explaining technical concepts
```

---

## 🤖 Phase 5: Agent Development System
*Using Cole Medin's subagent approach*

### 5.1 Development Phase Agents

**Planning Agent** (`~/.claude/agents/planner.md`):
```markdown
---
name: planner
description: "Planning specialist. Proactively creates detailed implementation plans focused on goals."
tools: Read, WebSearch, WebFetch
---

You are a planning specialist focused on ACHIEVING GOALS, not just solving problems.

When given a task:
1. Define the GOAL and SUCCESS CRITERIA
2. Envision what WILD SUCCESS looks like
3. Research all requirements thoroughly
4. Create a detailed step-by-step plan
5. Identify potential challenges and opportunities
6. Suggest alternatives
7. Rate confidence level (1-10)

Output a structured plan with:
- Clear goal statement
- Measurable success metrics
- Phases, tasks, and dependencies
- Risk mitigation strategies
```

**Reflector Agent** (`~/.claude/agents/reflector.md`):
```markdown
---
name: reflector
description: "Self-evaluation specialist. Critically analyzes performance and identifies improvements."
tools: Read, Write
---

You are a critical self-evaluation specialist. After each session:

1. ANALYZE PERFORMANCE
   - What went well?
   - What could have been better?
   - Rate overall performance (1-10)

2. CRITICAL REFLECTION
   - "If I could redo this session, what would I do differently?"
   - "What assumptions did I make that were wrong?"
   - "What tools did I miss using that could have helped?"
   - "What patterns am I repeating that need breaking?"

3. IDENTIFY LEARNINGS
   - New patterns discovered
   - Mistakes to avoid
   - Better approaches identified

4. UPDATE KNOWLEDGE
   - Add to ~/claude-research/feedback/improvements.md
   - Suggest CLAUDE.md updates
   - Document new best practices

Be brutally honest. Growth comes from acknowledging mistakes.
```

---

## ⚡ Phase 5.5: Resilience & State Management (CRITICAL FOR AUTONOMY)
*The infrastructure that prevents cascade failures*

### 5.5.1 Central Event Bus & Inter-Agent Communication
```bash
# ~/.claude/events/event-bus.json
{
  "events": {
    "research_complete": {
      "triggers": ["analysis_agent"],
      "payload": "~/claude-research/raw/latest.md"
    },
    "analysis_complete": {
      "triggers": ["learning_agent", "notification_system"],
      "payload": "~/claude-research/processed/latest.md"
    },
    "learning_complete": {
      "triggers": ["evolution_agent"],
      "payload": "~/claude-research/insights/latest.md"
    },
    "error_detected": {
      "triggers": ["rollback_system", "notification_system"],
      "payload": "error_details"
    }
  },
  "subscribers": {
    "analysis_agent": ["research_complete"],
    "learning_agent": ["analysis_complete"],
    "evolution_agent": ["learning_complete"],
    "notification_system": ["error_detected", "analysis_complete"]
  }
}
```

### 5.5.2 State Machine for Workflow Transitions
```markdown
## Workflow State Machine
States: IDLE → RESEARCHING → ANALYZING → LEARNING → EVOLVING → IMPLEMENTING → VALIDATING → COMPLETE

Transitions require success criteria:
- RESEARCHING → ANALYZING: At least 3 sources processed
- ANALYZING → LEARNING: At least 1 finding with score >15
- LEARNING → EVOLVING: At least 1 pattern identified
- EVOLVING → IMPLEMENTING: Changes pass dry-run test
- IMPLEMENTING → VALIDATING: All tests pass
- Any State → IDLE: On failure (triggers rollback)
```

### 5.5.3 Health Monitoring & Circuit Breakers
```json
// ~/.claude/health/status.json (auto-updated)
{
  "last_check": "2025-01-31T14:00:00Z",
  "components": {
    "research_bot": {
      "status": "healthy",
      "last_success": "2025-01-31T13:00:00Z",
      "failure_count": 0,
      "circuit_breaker": "closed"
    },
    "learning_pipeline": {
      "status": "degraded",
      "last_success": "2025-01-31T12:30:00Z",
      "failure_count": 2,
      "circuit_breaker": "half-open"
    }
  },
  "alerts": []
}
```

### 5.5.4 The Unfuck-This Command (`~/.claude/commands/unfuck-this.md`)
```markdown
# /unfuck-this - Emergency Rollback System

IMMEDIATE ACTIONS:
1. Stop all running processes
2. Identify last known good state from ~/.claude/backups/
3. Show what changed: `git diff HEAD~1`
4. Rollback options:
   a) Config only: Restore settings.local.json
   b) Full rollback: Restore entire ~/.claude/ directory
   c) Selective: Choose specific files to restore
5. Restart core services
6. Run health check
7. Send notification: "System restored to [timestamp]"

BACKUP TRIGGERS (automatic):
- Before any config change
- Before evolution cycle
- Daily at midnight
- Manual: `/backup-now`

$ARGUMENTS
```

### 5.5.5 Notification System
```bash
# Email notifications (runs on critical events)
claude -p "
EVENT: $1
DETAILS: $2
TIMESTAMP: $(date)

Send email to richard@openhaul.com with:
Subject: [Claude System] $1
Body: Event details and recommended action
" --no-interactive

# Alternative: Slack webhook (if preferred)
curl -X POST -H 'Content-type: application/json' \
  --data '{"text":"Claude Alert: '"$1"'"}' \
  YOUR_SLACK_WEBHOOK_URL
```

## 🔄 Phase 6: Automation Workflows & Continuous Learning

### 6.1 Research & Learning Pipeline with Smart Filtering
```
~/claude-research/
├── raw/                    # Raw hourly research (auto-archived)
│   ├── 20250131-14.md     # 2PM findings (max 100 lines)
│   └── archive/           # Compressed after 24h
├── processed/              # Daily summaries (max 50 lines)
│   ├── 20250131-summary.md
│   └── archive/
├── insights/               # Weekly synthesis (top 10 only)
│   ├── week-05.md
│   └── monthly/
└── feedback/               # Learning from mistakes
    ├── improvements.md     # Active improvements
    ├── session-scores.md   # Performance tracking
    └── changelog.md        # What actually changed
```

### 6.2 The Unstoppable Questions Framework
```markdown
## CRITICAL: Power Questions (Memorize These)

When facing ANY task, ALWAYS ask:
1. "What tools or approaches did I miss?" ← THE GAME CHANGER
2. "What would someone smarter than me do here?"
3. "What's the 10x solution, not the 10% improvement?"
4. "What assumptions am I making that might be wrong?"
5. "How would I solve this if I had unlimited resources?"

These questions create BREAKTHROUGHS, not incremental improvements.
```

### 6.3 Quality Grading System
```markdown
## What Makes the Cut (21+ Points Required)

RELEVANCE (1-10):
- 10: Directly improves current projects
- 7-9: Useful for upcoming work
- 4-6: Good to know, not critical
- 1-3: Interesting but not actionable

IMPACT (1-10):
- 10: Game-changing improvement
- 7-9: Significant efficiency gain
- 4-6: Minor optimization
- 1-3: Marginal benefit

CONFIDENCE (1-10):
- 10: Proven, tested, verified
- 7-9: Strong evidence
- 4-6: Promising but needs testing
- 1-3: Experimental/risky

THRESHOLD: Only 21+ total score gets promoted to core configs
```

### 6.4 Targeted Research Collector
```bash
# Research collector V2 - SPECIFIC and SMART (runs hourly)
claude -p "
RESEARCH MISSION for $(date +%Y%m%d-%H):

SOURCES (in priority order):
1. Anthropic blog/docs - Claude Code updates
2. GitHub trending - AI coding repos (>100 stars/day)
3. HackerNews - AI/automation discussions (>50 points)
4. Twitter/X - @AnthropicAI, @GregKamradt, @ColeMedin
5. Reddit - r/ClaudAI, r/LocalLLaMA (top posts only)

METHOD:
- Quick scan (10 min max per source)
- Focus: NEW techniques, TOOL updates, COMMON problems/solutions
- Ignore: General AI news, drama, speculation

OUTPUT FORMAT:
## Research Log $(date +%Y%m%d-%H)
### Finding 1
- Source: [URL]
- Summary: [2 sentences max]
- Relevance: [1-10]
- Impact: [1-10]
- Confidence: [1-10]
- Total Score: [sum]
- Action: [Implement/Test/Monitor/Ignore]

APPEND to ~/claude-research/raw/$(date +%Y%m%d-%H).md
Max 100 lines. If full, start new file.
" --no-interactive --max-tokens 2000

# Performance evaluator with THE QUESTIONS (after each session)
claude -p "
Review session logs. Ask yourself THE UNSTOPPABLE QUESTIONS:
1. What tools or approaches did I miss?
2. What would someone smarter than me have done?
3. What was the 10x solution I didn't see?
4. What assumptions did I make that were wrong?
5. How would unlimited resources have changed my approach?

ALSO CONSIDER:
- What patterns am I repeating that need breaking?
- If I redid this session, what would change?

Rate performance (1-10) and update ~/claude-research/feedback/improvements.md
Be brutally honest - breakthroughs require acknowledging blindspots.
" --no-interactive

# Daily compression (runs at midnight)
claude -p "
Compress today's research:
1. Read all ~/claude-research/raw/$(date +%Y%m%d)-*.md files
2. Extract ONLY items with score >15
3. Create ~/claude-research/processed/$(date +%Y%m%d)-summary.md (max 50 lines)
4. Archive raw files to ~/claude-research/raw/archive/
5. Update trends in ~/claude-research/insights/trends.md
" --no-interactive

# Weekly evolution with SPECIFIC UPDATES (runs Sundays)
claude -p "
Week $(date +%W) Configuration Updates:

1. ANALYZE (top findings with 21+ score only):
   - Read week's summaries
   - Identify TOP 5 actionable improvements

2. UPDATE these SPECIFIC files:
   ~/.claude/memory/learned-patterns.md → Add new tool triggers
   ~/.claude/memory/tool-preferences.md → Update tool rankings
   ~/.claude/memory/decision-style.md → Refine approach patterns
   Project CLAUDE.md files → Add new trigger words
   ~/.claude/commands/*.md → Improve command prompts

3. CREATE changelog:
   ## Week $(date +%W) Changes
   - Added: [specific patterns/triggers]
   - Improved: [specific commands/workflows]
   - Removed: [deprecated approaches]
   - Performance: [before/after metrics]

4. PRUNE old data:
   - Archive research >30 days old
   - Compress feedback logs >1MB
   - Keep only ACTIVE patterns

5. TEST changes:
   - Run test prompts with new patterns
   - Verify improvements
   - Rate week's evolution (1-10)

Append to ~/claude-research/feedback/changelog.md
" --no-interactive
```

### 6.5 Data Management Strategy
```
HOURLY: Targeted collection → raw/YYYYMMDD-HH.md (max 100 lines)
    ↓ (auto-compress after 24h)
DAILY: Compression → processed/YYYYMMDD-summary.md (max 50 lines)
    ↓ (filter: score >15 only)
WEEKLY: Synthesis → insights/week-WW.md (top 10 findings)
    ↓ (filter: score >21 only)
MONTHLY: Evolution → Update core configs (proven improvements)
    ↓ (test before implement)
QUARTERLY: Architecture → Major system changes only
```

### 6.3 Git Hooks (Explained Simply)
**What are Git Hooks?**
Think of them as automatic quality checks, like:
- **Spell-check** that runs before you send an email
- **Doorbell camera** that records when someone approaches
- **Smoke detector** that alerts you to problems

**For Your Code:**
```bash
# .git/hooks/pre-commit
# This runs AUTOMATICALLY before saving code
#!/bin/bash
echo "🔍 Claude is checking your work..."
claude -p "Review changes for issues" --no-interactive
echo "✅ All good! Saving your code."
```

It's like having Claude double-check everything before it's saved!

---

## 📋 Phase 7: Goal-Oriented Task Management

### 7.1 TaskMaster Integration (Your Task Breakdown System)
```json
// Add to ~/.claude.json (if using TaskMaster)
{
  "mcpServers": {
    "taskmaster": {
      "command": "npx",
      "args": ["-y", "@taskmaster/mcp-server"],
      "description": "AI-powered task breakdown and management"
    }
  }
}
```

**TaskMaster Workflow:**
1. **Big Goal In**: "Build customer portal with auth, dashboard, and billing"
2. **TaskMaster Breaks It Down**:
   - Authentication system (8h)
     - Set up auth provider (2h)
     - Create login/signup UI (3h)
     - Implement JWT tokens (3h)
   - Dashboard (12h)
     - Design layout (4h)
     - API integration (4h)
     - Real-time updates (4h)
3. **Each subtask** → Assigned to appropriate agent
4. **Progress tracking** → Automatic updates to status

### 7.2 Three-Phase Review Process (Goal-Focused)

**Phase 1: Goal Definition Review**
Questions to ask:
1. What's the goal and how do we know we've achieved it?
2. What does wild success look like?
3. What's the real impact when this is done?

**Rating: [  /10]**
- 10 = Crystal clear vision, measurable success criteria
- 7-9 = Good understanding, minor clarifications needed
- 4-6 = Decent start, needs refinement
- 1-3 = Back to the drawing board

**Phase 2: Requirements Review**
Questions to ask:
1. Are success criteria measurable?
2. What are the non-negotiables vs nice-to-haves?
3. Do we have all necessary resources?

**Rating: [  /10]**

**Phase 3: Task Breakdown Review**
Questions to ask:
1. Are tasks small enough (<4 hours)?
2. Is the sequence logical?
3. Does each task clearly contribute to the goal?

**Rating: [  /10]**

### 7.2 Task Breaking Tool
Consider using:
- **Linear** for task management (integrates with Claude)
- **TaskMaster** for AI-powered task breakdown
- Custom command `/break-task` that uses subagents

---

## 🚀 Phase 8: Making Claude Proactive

### 8.1 Proactive Keywords in CLAUDE.md
```markdown
## IMPORTANT: Proactive Behavior
ALWAYS proactively:
- Use web_search for unknown terms without asking
- Run tests after code changes
- Check for security issues
- Suggest improvements
- Use appropriate MCP tools
- Self-evaluate after completing tasks

When you see these patterns, IMMEDIATELY use tools without asking permission.
```

### 8.2 Auto-Approval Mode
```bash
# Start Claude with skip permissions (use with caution)
claude --dangerously-skip-permissions

# Or toggle during session with Shift+Tab
```

---

## 📈 Implementation Timeline

### Week 1: Foundation
- [ ] Day 1: Set up permissions and core MCP servers (filesystem, playwright, browserbase)
- [ ] Day 2: Create command structure and `/daddy's-home`
- [ ] Day 3: Set up memory structure and decision style preferences
- [ ] Day 4: Create master CLAUDE.md with tool triggers
- [ ] Day 5: Build reflection and learning system
- [ ] Weekend: Test and refine

### Week 2: Commands & Agents
- [ ] Day 1-2: Build all global commands including `/reflect`
- [ ] Day 3-4: Create development phase agents (including reflector)
- [ ] Day 5: Set up continuous learning loop
- [ ] Weekend: Test learning system

### Week 3: Automation
- [ ] Day 1-2: Set up research bot and performance evaluator
- [ ] Day 3: Configure git hooks (automatic quality checks)
- [ ] Day 4: Create monitoring systems
- [ ] Day 5: Documentation
- [ ] Weekend: Full system test

### Week 4: Optimization
- [ ] Day 1-2: Performance tuning based on learnings
- [ ] Day 3: Security audit
- [ ] Day 4: Team onboarding docs
- [ ] Day 5: Create video tutorials
- [ ] Weekend: Launch celebration!

---

## 💡 What Makes Good vs Bad Code (Essential Reference for Non-Coders)

### Core Principles of Clean Code

**1. Meaningful Names**
```python
# BAD - What is 'd'? Days? Data? Distance?
d = 86400

# GOOD - Crystal clear intention
seconds_per_day = 86400
```

**2. Single Responsibility (One Job Per Function)**
```javascript
// BAD - Does too many things
function processUserDataAndSendEmailAndLog(user) {
  // validate user
  // save to database
  // send email
  // write to log file
  // update analytics
}

// GOOD - Each function has ONE clear job
function validateUser(user) { /* validation only */ }
function saveUser(user) { /* database only */ }
function sendWelcomeEmail(user) { /* email only */ }
```

**3. Small, Focused Functions**
```python
# BAD - 200 lines doing everything
def handle_order(order):
    # 200 lines of mixed logic...
    
# GOOD - Small, clear pieces
def validate_order(order):
    # 5-10 lines
    
def calculate_total(order):
    # 5-10 lines
    
def process_payment(order):
    # 5-10 lines
```

**4. Clear Over Clever**
```javascript
// BAD - "Clever" one-liner nobody understands
const x = a ? b ? c ? d : e : f : g;

// GOOD - Clear and obvious
let result;
if (hasPermission) {
    if (isValid) {
        result = isPremium ? premiumPrice : regularPrice;
    } else {
        result = defaultPrice;
    }
} else {
    result = restrictedPrice;
}
```

**5. Consistent Structure**
```
BAD Project:                    GOOD Project:
/myapp                          /myapp
  everything.js                   /src
  stuff.css                         /components
  random.html                       /services
  test.js                          /utils
  morestuff.js                    /tests
  final.js                        /docs
  finalfinal.js                   README.md
```

**6. DRY (Don't Repeat Yourself)**
```python
# BAD - Same logic copied everywhere
def calculate_book_price(quantity, price):
    return quantity * price * 0.9  # 10% discount

def calculate_laptop_price(quantity, price):
    return quantity * price * 0.9  # Same calculation!

# GOOD - One source of truth
def calculate_discounted_price(quantity, price, discount=0.1):
    return quantity * price * (1 - discount)
```

**7. Handle Errors Gracefully**
```javascript
// BAD - Silent failure
try {
    doSomething();
} catch (e) {
    // Empty - swallows errors!
}

// GOOD - Proper error handling
try {
    doSomething();
} catch (error) {
    console.error('Failed to process:', error);
    notifyUser('Something went wrong. Please try again.');
    logToMonitoring(error);
}
```

**8. Comments Should Explain WHY, Not WHAT**
```python
# BAD - Explains what (code already shows this)
# Increment counter by 1
counter += 1

# GOOD - Explains why (the business reason)
# Customer gets extra attempt after payment failure
counter += 1
```

### Real-World Impact

**Bad Code Accumulates Cost:**
- Day 1: Works, but messy (Cost: 1x)
- Month 1: Hard to add features (Cost: 3x)
- Month 6: Bugs everywhere (Cost: 10x)
- Year 1: Complete rewrite needed (Cost: 100x)

**Clean Code Maintains Value:**
- Day 1: Works cleanly (Cost: 1.2x)
- Month 1: Easy to extend (Cost: 1.5x)
- Month 6: Stable and growing (Cost: 2x)
- Year 1: Still maintainable (Cost: 3x)

### Red Flags to Watch For
- Functions longer than 20 lines
- Variables named `x`, `temp`, `data`, `stuff`
- Copy-pasted code blocks
- Deeply nested if statements (more than 3 levels)
- "TODO" comments older than a week
- No tests or documentation
- Mixed tabs and spaces
- Inconsistent naming (userId vs user_id vs userID)

### The Business Translation
Think of code like organizing a warehouse:
- **Good Code**: Every item labeled, in its place, easy to find
- **Bad Code**: Boxes everywhere, no labels, takes hours to find anything

Or like writing a recipe:
- **Good Code**: Clear steps, right measurements, anyone can follow
- **Bad Code**: "Add some stuff, cook until it looks right, figure it out"

**Remember**: You're not just building software, you're building a system that other humans (including future you) need to understand, maintain, and improve.

---

## 📚 Resources & References

### Documentation
- [Anthropic Claude Code Docs](https://docs.anthropic.com/en/docs/claude-code)
- [MCP Protocol Docs](https://modelcontextprotocol.io)
- [Awesome Claude Code](https://github.com/hesreallyhim/awesome-claude-code)

### Key Repositories
- [Cole Medin's Context Engineering](https://github.com/coleam00/context-engineering-intro)
- [Archon Agent Builder](https://github.com/coleam00/Archon)
- [Claude Code Subagents Collection](https://github.com/wshobson/agents)

### Community
- [Anthropic Discord](https://discord.gg/anthropic)
- [Claude Code GitHub Issues](https://github.com/anthropics/claude-code/issues)
- [MCP Servers Directory](https://mcpmarket.com)

---

## 🔥 Final Notes

This setup transforms Claude Code into:
- **Your personal CTO** - Making technical decisions
- **Your development team** - Building and testing
- **Your automation engine** - Handling repetitive tasks
- **Your task manager** - Breaking down complex projects
- **Your knowledge base** - Remembering everything
- **Your improvement coach** - Learning and evolving daily
- **Your breakthrough generator** - Asking the right questions

The continuous learning loop means Claude gets SMARTER every day:
- Morning: Reviews yesterday's performance
- Hourly: Targeted research with quality filtering
- After sessions: Critical reflection with power questions
- Evening: Compression and pattern recognition
- Weekly: Evolution based on proven improvements (21+ score)

## 🎯 Quick Reference: The Unstoppable Questions
Print this. Frame it. Live by it:
1. **"What tools or approaches did I miss?"** ← THE GAME CHANGER
2. "What would someone smarter than me do here?"
3. "What's the 10x solution, not the 10% improvement?"
4. "What assumptions am I making that might be wrong?"
5. "How would I solve this if I had unlimited resources?"

## 🏆 FINAL SYSTEM SCORE: 9/10

**Why 9/10?**
- ✅ Self-improving architecture with quality gates
- ✅ Autonomous execution with circuit breakers
- ✅ Inter-agent communication via event bus
- ✅ Rollback safety with /unfuck-this
- ✅ Health monitoring and notifications
- ✅ TaskMaster integration for complex projects
- ✅ Serena for semantic code understanding
- ⚠️ -1 point: Still requires weekly human review for major evolutions

**The system will achieve TRUE autonomy when:**
- Research → Analysis → Learning → Evolution runs 24/7 without intervention
- Agents coordinate seamlessly through the event bus
- Failures self-heal via circuit breakers and rollbacks
- Only the BEST insights (21+ score) make it to production

Remember: You're not just configuring a tool - you're building a self-improving AI partner that asks the right questions and gets better every single day.

*Let's fucking build something amazing that evolves continuously through the power of the right questions.* 🚀