# CLAUDE.md and TASK.md Alignment Update

## Changes Made (2025-09-03)

### Hierarchy Established
- **CLAUDE.md** → Points to TASK.md for all task management
- **TASK.md** → Contains current phase tracker and points to task-management folder
- **task-management/** → Contains all phase documentation

### Key Clarifications

1. **6-Phase Structure Confirmed**
   - Phase 1: Infrastructure Foundation (MCP servers, Docker, Pass/GPG)
   - Phase 2: Command System Development
   - Phase 3: Automation & Scheduling (Apache Airflow)
   - Phase 4: Event Bus & Resilience (NATS JetStream)
   - Phase 5: Learning Pipeline
   - Phase 6: Validation & Monitoring

2. **Two-Tier Task Structure**
   - Original Tasks (11 hours): Foundation - MUST be done FIRST
   - Enhanced Tasks (9 hours): Production features - done AFTER original
   - Total: 20 hours implementation

3. **Global Directory Focus**
   - Target: `~/.claude/` (global configuration)
   - This will persist across ALL Claude Code sessions
   - Not limited to current project folder

4. **Removed Duplicates**
   - Deleted empty `phase-4-resilience/` folder
   - Phase 4 is now only `phase-4-event-bus/`

## Implementation Notes
- Enhanced tasks are ADDITIONS, not replacements
- Some tasks appear in both lists - do ONCE only
- Check implementation-strategy.md for overlap resolution
- ALWAYS start by checking TASK.md for current phase status