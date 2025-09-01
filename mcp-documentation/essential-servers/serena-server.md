# Serena MCP Server Documentation

## Overview
A powerful coding agent toolkit that transforms LLMs into fully-featured coding agents with IDE-like capabilities. Uses Language Server Protocol (LSP) for true semantic code understanding rather than simple text search.

## Key Features
- **Semantic Code Analysis**: Symbol-level understanding using LSP
- **Multi-Language Support**: Python, TypeScript/JavaScript, PHP, Go, Rust, C/C++, Java
- **Free to Use**: Works with Claude's free tier, no API keys required
- **IDE-Level Intelligence**: Refactoring, symbol navigation, cross-file edits
- **Local Operation**: Runs on your machine, code stays private
- **Memory System**: Project-specific memories for context retention

## Installation Methods

### Method 1: Direct Installation (Recommended)
```bash
# Install uv (Python package manager)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Or on macOS
brew install uv

# Or on Windows
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### Method 2: Claude Desktop Configuration
```json
{
  "mcpServers": {
    "serena": {
      "command": "uvx",
      "args": [
        "--from", 
        "git+https://github.com/oraios/serena",
        "serena",
        "start-mcp-server",
        "--context", "ide-assistant",
        "--project", "$(pwd)"
      ]
    }
  }
}
```

### Method 3: Claude Code
```bash
claude mcp add-json "serena" '{"command":"uvx","args":["--from","git+https://github.com/oraios/serena","serena-mcp-server"]}'
```

### Method 4: Docker
```bash
docker run --rm -i --network host \
  -v /path/to/your/projects:/workspaces/projects \
  ghcr.io/oraios/serena:latest \
  serena start-mcp-server --transport stdio
```

### Method 5: Nix Flake
```bash
nix run github:oraios/serena -- start-mcp-server --transport stdio
```

## Language Support

### Direct Support (Tested)
- **Python**: Full support
- **TypeScript/JavaScript**: Full support
- **PHP**: Full support
- **Go**: Requires Go and gopls installed
- **Rust**: Full support
- **C/C++**: Full support
- **Java**: Full support (slow startup on macOS)

### Indirect Support (Via multilspy)
- Ruby
- C#
- Kotlin
- Dart

## Available Tools

### Core Navigation
- `find_symbol` - Find symbols by name across project
- `find_references` - Find all references to a symbol
- `get_call_hierarchy` - Show function call relationships
- `get_type_hierarchy` - Show type inheritance
- `list_symbols_in_file` - List all symbols in a file

### Code Analysis
- `get_hover_info` - Get documentation for symbol
- `get_signature_help` - Get function signatures
- `get_diagnostics` - Get code errors/warnings
- `summarize_changes` - Summarize recent edits

### Code Editing
- `edit_symbol` - Edit symbol across all references
- `refactor_rename` - Rename symbol everywhere
- `apply_quick_fix` - Apply suggested fixes
- `format_file` - Format code properly

### Project Management
- `activate_project` - Initialize project for analysis
- `restart_language_server` - Restart LSP if needed
- `search_for_pattern` - Search code patterns
- `execute_shell_command` - Run shell commands

### Memory System
- `write_memory` - Store project-specific knowledge
- `read_memories` - Retrieve stored knowledge
- `list_memories` - List all memories

### Modes
- `switch_modes` - Change operational modes:
  - `planning` - For architecture planning
  - `editing` - For code modifications
  - `interactive` - For step-by-step work
  - `one-shot` - For single operations

## Dashboard & Monitoring

Serena provides a web dashboard at:
```
http://localhost:24282/dashboard/index.html
```

Features:
- View logs and agent activity
- Monitor language server status
- Shutdown server if client fails to terminate

## Usage Examples

### Project Activation
```
"Activate the current directory as a project using Serena"
```

### Code Analysis
```
"Find all references to the UserService class"
"Show me the call hierarchy for the processPayment function"
```

### Refactoring
```
"Rename the variable 'temp' to 'temporaryBuffer' across all files"
"Extract this method into a separate utility function"
```

### Memory Usage
```
"Remember that we use camelCase for all function names in this project"
"What coding conventions did we establish for this project?"
```

## Key Advantages Over Other Tools

### vs Text-Based RAG Approaches
- **Serena**: Symbol-level understanding, knows relationships between code elements
- **RAG**: Text similarity only, no semantic understanding

### vs Subscription Services
- **Serena**: Free, open-source, runs locally
- **Cursor/Copilot**: Paid subscriptions, cloud-based

### vs Simple File Search
- **Serena**: Understands imports, inheritance, type relationships
- **File Search**: Just text matching, no code intelligence

## Integration Options

1. **MCP Protocol**: Claude Desktop, Claude Code, VSCode, Cursor, IntelliJ
2. **Agno Framework**: Works with any LLM (OpenAI, Google, Anthropic)
3. **MCPO Bridge**: Connect to ChatGPT and non-MCP clients
4. **Custom Integration**: Use as library in your own applications

## Best Practices

1. **Project Activation**: Always activate project first for indexing
2. **Memory Usage**: Store project conventions and decisions
3. **Mode Selection**: Use appropriate mode for task type
4. **Large Projects**: If running out of context, use memory system to continue in new conversation
5. **Path Usage**: Always use absolute paths for reliability

## Troubleshooting

### Common Issues
- **Language server crashes**: Restart with `restart_language_server`
- **Hanging processes**: Check dashboard, terminate manually if needed
- **Path issues**: Always use absolute paths
- **Client termination**: Use dashboard to shutdown if client fails

### Performance Tips
- Java support can be slow to start on macOS
- Large projects may need memory system for context management
- Use appropriate mode to optimize performance

## GitHub Repository
https://github.com/oraios/serena

## Key Differentiators
- **Free Forever**: No subscriptions or API keys
- **True Code Intelligence**: LSP-based, not text search
- **Privacy-First**: Runs locally, code never leaves your machine
- **Extensible**: Small codebase, easy to modify
- **Active Development**: 2.9k+ GitHub stars, regular updates

## Notes
- Dashboard allows manual shutdown if client fails to terminate
- Language servers run in subprocess with asyncio
- MCP stdio transport means server started by client
- Project memories stored in `.serena/memories/` directory
