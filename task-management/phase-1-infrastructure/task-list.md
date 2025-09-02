# Phase 1: Critical Infrastructure Fixes - Task List

## Phase Overview
**Duration:** 2 hours
**Priority:** CRITICAL
**Dependencies:** None (first phase)
**Risk Level:** Medium-High

## Tasks

### Section 1.1: MCP Server Repairs (1 hour)

#### Task 1.1.1: Remove Broken Filesystem MCP
- **Status:** ⬜ Not Started
- **Command:** `claude mcp remove filesystem`
- **Time Estimate:** 2 minutes
- **Dependencies:** None
- **Validation:** Run `claude mcp list` and verify filesystem is removed
- **Rollback:** None needed (removing broken server)
- **Alternative:** Manual edit of ~/.claude.json if command fails

#### Task 1.1.2: Reinstall Filesystem MCP
- **Status:** ⬜ Not Started
- **Command:** `claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem /`
- **Time Estimate:** 5 minutes
- **Dependencies:** Task 1.1.1 complete
- **Validation:** `claude mcp list` shows filesystem "✓ Connected"
- **Rollback:** `claude mcp remove filesystem`
- **Alternative:** Try with different path if "/" causes issues

#### Task 1.1.3: Remove Broken Context7
- **Status:** ⬜ Not Started
- **Command:** `claude mcp remove context7`
- **Time Estimate:** 2 minutes
- **Dependencies:** None
- **Validation:** Verify removal with `claude mcp list`
- **Rollback:** None needed
- **Alternative:** Manual ~/.claude.json edit

#### Task 1.1.4: Install Context7 Remote
- **Status:** ⬜ Not Started
- **Command:** `claude mcp add context7 -- npx -y mcp-remote https://mcp.context7.com/mcp`
- **Time Estimate:** 5 minutes
- **Dependencies:** Task 1.1.3 complete
- **Validation:** Test with documentation fetch request
- **Rollback:** `claude mcp remove context7`
- **Alternative:** Skip if API requires paid subscription

#### Task 1.1.5: Create GitHub Token
- **Status:** ⬜ Not Started
- **Steps:**
  1. Navigate to https://github.com/settings/tokens
  2. Click "Generate new token (classic)"
  3. Name: "Claude-Code-MCP"
  4. Select scopes: `repo`, `workflow`, `read:org`
  5. Generate and copy token
- **Time Estimate:** 5 minutes
- **Dependencies:** GitHub account
- **Validation:** Token starts with `ghp_`
- **Rollback:** Revoke token from GitHub settings
- **Security:** Store token in password manager immediately

#### Task 1.1.6: Update GitHub Token in Config
- **Status:** ⬜ Not Started
- **Command:** `sed -i '' 's/"GITHUB_TOKEN": "YOUR_TOKEN_HERE"/"GITHUB_TOKEN": "ghp_ACTUAL_TOKEN"/' ~/.claude.json`
- **Time Estimate:** 2 minutes
- **Dependencies:** Task 1.1.5 complete
- **Validation:** Test with `gh api user` or GitHub MCP command
- **Rollback:** Restore from backup
- **Security:** Ensure file permissions are 600

#### Task 1.1.7: Install Browserbase MCP
- **Status:** ⬜ Not Started
- **Command:** `claude mcp add browserbase -- npx -y @browserbasehq/mcp-server-browserbase`
- **Time Estimate:** 5 minutes
- **Dependencies:** None
- **Validation:** Check connection status
- **Rollback:** `claude mcp remove browserbase`
- **Note:** May require API key from browserbase.com

#### Task 1.1.8: Configure Browserbase Credentials
- **Status:** ⬜ Not Started
- **Steps:**
  1. Get API key from browserbase.com
  2. Add to ~/.claude.json env section
  3. Set BROWSERBASE_PROJECT_ID
- **Time Estimate:** 10 minutes
- **Dependencies:** Task 1.1.7, Browserbase account
- **Validation:** Test browser automation command
- **Rollback:** Remove env variables
- **Alternative:** Use Playwright only if Browserbase unavailable

#### Task 1.1.9: Install TaskMaster MCP
- **Status:** ⬜ Not Started
- **Command:** `claude mcp add taskmaster -- npx -y @taskmaster/mcp-server`
- **Time Estimate:** 5 minutes
- **Dependencies:** None
- **Validation:** Verify in MCP list
- **Rollback:** `claude mcp remove taskmaster`
- **Benefits:** AI-powered task breakdown

#### Task 1.1.10: Install MCP-Cron Server
- **Status:** ⬜ Not Started
- **Command:** `claude mcp add cron -- npx -y @jolks/mcp-cron`
- **Time Estimate:** 5 minutes
- **Dependencies:** None
- **Validation:** Test cron expression parsing
- **Rollback:** `claude mcp remove cron`
- **Benefits:** Better than bash cron for scheduling

