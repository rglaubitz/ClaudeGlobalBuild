# Phase 3: Automation Activation - Task List

## Phase Overview
**Duration:** 1.5 hours
**Priority:** HIGH
**Dependencies:** Phase 1 MCP servers, Phase 2 commands
**Risk Level:** Medium (automation can run away if misconfigured)

## Tasks

### Section 3.1: Cron Automation Setup (45 minutes)

#### Task 3.1.1: Install and Configure MCP-Cron Server
- **Status:** ⬜ Not Started
- **File:** ~/.claude/mcp-servers/mcp-cron/
- **Installation:**
  ```bash
  npm install -g @modelcontextprotocol/server-cron
  ```
- **Configuration:**
  ```json
  {
    "mcpServers": {
      "cron": {
        "command": "mcp-cron",
        "args": ["--config", "~/.claude/cron/config.json"]
      }
    }
  }
  ```
- **Time Estimate:** 10 minutes
- **Dependencies:** npm installed
- **Validation:** Test with simple schedule
- **Why MCP-Cron:** Superior to bash cron - supports AI prompts and shell commands
- **Features:**
  - Natural language scheduling
  - Error recovery built-in
  - MCP integration native

#### Task 3.1.2: Configure Health Check Automation
- **Status:** ⬜ Not Started
- **File:** ~/.claude/cron/health-check.json
- **Schedule:** Every 5 minutes
- **Configuration:**
  ```json
  {
    "name": "health-check",
    "schedule": "*/5 * * * *",
    "command": "~/.claude/scripts/health-check.sh",
    "retries": 3,
    "timeout": 30,
    "notifications": {
      "onFailure": "email",
      "onRecovery": "email"
    }
  }
  ```
- **Time Estimate:** 8 minutes
- **Dependencies:** health-check.sh from Phase 1
- **Output:** ~/.claude/logs/health/
- **Alert Thresholds:**
  - 3 consecutive failures = alert
  - Any MCP server down = alert
  - Memory > 80% = warning

#### Task 3.1.3: Setup Daily Report Generation
- **Status:** ⬜ Not Started
- **File:** ~/.claude/cron/daily-report.json
- **Schedule:** 9 AM daily
- **Components:**
  - Yesterday's health metrics
  - Command usage stats
  - Error summary
  - Learning pipeline results
  - GitHub activity
- **Time Estimate:** 7 minutes
- **Dependencies:** Analytics scripts
- **Output Format:** Markdown + JSON
- **Distribution:** Email + Slack webhook

#### Task 3.1.4: Configure Research Bot Schedule
- **Status:** ⬜ Not Started
- **File:** ~/.claude/cron/research-bot.json
- **Schedule:** Every 6 hours
- **AI Prompt Mode:**
  ```json
  {
    "name": "research-bot",
    "schedule": "0 */6 * * *",
    "type": "ai-prompt",
    "prompt": "Research latest Claude Code updates and MCP developments. Score findings and update knowledge base.",
    "model": "claude-3-opus",
    "timeout": 300
  }
  ```
- **Time Estimate:** 6 minutes
- **Dependencies:** Research mode command
- **Quality Threshold:** 15+ score
- **Storage:** ~/.claude/research/findings/

#### Task 3.1.5: Setup Backup Automation
- **Status:** ⬜ Not Started
- **File:** ~/.claude/cron/backup.json
- **Schedule Types:**
  - Hourly: Config files only
  - Daily: Full backup at 2 AM
  - Weekly: Encrypted cloud upload
- **Time Estimate:** 5 minutes
- **Dependencies:** Backup scripts from Phase 1
- **Retention Policy:**
  - Hourly: Keep 24
  - Daily: Keep 30
  - Weekly: Keep 12
- **Verification:** Auto-restore test weekly

#### Task 3.1.6: Configure Pattern Extraction Schedule
- **Status:** ⬜ Not Started
- **File:** ~/.claude/cron/pattern-extraction.json
- **Schedule:** Every 4 hours
- **Process:**
  1. Analyze recent commands
  2. Extract usage patterns
  3. Score effectiveness
  4. Update pattern library
- **Time Estimate:** 5 minutes
- **Dependencies:** Learning pipeline (Phase 5)
- **Minimum Data:** 10 command executions
- **Output:** ~/.claude/patterns/extracted/

### Section 3.2: Monitoring Infrastructure (30 minutes)

#### Task 3.2.1: Create System Monitor Dashboard
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/monitor-dashboard.sh
- **Display Elements:**
  ```
  ┌─────────────────────────────────────┐
  │     Claude System Monitor v1.0      │
  ├─────────────────────────────────────┤
  │ MCP Servers:  ████████████ 12/12 OK │
  │ Memory:       ████░░░░░░░░ 42%      │
  │ CPU:          ████████░░░░ 67%      │
  │ Automations:  ████████████ All Active│
  │ Last Error:   None (2h ago)         │
  └─────────────────────────────────────┘
  ```
- **Time Estimate:** 10 minutes
- **Dependencies:** tmux or screen
- **Refresh Rate:** 30 seconds
- **Alert Colors:** Green/Yellow/Red

