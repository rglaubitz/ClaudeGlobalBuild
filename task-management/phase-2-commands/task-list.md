# Phase 2: Complete Command System - Task List

## Phase Overview
**Duration:** 1 hour
**Priority:** HIGH
**Dependencies:** Phase 1 MCP servers operational
**Risk Level:** Medium (God mode command is dangerous)

## Tasks

### Section 2.1: Core Command Creation (45 minutes)

#### Task 2.1.1: Create /activate-god-mode Command
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/activate-god-mode.md
- **Content Structure:**
  ```markdown
  # Activate God Mode - Full Permissions
  
  WARNING: This enables dangerous operations. Use with extreme caution.
  
  EXECUTE:
  1. Backup current settings
  2. Switch to godmode.json
  3. Display warning
  4. Set auto-revert timer (30 min)
  
  $ARGUMENTS
  ```
- **Time Estimate:** 5 minutes
- **Dependencies:** godmode.json exists
- **Validation:** Test in sandbox first
- **Security:** Add confirmation prompt
- **Enhancement:** Auto-revert after timeout

#### Task 2.1.2: Create /safe-mode Companion Command
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/safe-mode.md
- **Purpose:** Quick revert from god mode
- **Time Estimate:** 3 minutes
- **Dependencies:** Task 2.1.1
- **Features:**
  - Immediate revert to safe settings
  - Clear god mode timer
  - Verify restricted permissions active
- **Testing:** Must work even if system partially broken

#### Task 2.1.3: Create /serena Command
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/serena.md
- **Purpose:** One-line Serena activation
- **Key Feature:** Auto-detect project type and configure
- **Time Estimate:** 5 minutes
- **Dependencies:** None
- **Validation:** Test with multiple project types
- **Enhancement:** Add language detection

#### Task 2.1.4: Create /security-audit Command
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/security-audit.md
- **Components:**
  1. Dependency scanning (npm, pip, cargo)
  2. Secret detection (API keys, passwords)
  3. Permission audit (file permissions)
  4. Network exposure (open ports)
  5. Report generation (JSON + markdown)
- **Time Estimate:** 8 minutes
- **Dependencies:** ripgrep, network tools
- **Output:** ~/.claude/security/audit-{timestamp}.md
- **Automation:** Can be scheduled via cron

#### Task 2.1.5: Create /build-agent Command
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/build-agent.md
- **Integration:** Archon agent builder
- **Parameters:**
  - Agent name
  - Purpose/description
  - Available tools
  - Trigger conditions
  - Model selection (NEW)
- **Time Estimate:** 6 minutes
- **Dependencies:** Archon available
- **Validation:** Create test agent
- **Storage:** ~/.claude/agents/{name}.md

#### Task 2.1.6: Create /daily-standup Command
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/daily-standup.md
- **Data Sources:**
  - Yesterday's health status
  - Completed tasks (from todo system)
  - Today's context (daily-context.md)
  - Active PRs/issues (GitHub)
  - System metrics
- **Time Estimate:** 5 minutes
- **Dependencies:** Health monitoring active
- **Output Format:** Bullet points
- **Enhancement:** Email summary option

#### Task 2.1.7: Create /research-mode Command
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/research-mode.md
- **Activation:**
  - Enable multi-source search
  - Set quality threshold (15+)
  - Configure output directory
  - Start pattern tracking
- **Time Estimate:** 5 minutes
- **Dependencies:** Research bot script
- **Parameters:** Topics to research
- **Deactivation:** /research-mode off

#### Task 2.1.8: Create /backup-now Command
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/backup-now.md
- **Features:**
  - Full system backup
  - Selective backup (configs only)
  - Encrypted option
  - Cloud upload option
- **Time Estimate:** 4 minutes
- **Dependencies:** Backup scripts from Phase 1
- **Validation:** Restore test
- **Enhancement:** Progress bar