#### Task 1.1.11: Comprehensive MCP Validation
- **Status:** ⬜ Not Started
- **Steps:**
  1. Run `claude mcp list`
  2. Document all connection statuses
  3. Test each MCP with simple command
  4. Record response times
- **Time Estimate:** 10 minutes
- **Dependencies:** Tasks 1.1.1-1.1.10
- **Success Criteria:** 6+ servers showing "Connected"
- **Troubleshooting:** Check logs with `--mcp-debug` flag

### Section 1.2: Backup & Recovery Setup (30 minutes)

#### Task 1.2.1: Create Backup Directory Structure
- **Status:** ⬜ Not Started
- **Command:** `mkdir -p ~/.claude/backups/{daily,weekly,configs,emergency}`
- **Time Estimate:** 2 minutes
- **Dependencies:** None
- **Validation:** `ls -la ~/.claude/backups/`
- **Rollback:** None needed
- **Enhancement:** Add .gitignore for sensitive files

#### Task 1.2.2: Initial Configuration Backup
- **Status:** ⬜ Not Started
- **Script:**
```bash
#!/bin/bash
BACKUP_DIR=~/.claude/backups/configs
TIMESTAMP=$(date +%Y%m%d-%H%M%S)
cp ~/.claude.json "$BACKUP_DIR/claude-config-$TIMESTAMP.json"
cp -r ~/.claude/settings*.json "$BACKUP_DIR/"
tar -czf "$BACKUP_DIR/full-backup-$TIMESTAMP.tar.gz" ~/.claude/
```
- **Time Estimate:** 5 minutes
- **Dependencies:** Task 1.2.1
- **Validation:** Verify backup files exist and are non-empty
- **Retention:** Keep last 10 backups
- **Automation:** Add to cron later

#### Task 1.2.3: Create Restoration Script
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/restore-mcp.sh
- **Features:**
  - List available backups
  - Select specific backup or latest
  - Dry-run mode
  - Verification after restore
- **Time Estimate:** 10 minutes
- **Dependencies:** Task 1.2.2
- **Testing:** Test with non-critical file first
- **Documentation:** Add --help flag

#### Task 1.2.4: Emergency Recovery Plan
- **Status:** ⬜ Not Started
- **Components:**
  1. Quick restore script (one-liner)
  2. Offline backup to external drive
  3. Documentation of manual recovery steps
  4. Contact info for support
- **Time Estimate:** 10 minutes
- **Dependencies:** Tasks 1.2.1-1.2.3
- **Testing:** Simulate failure and recovery
- **Location:** ~/.claude/EMERGENCY-RECOVERY.md

#### Task 1.2.5: Backup Validation Test
- **Status:** ⬜ Not Started
- **Steps:**
  1. Create test backup
  2. Corrupt current config (safely)
  3. Run restoration
  4. Verify functionality
  5. Document results
- **Time Estimate:** 15 minutes
- **Dependencies:** All Section 1.2 tasks
- **Success Criteria:** Full recovery in <2 minutes
- **Report:** Save test results to logs

## Validation Checklist

### Pre-Implementation
- [ ] Current config backed up
- [ ] GitHub token ready
- [ ] API keys obtained (if needed)
- [ ] Network connectivity verified
- [ ] 2GB free disk space

### Post-Implementation
- [ ] All MCP servers showing correct status
- [ ] Filesystem MCP can browse files
- [ ] Context7 can fetch documentation
- [ ] GitHub operations work with token
- [ ] Backup system tested
- [ ] Restoration verified
- [ ] Performance acceptable (<5s for MCP commands)

## Risk Assessment

### High Risks
1. **GitHub Token Exposure**
   - Mitigation: Use environment variable or secure storage
   - Monitor: Check GitHub audit log

2. **MCP Server Incompatibility**
   - Mitigation: Test each server individually
   - Fallback: Previous working config in backup

3. **Network Dependencies**
   - Mitigation: Local alternatives where possible
   - Fallback: Offline mode documentation

### Medium Risks
1. **Windows Path Issues**
   - Mitigation: Use forward slashes consistently
   - Test: WSL compatibility

2. **Permission Errors**
   - Mitigation: Check file ownership
   - Fix: `chmod 755` for scripts

## Success Metrics
- **Primary:** 100% MCP servers connected
- **Secondary:** Backup/restore working
- **Performance:** MCP commands respond in <3 seconds
- **Reliability:** No failures in 24-hour test

## Next Steps
After Phase 1 completion:
1. Document all configuration changes
2. Create snapshot for rollback point
3. Run integration tests
4. Proceed to Phase 2 (Commands) only after validation

## Notes
- Keep terminal open with `claude mcp list` for monitoring
- Document any deviations from plan
- If more than 3 MCP servers fail, stop and troubleshoot
- Consider Docker containers for complex MCPs (future enhancement)