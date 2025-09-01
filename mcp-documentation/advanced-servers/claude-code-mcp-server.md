# Claude-Code-MCP Server Documentation

## Overview
An MCP server that allows running Claude Code in one-shot mode with permissions bypassed automatically. Enables Claude to spawn another Claude instance for complex multi-file operations, essentially providing "an agent in your agent."

## Key Features
- **Recursive Agent Spawning**: Main Claude delegates complex tasks to sub-Claude
- **Permission Bypass**: Uses `--dangerously-skip-permissions` for uninterrupted work
- **Multi-File Operations**: Handle complex edits across multiple files
- **Maintained Conversation**: Main Claude continues while sub-Claude works

## Installation

### NPM Package
```bash
npm install -g @steipete/claude-code-mcp
```

### Claude Desktop Configuration
```json
{
  "mcpServers": {
    "claude-code-mcp": {
      "command": "npx",
      "args": ["-y", "@steipete/claude-code-mcp@latest"],
      "env": {
        "CLAUDE_CLI_NAME": "claude"
      }
    }
  }
}
```

### Cursor Configuration
```json
{
  "mcpServers": {
    "claude-code-mcp": {
      "command": "npx",
      "args": ["-y", "@steipete/claude-code-mcp@latest"]
    }
  }
}
```

### Claude Code CLI
```bash
claude mcp add-json "claude-code-mcp" '{"command":"npx","args":["-y","@steipete/claude-code-mcp@latest"]}'
```

## First-Time Setup

**CRITICAL**: Before using the MCP server, you must run Claude CLI manually once:
```bash
claude --dangerously-skip-permissions
```
Follow prompts to accept terms. This one-time requirement enables the MCP server to use the flag non-interactively afterward.

Note: macOS might request folder permissions on first run.

## Environment Variables

```json
{
  "env": {
    "CLAUDE_CLI_NAME": "claude",      // Override CLI binary name or path
    "MCP_CLAUDE_DEBUG": "true"         // Enable debug logging
  }
}
```

## Available Tool

### claude_code
The unified tool that executes prompts directly using Claude Code CLI with permissions bypassed.

**Parameters:**
- `prompt`: The instruction to execute
- `cwd`: Current working directory (critical for file operations)

## Usage Examples

### File Creation
```
"Your work folder is /Users/username/my_project

Create a new file named 'config.yml' in the 'app/settings' directory with the following content:
port: 8080
database: main_db"
```

### File Editing
```
"Your work folder is /Users/username/my_project

Edit file 'public/css/style.css': Add a new CSS rule at the end to make all 'h2' elements have 'color: navy'."
```

### Multi-Step Operations
```
"Your work folder is /Users/username/my_project

Move the file 'report.docx' from the 'drafts' folder to 'final_reports' folder and rename it to 'Q1_Report_Final.docx'."
```

### Complex Refactoring
```
"Your work folder is /Users/username/my_project

Refactor the entire authentication module to use JWT tokens instead of sessions. Update all related files."
```

### Git Operations
```
"Your work folder is /Users/username/my_project

Check the status of CI checks for Pull Request #42 in the GitHub repository 'owner/repo'. Report if they have passed, failed, or are still running."
```

### Release Preparation
```
"Your work folder is /Users/username/my_project

Prepare a release:
1. Create a release branch
2. Update package.json version
3. Update CHANGELOG.md
4. Commit changes
5. Create pull request"
```

## How It Works

1. **Main Claude** receives complex task
2. **Delegates** to sub-Claude via claude_code tool
3. **Sub-Claude** executes with --dangerously-skip-permissions
4. **Works autonomously** without permission interruptions
5. **Returns results** to main Claude
6. **Main Claude** continues conversation

## Key Advantages

### vs Direct Editing
- **Claude/Windsurf** often struggle with file editing
- **Claude Code** is better and faster at it
- **Multiple commands** can be queued

### Context Efficiency
- Saves context space
- More important information retained
- Fewer compacts happen

### Autonomy
- No permission interruptions
- Complete complex tasks unattended
- Handle multi-file operations seamlessly

## Best Practices

### Always Provide CWD
**CRITICAL**: Include working directory in prompts:
```
"Your work folder is /path/to/project

[Your actual command...]"
```

### Structured Prompts
Break complex tasks into clear steps:
```
"Your work folder is /path/to/project

1. First do X
2. Then do Y
3. Finally do Z"
```

### Use for Complex Tasks
Ideal for:
- Multi-file refactoring
- Project setup
- Release preparation
- Test generation
- Documentation updates
- Git workflow automation

## Troubleshooting

### Common Issues

**"Command not found" (claude-code-mcp)**
- Ensure npm global bin directory is in PATH
- If using npx, verify npx is working

**"Command not found" (claude)**
- Ensure Claude CLI is installed correctly
- Run `claude doctor` to check

**Permission Issues**
- Make sure you've run first-time setup
- Check file system permissions

**JSON Errors**
- If MCP_CLAUDE_DEBUG is true, logs might interfere
- Disable debug mode for production

**macOS Folder Permissions**
- First run may fail
- Grant permissions when prompted
- Subsequent runs should work

## Integration Examples

### With Other MCP Servers
```json
{
  "mcpServers": {
    "claude-code-mcp": {...},
    "filesystem": {...},
    "github": {...}
  }
}
```

### In Automation Workflows
```bash
# Use in scripts
echo "Your work folder is $(pwd)
Refactor all TypeScript files to use async/await" | \
npx @steipete/claude-code-mcp
```

## Enhanced Version
There's an enhanced fork with additional features:
- Advanced task orchestration
- Robust error handling
- "Boomerang pattern" for subtasks
- Better reliability

GitHub: `grahama1970/claude-code-mcp-enhanced`

## GitHub Repository
https://github.com/steipete/claude-code-mcp

## NPM Package
`@steipete/claude-code-mcp`

## Notes
- Executes in context of user running server
- No built-in authentication beyond file system
- Tool calls execute without confirmation by default
- Client should implement own confirmation mechanism
- Consider security implications of file system access
