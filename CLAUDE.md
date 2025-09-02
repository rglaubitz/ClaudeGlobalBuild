# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is the Claude Code Global Project - a configuration and documentation repository for building an autonomous AI system that transforms Claude Code into a self-improving intelligence platform. The repository focuses on MCP (Model Context Protocol) server documentation, Claude Code configuration, and multi-agent system architecture.

## Repository Structure

```
ClaudeGlobalProject/
├── .claude/                          # Claude configuration directory
│   ├── CLAUDE.md                    # Project-specific Claude guidelines
│   ├── settings.local.json          # Permission settings
│   └── commands/                    # Custom Claude commands
│       ├── generate-prp.md          # PRP generation command
│       └── execute-prp.md           # PRP execution command
├── mcp-documentation/                # MCP server documentation
│   ├── core-protocol/               # MCP fundamentals
│   ├── essential-servers/           # Core MCP servers
│   ├── advanced-servers/            # Advanced MCP capabilities
│   └── setup-guides/                # Installation guides
├── claude-code-ai-agent-prd.md      # Product Requirements Document
└── claude-code-ultimate-config.md   # Ultimate configuration guide
```

## High-Level Architecture

### Core System Components

1. **Command Center (~/.claude/)** - The global Claude configuration that persists across all projects:
   - Permissions & Security
   - Global Commands
   - Memory & Context
   - Agent Orchestration
   - Health Monitoring

2. **Learning Pipeline (~/claude-research/)** - Continuous improvement system:
   - Research Collection
   - Analysis & Scoring
   - Pattern Recognition
   - Evolution Engine

3. **Multi-Agent System** - Specialized agents for different tasks:
   - **Planner**: Creates detailed implementation plans
   - **Researcher**: Deep investigation with multiple sources
   - **Implementer**: Code generation and execution
   - **Tester**: Comprehensive testing strategies
   - **Documenter**: Technical documentation
   - **Reflector**: Performance analysis and learning

### MCP Server Integration

The system leverages Model Context Protocol servers for extended capabilities:
- **Filesystem**: File operations and navigation
- **Memory**: Persistent knowledge across sessions
- **Serena**: Semantic code understanding and refactoring
- **Context7**: Documentation fetching
- **GitHub**: Repository and issue management
- **Playwright/Browserbase**: Browser automation

## Working with This Repository

### Task Management

- Check `.claude/TASK.md` for understanding of Task-management in the(though currently empty)
- Tasks should be tracked with clear descriptions and dates
- Mark tasks complete immediately after finishing

### PRP (Project Requirements Plan) System

This repository uses a PRP system for complex feature implementation:

1. **Generate PRP**: Use the `generate-prp` command to create comprehensive implementation plans
2. **Execute PRP**: Use the `execute-prp` command to implement the plan
3. **Validation Gates**: All PRPs include executable validation steps

### Configuration Management

The repository maintains both:
- **settings.local.json**: Safe, restricted permissions
- **settings.godmode.json**: Full permissions (use with caution)

## Key Principles

### Autonomous Operation
- System designed for 24/7 operation
- Self-improving through continuous learning
- Event-driven communication between agents

### Quality Gates
- Only patterns scoring 21+ (out of 30) get promoted
- 100% of commands have rollback capability
- All agents pass acceptance criteria before activation

### The Power of Questions
Core breakthrough questions that drive improvement:
- "What tools or approaches did I miss?"
- "What patterns can be extracted from this?"
- "How can this be automated?"
- "What would make this 10x better?"

## Documentation Resources

### Internal Documentation
- **claude-code-ai-agent-prd.md**: Complete PRD for the autonomous AI system
- **claude-code-ultimate-config.md**: Step-by-step configuration guide
- **mcp-documentation/**: Comprehensive MCP server documentation

### MCP Server Priority
1. **Phase 1 (Foundation)**: Filesystem, Memory, Context7
2. **Phase 2 (Development)**: Serena, GitHub, Playwright/Browserbase
3. **Phase 3 (Advanced)**: Claude Code MCP Server for recursive agents

## Development Practices

### Python Development
- Follow PEP8 standards
- Use type hints and format with `black`
- Use `pydantic` for data validation
- Write Google-style docstrings for all functions
- Keep files under 500 lines

### Testing
- Create Pytest unit tests for new features
- Include expected use, edge cases, and failure cases
- Tests live in `/tests` folder mirroring app structure

### Documentation
- Comment non-obvious code with `# Reason:` explanations
- Update documentation when features change
- Ensure code is understandable to mid-level developers

## Success Metrics

The system aims for:
- 10x productivity increase in development tasks
- 90% reduction in repetitive workflow time
- 50% decrease in bug discovery time
- 24/7 autonomous operation with <1% error rate
- 5x faster project completion through intelligent task breakdown