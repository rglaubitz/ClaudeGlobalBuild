# Memory MCP Server Documentation

## Overview
Knowledge graph-based persistent memory system that lets Claude remember information about users across chats. Organizes information through entities, relations, and observations to create a structured memory system.

## Installation

### Claude Desktop Configuration
```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

### Docker Configuration
```json
{
  "mcpServers": {
    "memory": {
      "command": "docker",
      "args": [
        "run", "-i",
        "-v", "claude-memory:/app/dist",
        "--rm", "mcp/memory"
      ]
    }
  }
}
```

### VS Code Configuration
```json
{
  "servers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

## Configuration Options

### Environment Variables
```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"],
      "env": {
        "MEMORY_FILE_PATH": "/path/to/custom/memory.json"
      }
    }
  }
}
```

## Knowledge Graph Structure

### Entities
Primary nodes in the knowledge graph. Each entity has:
- **Name**: Unique identifier (e.g., "John_Smith")
- **Type**: Category (e.g., "person", "organization", "concept")
- **Observations**: List of facts about the entity

Example:
```json
{
  "entityName": "John_Smith",
  "entityType": "person",
  "observations": [
    "Speaks fluent Spanish",
    "Graduated in 2019",
    "Prefers morning meetings"
  ]
}
```

### Relations
Directed connections between entities. Always stored in active voice:
- **From**: Source entity
- **To**: Target entity  
- **Relationship**: Type of connection (e.g., "works_for", "knows", "created")

Example:
```json
{
  "from": "John_Smith",
  "to": "Acme_Corp",
  "relationship": "works_for"
}
```

### Observations
Discrete pieces of information about an entity:
- **Time-stamped**: When the observation was made
- **Immutable**: Cannot be edited, only new ones added
- **Accumulative**: Build complete picture over time

## Available Tools

### Core Operations
- `create_entities` - Create new entities in the knowledge graph
- `create_relations` - Create relationships between entities
- `add_observations` - Add facts to existing entities
- `delete_entities` - Remove entities from the graph
- `delete_relations` - Remove relationships
- `delete_observations` - Remove specific observations

### Query Operations
- `read_graph` - Retrieve entire knowledge graph structure
- `search_nodes` - Search for nodes based on text query
- `open_nodes` - Retrieve specific nodes by name
- `list_entities` - List all entities of a specific type

## Usage Prompts

### Sample Memory Prompt for Claude
```markdown
Follow these steps for each interaction:

1. User Identification:
   - Assume you are interacting with default_user
   - If not identified, proactively try to identify

2. Memory Retrieval:
   - Begin with "Remembering..." and retrieve relevant information
   - Always refer to knowledge graph as your "memory"

3. Memory Storage:
   - Store new information in these categories:
     a) Basic Identity (age, gender, location, job)
     b) Preferences (likes, dislikes, habits)
     c) Relationships (family, friends, colleagues)
     d) Important dates (birthdays, anniversaries)
     e) Projects and goals
     f) Context from conversations

4. Memory Usage:
   - Personalize responses based on stored information
   - Reference past conversations naturally
   - Update information when corrections are made
```

## Usage Examples

### Creating Entities
```
"Create an entity for John Smith who is a software engineer"
```

### Adding Relations
```
"John Smith works for Acme Corp"
```

### Adding Observations
```
"John Smith prefers Python over Java and likes coffee"
```

### Searching Memory
```
"What do you remember about John?"
"Search for information about coffee preferences"
```

## Data Persistence

### Storage Format
- **Line-delimited JSON** (JSONL format)
- **Default location**: `memory.json` in server directory
- **Backup recommended**: Regular backups of memory file

### Data Structure
```json
{"type": "entity", "data": {...}}
{"type": "relation", "data": {...}}
{"type": "observation", "data": {...}}
```

## Best Practices

### Entity Naming
- Use consistent naming: "John_Smith" not "john" or "J Smith"
- Include type suffix when ambiguous: "Apple_company" vs "apple_fruit"
- Keep names unique and descriptive

### Relation Management
- Use active voice: "works_for" not "employed_by"
- Be specific: "manages" not just "related_to"
- Maintain bidirectional awareness

### Observation Guidelines
- Keep observations atomic (one fact per observation)
- Include temporal context when relevant
- Don't duplicate existing observations
- Update by adding new observations, not editing

## Memory Strategies

### Conversation Memory
1. **Identify key facts** during conversation
2. **Create entities** for new people/concepts
3. **Link relationships** between entities
4. **Add observations** for details
5. **Query before responding** to use context

### Project Memory
- Create project entity
- Link team members as relations
- Add milestones as observations
- Track decisions and rationale

### Personal Preferences
- Create user entity
- Add preferences as observations
- Update when preferences change
- Reference in personalized responses

## Troubleshooting

### Common Issues
- **Memory not persisting**: Check file path permissions
- **Duplicate entities**: Use consistent naming
- **Lost relations**: Ensure both entities exist
- **Search not finding**: Try broader search terms

### Performance Tips
- Limit entities to few thousand for best performance
- Periodically clean up unused entities
- Use specific searches rather than full graph reads

## GitHub Repository
https://github.com/modelcontextprotocol/servers/tree/main/src/memory

## NPM Package
`@modelcontextprotocol/server-memory`

## Notes
- Memory persists across Claude sessions
- Each memory file is independent
- Can run multiple instances with different memory files
- Docker volume ensures persistence in containers
