# Code Style and Conventions

## Language
- **Primary Language**: Python
- **Framework Preferences**: FastAPI for APIs, SQLAlchemy/SQLModel for ORM

## Python Standards
- Follow PEP8 standards strictly
- Use type hints for all function parameters and returns
- Format code with `black`
- Use `pydantic` for data validation
- Maximum file size: 500 lines (refactor if approaching limit)

## Code Organization
- Organize into clearly separated modules by feature/responsibility
- For agents:
  - `agent.py` - Main agent definition and execution logic
  - `tools.py` - Tool functions used by the agent  
  - `prompts.py` - System prompts
- Use relative imports within packages
- Use `python_dotenv` and `load_env()` for environment variables

## Documentation
- Write Google-style docstrings for every function:
```python
def example():
    """
    Brief summary.
    
    Args:
        param1 (type): Description.
    
    Returns:
        type: Description.
    """
```
- Comment non-obvious code with `# Reason:` explanations
- Ensure code is understandable to mid-level developers
- Update README.md when features change

## Testing
- Always create Pytest unit tests for new features
- Tests live in `/tests` folder mirroring app structure
- Include at least:
  - 1 test for expected use
  - 1 edge case
  - 1 failure case
- Update existing tests when logic changes

## Task Management
- Check `TASK.md` before starting work
- Mark tasks complete immediately after finishing
- Add discovered sub-tasks to `TASK.md`
- Read `PLANNING.md` at conversation start

## Quality Gates
- Only patterns scoring 21+ (out of 30) get promoted
- 100% of commands have rollback capability
- All agents pass acceptance criteria before activation