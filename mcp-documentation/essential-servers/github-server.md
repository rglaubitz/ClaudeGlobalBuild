# GitHub MCP Server Documentation

## Overview
Official GitHub MCP server for repository management, file operations, and GitHub API integration. Enables AI models to interact with GitHub repositories, issues, pull requests, and more.

## Installation

### Claude Desktop Configuration
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your-token-here"
      }
    }
  }
}
```

### VS Code Configuration
```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your-token"
      }
    }
  }
}
```

## Required Setup

### GitHub Personal Access Token
1. Go to GitHub Settings
2. Navigate to Developer settings > Personal access tokens
3. Create a new token with required permissions:
   - `repo` - Full control of private repositories
   - `read:org` - Read org and team membership
   - `write:discussion` - Read/write discussions
   - `read:packages` - Read packages
   - `read:project` - Read projects

## Available Tools

### Repository Management
- List repositories
- Create new repository
- Clone repository
- Fork repository
- Archive/unarchive repository
- Get repository details

### File Operations
- Read file contents
- Create/update files
- Delete files
- Move/rename files
- Get file history
- Automatic branch creation for updates

### Branch Management
- List branches
- Create new branch
- Delete branch
- Merge branches
- Compare branches
- Set default branch

### Issues & Pull Requests
- List issues/PRs
- Create new issue/PR
- Update issue/PR
- Close/reopen issue/PR
- Add comments
- Assign users
- Add/remove labels

### Search Functionality
- Search repositories
- Search code
- Search issues
- Search pull requests
- Search users
- Search commits

### Collaboration
- List collaborators
- Add/remove collaborators
- Manage permissions
- Create/manage teams
- Review pull requests

### Workflow Integration
- Trigger GitHub Actions
- View workflow runs
- Download artifacts
- Manage secrets
- Create releases

## Usage Examples

### Repository Operations
```
"List all my GitHub repositories"
"Create a new repository called my-project"
"Fork the facebook/react repository"
```

### File Management
```
"Read the README.md file from my-repo"
"Update the package.json version to 2.0.0"
"Create a new file called config.js with this content..."
```

### Issue Management
```
"List all open issues in my-project"
"Create a new issue titled 'Bug in login flow'"
"Close issue #42 with a comment"
```

### Pull Requests
```
"Create a PR from feature-branch to main"
"Review the latest pull request"
"Merge PR #15 using squash merge"
```

### Search
```
"Search for repositories about machine learning"
"Find all issues mentioning 'performance'"
"Search for code containing 'authentication'"
```

## Advanced Features

### Automatic Branch Creation
When updating files, the server can automatically:
- Create a new branch
- Commit changes
- Push to remote
- Create pull request

### Batch Operations
- Update multiple files in single commit
- Bulk issue operations
- Mass label management

### Integration with CI/CD
- Trigger workflows
- Monitor build status
- Deploy releases
- Manage environments

## Security Considerations

### Token Permissions
Only grant necessary permissions:
- Use fine-grained tokens when possible
- Rotate tokens regularly
- Never commit tokens to repositories

### Rate Limiting
GitHub API has rate limits:
- 5,000 requests/hour for authenticated requests
- 60 requests/hour for unauthenticated
- Server handles rate limiting automatically

## Error Handling
The server provides clear error messages for:
- Authentication failures
- Permission denied
- Rate limit exceeded
- Repository not found
- Invalid operations

## GitHub Repository
https://github.com/modelcontextprotocol/servers

## NPM Package
`@modelcontextprotocol/server-github`

## Notes
- Supports GitHub.com and GitHub Enterprise
- Works with private and public repositories
- Respects repository permissions
- Maintains audit trail of operations
