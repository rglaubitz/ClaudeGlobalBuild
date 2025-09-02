# Task Completion Checklist

## When Completing Any Task

### 1. Code Quality Checks
- [ ] Run `black .` to format Python code
- [ ] Run `pytest` to ensure all tests pass
- [ ] Run linting if available (`ruff check`)
- [ ] Verify no files exceed 500 lines

### 2. Testing
- [ ] Create/update unit tests for new features
- [ ] Ensure tests cover:
  - Expected use case
  - Edge case
  - Failure case
- [ ] All tests pass in `/tests` folder

### 3. Documentation
- [ ] Update docstrings for new/modified functions
- [ ] Add `# Reason:` comments for non-obvious code
- [ ] Update README.md if features changed
- [ ] Update relevant documentation files

### 4. Task Management
- [ ] Mark task as complete in `TASK.md`
- [ ] Add any discovered sub-tasks to `TASK.md`
- [ ] Document completion date

### 5. Git Operations (if requested)
- [ ] Stage appropriate files with `git add`
- [ ] Write clear commit message
- [ ] Ensure no secrets/keys in commits

### 6. Validation
- [ ] Verify implementation matches requirements
- [ ] Check for security best practices
- [ ] Ensure consistent code style
- [ ] Confirm file paths and imports are correct

### 7. Performance Review
- [ ] Run `/reflect` command after major tasks
- [ ] Document lessons learned
- [ ] Identify improvement opportunities

## Important Reminders
- Never assume missing context - ask questions
- Never hallucinate libraries or functions
- Never delete/overwrite code unless instructed
- Always confirm file paths exist before referencing