#### Task 3.2.2: Setup Log Aggregation
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/log-aggregator.sh
- **Log Sources:**
  - MCP server logs
  - Command execution logs
  - Automation logs
  - Error logs
- **Time Estimate:** 8 minutes
- **Dependencies:** None
- **Output:** ~/.claude/logs/aggregated/
- **Rotation:** Daily with compression
- **Search:** Indexed with ripgrep

#### Task 3.2.3: Create Alert System
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/alert-system.sh
- **Alert Channels:**
  - Email (primary)
  - Slack webhook
  - Desktop notification
  - SMS (critical only)
- **Time Estimate:** 7 minutes
- **Dependencies:** mail, curl
- **Alert Levels:**
  - INFO: Log only
  - WARNING: Email digest
  - ERROR: Immediate email
  - CRITICAL: All channels
- **Rate Limiting:** Max 10 alerts/hour

#### Task 3.2.4: Implement Performance Tracking
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/performance-tracker.sh
- **Metrics Tracked:**
  - Command execution times
  - MCP response times
  - Memory usage trends
  - Pattern success rates
- **Time Estimate:** 5 minutes
- **Dependencies:** time command
- **Storage:** SQLite database
- **Visualization:** ASCII graphs

### Section 3.3: Testing & Validation (15 minutes)

#### Task 3.3.1: Create Automation Test Suite
- **Status:** ⬜ Not Started
- **File:** ~/.claude/tests/test-automation.sh
- **Test Coverage:**
  - All cron jobs trigger correctly
  - Alerts fire on conditions
  - Logs rotate properly
  - Dashboard displays accurately
- **Time Estimate:** 5 minutes
- **Dependencies:** All automation scripts
- **Output:** Test report with pass/fail
- **Mode:** Can run in dry-run mode

#### Task 3.3.2: Setup Automation Kill Switch
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/kill-automation.sh
- **Features:**
  - Stop all cron jobs immediately
  - Preserve current state
  - Log the kill event
  - Send notification
- **Time Estimate:** 3 minutes
- **Dependencies:** None
- **Activation:** Manual or on 5 critical errors
- **Recovery:** Manual restart required

#### Task 3.3.3: Create Automation Documentation
- **Status:** ⬜ Not Started
- **File:** ~/.claude/automation/README.md
- **Contents:**
  - Schedule overview table
  - Alert threshold reference
  - Troubleshooting guide
  - Recovery procedures
- **Time Estimate:** 5 minutes
- **Dependencies:** All automation complete
- **Format:** Markdown with diagrams
- **Updates:** Auto-generated from configs

#### Task 3.3.4: Validate MCP-Cron Integration
- **Status:** ⬜ Not Started
- **Tests:**
  - AI prompt execution
  - Shell command execution
  - Error handling
  - Retry logic
- **Time Estimate:** 2 minutes
- **Dependencies:** MCP-Cron installed
- **Success Criteria:** All test schedules execute

## Validation Checklist

### Pre-Implementation
- [ ] Phase 1 & 2 complete and stable
- [ ] MCP-Cron server available
- [ ] Email/Slack credentials configured
- [ ] Backup destination ready

### Post-Implementation
- [ ] All cron jobs scheduled
- [ ] Monitoring dashboard operational
- [ ] Alerts tested and working
- [ ] Kill switch tested
- [ ] Documentation complete
- [ ] Performance baseline established

## Risk Assessment

### High Risks
1. **Runaway Automation**
   - Risk: Scripts executing too frequently
   - Mitigation: Rate limiting, kill switch
   - Monitor: Execution count alerts

2. **Alert Storm**
   - Risk: Too many alerts overwhelming
   - Mitigation: Alert aggregation, rate limiting
   - Review: Daily alert volume

### Medium Risks
1. **MCP-Cron Failure**
   - Risk: All automation stops
   - Mitigation: Fallback to system cron
   - Monitor: Cron service health

2. **Log Disk Usage**
   - Risk: Logs fill disk
   - Mitigation: Rotation, compression, cleanup
   - Monitor: Disk usage alerts

## Success Metrics
- **Primary:** 100% scheduled tasks executing
- **Secondary:** <1% false positive alerts
- **Tertiary:** Dashboard uptime >99%
- **Bonus:** 50% reduction in manual tasks

## Integration Points
- **With Phase 1:** Uses MCP servers
- **With Phase 2:** Triggers commands
- **With Phase 4:** Sends events to bus
- **With Phase 5:** Feeds learning pipeline
- **With Phase 6:** Included in validation

## Next Steps
After Phase 3 completion:
1. Monitor automation for 24 hours
2. Tune alert thresholds
3. Optimize schedules based on usage
4. Document patterns observed
5. Proceed to Phase 4 (Event Bus)

## Notes
- MCP-Cron superior to bash cron (40% fewer failures)
- Consider containerizing automation services
- Plan for time zone handling
- Add automation metrics to daily standup
- Consider using Airflow for complex workflows later