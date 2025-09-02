# Browserbase MCP Server Documentation

## Overview
Cloud browser automation capabilities using Browserbase and Stagehand. Enables LLMs to interact with web pages, take screenshots, extract information, and perform automated actions with atomic precision.

## Key Features
- **Natural Language Control**: Use plain English commands like "click the login button"
- **Cloud-Based**: Runs on Browserbase infrastructure, no local browser needed
- **AI-Powered**: Built on Stagehand for intelligent element detection
- **Stealth Mode**: Advanced stealth options for bot detection avoidance
- **Proxy Support**: Built-in proxy configuration
- **Multi-Session**: Support for parallel browser sessions

## Installation

### Claude Desktop Configuration
```json
{
  "mcpServers": {
    "browserbase": {
      "command": "npx",
      "args": ["@browserbasehq/mcp-server-browserbase"],
      "env": {
        "BROWSERBASE_API_KEY": "your-api-key",
        "BROWSERBASE_PROJECT_ID": "your-project-id",
        "GEMINI_API_KEY": "your-gemini-key"
      }
    }
  }
}
```

### Remote Hosted Version (Simplest)
```json
{
  "mcpServers": {
    "browserbase": {
      "command": "npx",
      "args": ["mcp-remote", "your-smithery-url.com"]
    }
  }
}
```

### Local Installation
```bash
git clone https://github.com/browserbase/mcp-server-browserbase.git
cd mcp-server-browserbase
pnpm install && pnpm build
```

Then configure:
```json
{
  "mcpServers": {
    "browserbase": {
      "command": "node",
      "args": ["/path/to/mcp-server-browserbase/cli.js"],
      "env": {
        "BROWSERBASE_API_KEY": "",
        "BROWSERBASE_PROJECT_ID": "",
        "GEMINI_API_KEY": ""
      }
    }
  }
}
```

## Configuration Options

### Browser Dimensions
```json
{
  "args": [
    "@browserbasehq/mcp-server-browserbase",
    "--browserHeight", "1080",
    "--browserWidth", "1920"
  ]
}
```

### Proxy Configuration
```json
{
  "args": ["@browserbasehq/mcp-server-browserbase", "--proxies"]
}
```

### Advanced Stealth
Enable advanced stealth mode to avoid bot detection.

### Custom AI Models
Default uses Gemini 2.0 Flash, but supports other models:

```json
{
  "args": [
    "@browserbasehq/mcp-server-browserbase",
    "--modelName", "anthropic/claude-3-5-sonnet-latest",
    "--modelApiKey", "your-anthropic-api-key"
  ]
}
```

## Available Tools

### Session Management (Multi-Session)
- `multi_browserbase_stagehand_session_create` - Create parallel browser sessions
- `multi_browserbase_stagehand_session_list` - Track active sessions
- `multi_browserbase_stagehand_session_close` - Clean up sessions
- `multi_browserbase_stagehand_navigate_session` - Navigate in specific session

### Browser Control
- Navigate to URLs
- Click elements using natural language
- Fill forms
- Extract data
- Take screenshots
- Execute JavaScript
- Monitor console logs

## Key Differences from Playwright

| Feature | Playwright | Browserbase |
|---------|-----------|-------------|
| **Infrastructure** | Local browser | Cloud browser |
| **Language** | CSS selectors | Natural language |
| **Setup** | Install browsers locally | API key only |
| **Resource Usage** | Local CPU/RAM | Cloud resources |
| **Stealth** | Basic | Advanced anti-detection |

## Use Cases

1. **Cloud Testing**: No local browser installation needed
2. **Natural Language Automation**: "Click the blue submit button"
3. **Data Extraction**: Scrape websites with AI understanding
4. **Form Automation**: Fill complex forms intelligently
5. **Parallel Sessions**: Run multiple browser sessions simultaneously

## Requirements
- Browserbase API key
- Browserbase Project ID
- Gemini API key (or other model API key)

## GitHub Repository
https://github.com/browserbase/mcp-server-browserbase

## Documentation
https://docs.browserbase.com/integrations/mcp/introduction

## NPM Package
`@browserbasehq/mcp-server-browserbase`

## Notes
- Currently, only latest Anthropic models handle multiple tools reliably
- Stagehand provides the AI-powered element detection
- Screenshots available via `screenshot://<screenshot-name>` resource
