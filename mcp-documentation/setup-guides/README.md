# MCP Documentation Master Index

## Overview
Complete documentation collection for Model Context Protocol (MCP) servers needed for Claude Code Ultimate Configuration and autonomous AI system setup.

## Directory Structure
```
mcp-documentation/
├── core-protocol/
│   └── mcp-overview.md              # MCP fundamentals and architecture
├── essential-servers/
│   ├── filesystem-server.md         # File operations and navigation
│   ├── playwright-server.md         # Browser automation (local)
│   ├── browserbase-server.md        # Browser automation (cloud)
│   ├── serena-server.md            # Code intelligence with LSP
│   ├── context7-server.md          # Documentation fetching
│   ├── github-server.md            # GitHub repository management
│   └── memory-server.md            # Knowledge graph persistence
├── advanced-servers/
│   └── claude-code-mcp-server.md   # Recursive agent spawning
└── setup-guides/
    └── README.md                    # This file
```

## Quick Setup Guide

### 1. Core MCP Protocol Setup

MCP is the foundation that enables Claude to connect to external tools and data sources.

**Key Concepts:**
- **Client-Server Architecture**: Claude acts as client, MCP servers provide tools
- **JSON-RPC Protocol**: Standardized communication format
- **Transport Methods**: stdio (local), SSE/HTTP (remote)

### 2. Priority Installation Order

#### Phase 1: Foundation (Essential for all setups)
1. **Filesystem** - File operations and navigation
2. **Memory** - Persistent knowledge across sessions
3. **Context7** - Up-to-date documentation

#### Phase 2: Development Tools
4. **Serena** - IDE-level code intelligence (FREE alternative to Cursor)
5. **GitHub** - Repository and issue management
6. **Playwright** OR **Browserbase** - Choose based on needs:
   - Playwright: Local browser, CSS selectors
   - Browserbase: Cloud browser, natural language

#### Phase 3: Advanced Automation
7. **Claude-Code-MCP** - Recursive agents for complex tasks

### 3. Configuration Template

Create `~/.claude.json` (Claude Desktop) or appropriate config file:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/"]
    },
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    },
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp@latest"]
    },
    "serena": {
      "command": "uvx",
      "args": [
        "--from", "git+https://github.com/oraios/serena",
        "serena", "start-mcp-server",
        "--context", "ide-assistant",
        "--project", "$(pwd)"
      ]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your-token"
      }
    },
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    },
    "claude-code-mcp": {
      "command": "npx",
      "args": ["-y", "@steipete/claude-code-mcp@latest"]
    }
  }
}
```

## Server Comparison Matrix

| Server | Purpose | Cost | Setup Complexity | Key Advantage |
|--------|---------|------|------------------|---------------|
| **Filesystem** | File operations | Free | Simple | Core functionality |
| **Memory** | Persistent context | Free | Simple | Cross-session memory |
| **Context7** | Current docs | Free | Simple | No more outdated APIs |
| **Serena** | Code intelligence | Free | Moderate | LSP-based understanding |
| **GitHub** | Repository mgmt | Free | Simple | Direct GitHub integration |
| **Playwright** | Browser testing | Free | Simple | Local automation |
| **Browserbase** | Cloud browser | API Key | Moderate | Natural language |
| **Claude-Code-MCP** | Recursive agents | Free | Moderate | Complex automation |

## Critical Setup Notes

### For Serena (Code Intelligence)
1. Install uv first: `curl -LsSf https://astral.sh/uv/install.sh | sh`
2. Dashboard available at: http://localhost:24282/dashboard/index.html
3. Activate project: "Activate the current directory as a project using Serena"

### For GitHub
1. Create Personal Access Token with required permissions
2. Add to environment variables in config

### For Claude-Code-MCP
1. First-time setup required: `claude --dangerously-skip-permissions`
2. Always provide CWD in prompts: "Your work folder is /path/to/project"

### For Context7
1. Just add "use context7" to any prompt needing current docs
2. Optional API key for higher rate limits

## Common Use Patterns

### Pattern 1: Full Stack Development
- Serena + Context7 + GitHub + Filesystem
- Perfect for: Building applications with current frameworks

### Pattern 2: Web Automation & Testing
- Playwright + Filesystem + Memory
- Perfect for: E2E testing, web scraping

### Pattern 3: Documentation & Learning
- Context7 + Memory + Filesystem
- Perfect for: Learning new libraries, documentation tasks

### Pattern 4: Complex Refactoring
- Serena + Claude-Code-MCP + GitHub
- Perfect for: Large-scale code changes

## Troubleshooting

### Common Issues

**"Connection closed" errors**
- Windows: Use `cmd /c` wrapper for npx commands
- Check server is installed: `npx @package/name --version`

**Permission denied**
- Check file system permissions
- Run first-time setup for Claude-Code-MCP
- Verify API tokens are set

**Server not responding**
- Check dashboard/logs if available
- Restart Claude Desktop
- Verify JSON syntax in config

**Context7 not working**
- Ensure Node.js >= 18
- Try direct path if npx fails

## Resources

### Official Documentation
- MCP Protocol: https://modelcontextprotocol.io
- Anthropic Docs: https://docs.anthropic.com/en/docs/mcp

### GitHub Repositories
- MCP Servers: https://github.com/modelcontextprotocol/servers
- Awesome MCP: https://github.com/wong2/awesome-mcp-servers

### Community
- Anthropic Discord
- GitHub Discussions

## Next Steps

1. Install servers based on your needs
2. Test each server individually
3. Build your workflows
4. Customize with your own servers

## Notes
- All TypeScript servers work with `npx`
- Python servers use `uvx` or `pip`
- Docker alternatives available for most servers
- MCP support in VSCode, Cursor, Windsurf, and more

---
*Documentation compiled for Claude Code Ultimate Configuration build*
*Last updated: January 2025*
