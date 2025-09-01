# Playwright MCP Server Documentation

## Overview
A Model Context Protocol (MCP) server that provides browser automation capabilities using Playwright. This server enables LLMs to interact with web pages through structured accessibility snapshots, bypassing the need for screenshots or visually-tuned models.

## Key Features
- **Fast and lightweight**: Uses Playwright's accessibility tree, not pixel-based input
- **LLM-friendly**: No vision models needed, operates purely on structured data
- **Deterministic tool application**: Avoids ambiguity common with screenshot-based approaches
- **Multi-browser support**: Chromium, Firefox, WebKit
- **Headless or headed operation**

## Installation

### Claude Desktop Configuration
```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

### Alternative with ExecuteAutomation Version
```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@executeautomation/playwright-mcp-server"]
    }
  }
}
```

### VS Code Configuration
```bash
code --add-mcp '{"name":"playwright","command":"npx","args":["@playwright/mcp@latest"]}'
```

## Command Line Arguments

```bash
npx @playwright/mcp@latest --help

--allowed-origins <origins>  # Semicolon-separated list of origins to allow
--blocked-origins <origins>  # Semicolon-separated list of origins to block
--block-service-workers      # Block service workers
--browser <browser>          # chrome, firefox, webkit, msedge
--caps <caps>               # Additional capabilities: vision, pdf
--cdp-endpoint <endpoint>   # CDP endpoint to connect to
--config <path>             # Path to configuration file
--device <device>           # Device to emulate (e.g., "iPhone 15")
--executable-path <path>    # Path to browser executable
--extension                 # Connect to running browser instance
--headless                  # Run browser in headless mode
--host <host>              # Host to bind server to (default: localhost)
--ignore-https-errors       # Ignore HTTPS errors
--isolated                  # Keep browser profile in memory
--image-responses <mode>    # "allow" or "omit" (default: allow)
--no-sandbox               # Disable sandbox
--output-dir <path>        # Path for output files
--port <port>              # Port for SSE transport
```

## Available Tools

### Browser Control
- `browser_navigate` - Navigate to URL
- `browser_navigate_back` - Go back in history
- `browser_navigate_forward` - Go forward in history
- `browser_refresh` - Refresh current page
- `browser_close` - Close browser

### Page Interaction
- `browser_click` - Click on elements
- `browser_type` - Type text into elements
- `browser_fill_form` - Fill multiple form fields
- `browser_select_option` - Select dropdown options
- `browser_press_key` - Press keyboard keys
- `browser_hover` - Hover over elements
- `browser_drag` - Drag and drop elements

### Content & Analysis
- `browser_get_visible_text` - Get visible text content
- `browser_get_html` - Get HTML content
- `browser_take_screenshot` - Capture screenshots
- `browser_save_pdf` - Save page as PDF
- `browser_execute_javascript` - Execute JavaScript
- `browser_web_scrape` - Extract structured data

### Testing & Recording
- `playwright_codegen_start` - Start recording test actions
- `playwright_codegen_stop` - Stop recording and generate test
- `playwright_wait_for_response` - Wait for HTTP responses
- `playwright_assert_response` - Validate responses

### Console & Network
- `browser_get_console_logs` - Get console messages
- Monitor network requests
- Track JavaScript errors

## Use Cases

1. **Web Testing**: Automated end-to-end testing
2. **Web Scraping**: Extract data from websites
3. **Form Automation**: Fill and submit forms
4. **Visual Testing**: Screenshot comparison
5. **API Testing**: Validate endpoints
6. **Cross-browser Testing**: Test across multiple browsers

## GitHub Repositories
- Microsoft Official: https://github.com/microsoft/playwright-mcp
- ExecuteAutomation: https://github.com/executeautomation/mcp-playwright

## NPM Packages
- `@playwright/mcp`
- `@executeautomation/playwright-mcp-server`

## Notes
- When using Docker with stdio servers, don't use detach option (-d)
- Server must run in foreground to communicate with VS Code
- Some clients have 60-character limit for server:tool_name combined