#### Task 2.1.9: Create /health-status Command  
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/health-status.md
- **Dashboard Shows:**
  - MCP server status (visual grid)
  - Automation status (cron jobs)
  - Memory usage
  - Recent errors
  - Performance metrics
- **Time Estimate:** 4 minutes
- **Dependencies:** Health check script
- **Output:** ASCII dashboard
- **Refresh:** Auto-refresh option

### Section 2.2: Command Infrastructure (15 minutes)

#### Task 2.2.1: Create Command Test Framework
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/test-commands.sh
- **Test Types:**
  - Syntax validation
  - Dry-run execution
  - Parameter handling
  - Error scenarios
- **Time Estimate:** 5 minutes
- **Dependencies:** All commands created
- **Output:** Test report with pass/fail
- **CI Integration:** Can run in GitHub Actions

#### Task 2.2.2: Create Command Documentation
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/README.md
- **Sections:**
  - Command list with descriptions
  - Usage examples
  - Parameter reference
  - Common workflows
  - Troubleshooting
- **Time Estimate:** 5 minutes
- **Dependencies:** All commands created
- **Format:** Markdown with TOC
- **Auto-generation:** Script to update from command files

#### Task 2.2.3: Create Command Aliases
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/aliases.json
- **Examples:**
  ```json
  {
    "/god": "/activate-god-mode",
    "/safe": "/safe-mode",
    "/audit": "/security-audit",
    "/status": "/health-status"
  }
  ```
- **Time Estimate:** 3 minutes
- **Dependencies:** None
- **Validation:** No conflicts with existing commands
- **User customizable:** Yes

#### Task 2.2.4: Command Usage Analytics
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/command-analytics.sh
- **Tracking:**
  - Command frequency
  - Success/failure rates
  - Execution times
  - Parameter patterns
- **Time Estimate:** 2 minutes
- **Privacy:** Local only, no external tracking
- **Output:** Weekly usage report
- **Purpose:** Identify most useful commands

## Validation Checklist

### Pre-Implementation
- [ ] Phase 1 MCP servers connected
- [ ] Backup system operational
- [ ] Test environment ready
- [ ] God mode safety measures defined

### Post-Implementation  
- [ ] All 9+ commands created
- [ ] Each command has help text
- [ ] Test suite passes
- [ ] Documentation complete
- [ ] Aliases configured
- [ ] No naming conflicts

## Risk Assessment

### High Risks
1. **God Mode Activation**
   - Risk: Accidental system damage
   - Mitigation: Auto-revert, confirmation prompt
   - Monitor: Audit log of god mode usage

2. **Security Audit False Positives**
   - Risk: Flagging legitimate patterns as vulnerabilities
   - Mitigation: Whitelist configuration
   - Review: Manual review of first runs

### Medium Risks
1. **Command Name Conflicts**
   - Risk: Overwriting existing commands
   - Mitigation: Namespace prefixing
   - Check: Validate before creation

2. **Performance Impact**
   - Risk: Heavy commands slowing system
   - Mitigation: Async execution option
   - Monitor: Execution time tracking

## Success Metrics
- **Primary:** All commands executable without errors
- **Secondary:** Average execution time <2 seconds
- **Tertiary:** Help text clarity (user feedback)
- **Bonus:** 50%+ commands used daily

## Integration Points
- **With Phase 1:** Uses MCP servers for functionality
- **With Phase 3:** Commands can trigger automation
- **With Phase 4:** Commands emit events to event bus
- **With Phase 5:** Commands feed learning pipeline

## Next Steps
After Phase 2 completion:
1. Run full command test suite
2. Create quick reference card
3. Set up command shortcuts in shell
4. Train on common workflows
5. Proceed to Phase 3 (Automation)

## Notes
- Commands use markdown for portability
- $ARGUMENTS allows parameter passing
- All commands should be idempotent
- Consider command versioning for updates
- Add telemetry for improvement insights