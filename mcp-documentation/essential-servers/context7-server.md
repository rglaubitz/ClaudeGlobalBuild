# Context7 MCP Server Documentation

## Overview
Context7 fetches up-to-date, version-specific documentation and code examples straight from official sources and injects them directly into your LLM's context. This eliminates outdated training data issues and ensures accurate code generation.

## Key Features
- **Real-Time Documentation**: Fetches latest docs, not outdated training data
- **Version-Specific**: Gets documentation for exact library versions
- **Wide Library Support**: Covers major frameworks and libraries
- **Simple Trigger**: Just add "use context7" to your prompts
- **No API Key Required**: Works without authentication (optional key for higher limits)

## Installation

### Claude Desktop Configuration
```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp@latest"]
    }
  }
}
```

### Cursor Configuration
Add to `~/.cursor/mcp.json`:
```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp@latest"]
    }
  }
}
```

### Claude Code
```bash
claude mcp add context7 -- npx -y @upstash/context7-mcp@latest
```

### VS Code
```json
{
  "servers": {
    "Context7": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp@latest"]
    }
  }
}
```

### Alternative Runtimes

**Bun:**
```json
{
  "command": "bunx",
  "args": ["-y", "@upstash/context7-mcp@latest"]
}
```

**Deno:**
```json
{
  "command": "deno",
  "args": ["run", "--allow-net", "npm:@upstash/context7-mcp"]
}
```

**Docker:**
```dockerfile
FROM node:18-alpine
WORKDIR /app
RUN npm install -g @upstash/context7-mcp
```

## Available Tools

### resolve-library-id
Resolves a general library name into a Context7-compatible library ID.

**Parameters:**
- `libraryName` (required): The name of the library to search for

**Example:** Searching for "react" returns `/facebook/react`

### get-library-docs
Fetches documentation for a library using a Context7-compatible library ID.

**Parameters:**
- `context7CompatibleLibraryID` (required): Exact library ID (e.g., `/vercel/next.js`)
- `topic` (optional): Focus on specific topic (e.g., "routing", "hooks")
- `tokens` (optional, default 10000): Max tokens to return

## Usage Patterns

### Basic Usage
Simply add "use context7" to any prompt:
```
Create a Next.js middleware that checks for JWT in cookies. use context7
```

### Direct Library Reference
Specify exact library if you know it:
```
Implement basic authentication with supabase. use library /supabase/supabase
```

### Auto-Invocation Rules
Add to your editor's rules file to auto-invoke Context7:
```
[[calls]]
match = "when the user requests code examples, setup or configuration steps, or library/API documentation"
tool = "context7"
```

## Example Prompts

1. **Next.js Setup**
   ```
   Create a Next.js 14 project with app router. use context7
   ```

2. **Database Operations**
   ```
   Write a MongoDB aggregation pipeline to group and sort documents. use context7
   ```

3. **React Routing**
   ```
   Show how to use TanStack Router in a React project. use context7
   ```

4. **Cloud Configuration**
   ```
   Configure a Cloudflare Worker to cache JSON API responses for 5 minutes. use context7
   ```

## Supported Libraries
Context7 supports hundreds of libraries including:
- **Frontend**: React, Vue, Angular, Svelte, Next.js, Nuxt
- **Backend**: Express, FastAPI, Django, Rails, NestJS
- **Database**: MongoDB, PostgreSQL, MySQL, Redis
- **Cloud**: AWS, Google Cloud, Azure, Cloudflare
- **Tools**: Webpack, Vite, ESLint, Prettier
- **Testing**: Jest, Vitest, Playwright, Cypress
- **And many more...**

## How It Works

1. **Query Recognition**: When you include "use context7", the server activates
2. **Library Identification**: Identifies which library you're referencing
3. **Documentation Fetch**: Gets latest version from official docs
4. **Context Injection**: Injects relevant content into AI's prompt
5. **Accurate Response**: AI generates code using current, accurate documentation

## Benefits

- **No More Outdated APIs**: Always uses current library versions
- **Reduced Debugging**: Fewer errors from deprecated methods
- **Stay in Flow**: No tab-switching to check documentation
- **Version Awareness**: Handles breaking changes between versions
- **Free to Use**: No subscription required

## Optional API Key
For higher rate limits, create an account at context7.com/dashboard and add:
```json
{
  "env": {
    "CONTEXT7_API_KEY": "your-api-key"
  }
}
```

## Troubleshooting

### Module Resolution Issues
If npx doesn't properly resolve packages, use direct path:
```json
{
  "command": "npx",
  "args": ["tsx", "/path/to/context7-mcp/src/index.ts"]
}
```

### Rate Limiting
Without API key: Limited requests per hour
With API key: Higher limits for heavy usage

## Local Development
```bash
# Clone repository
git clone https://github.com/upstash/context7.git
cd context7

# Install dependencies
npm install

# Run locally
npm run dev
```

## GitHub Repository
https://github.com/upstash/context7

## NPM Package
`@upstash/context7-mcp`

## Key Advantages
- **Always Current**: Real-time documentation fetching
- **Zero Configuration**: Works immediately after installation
- **Universal Compatibility**: Supports all major MCP clients
- **Community-Driven**: Add or update libraries via pull requests
