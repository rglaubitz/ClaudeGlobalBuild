# Filesystem MCP Server Documentation

## Overview
Node.js server implementing Model Context Protocol (MCP) for filesystem operations with secure, configurable access controls.

## Installation & Configuration

### Claude Desktop Configuration
Add to `~/.claude.json`:
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/username/Desktop",
        "/path/to/other/allowed/dir"
      ]
    }
  }
}
```

### VS Code Configuration
Add to `.vscode/mcp.json` or user configuration:
```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "${workspaceFolder}"
      ]
    }
  }
}
```

## Directory Access Control

The server uses a flexible directory access control system:

1. **Command-line Arguments**: Specify allowed directories when starting the server
2. **Dynamic via Roots**: MCP clients that support Roots can dynamically update allowed directories

### Access Control Flow:
- If server starts without command-line arguments AND client doesn't support roots protocol, the server will throw an error
- Roots from client completely replace any server-side allowed directories when provided
- Enables runtime directory updates via roots/list_changed notifications without server restart

## Available Tools

The filesystem server provides the following tools:
- Read file operations
- Write file operations  
- Directory listing
- File search capabilities
- File metadata retrieval
- Safe file operations with configurable access controls

## Security Features

- Configurable directory access controls
- No access outside allowed directories
- Safe operation defaults
- MIT License

## Docker Support

```json
{
  "servers": {
    "filesystem": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "--mount", "type=bind,src=${workspaceFolder},dst=/projects/workspace",
        "mcp/filesystem",
        "/projects"
      ]
    }
  }
}
```

**Note**: Don't use detach option (-d) with Docker - server must run in foreground to communicate with VS Code.

## NPM Package
`@modelcontextprotocol/server-filesystem`

## GitHub Repository
https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem
