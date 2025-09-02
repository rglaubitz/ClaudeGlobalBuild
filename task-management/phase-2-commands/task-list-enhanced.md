# Phase 2: Complete Command System - Task List (Enhanced)

## Phase Overview
**Duration:** 1.5 hours (increased from 1 hour)
**Priority:** HIGH
**Dependencies:** Phase 1 MCP servers operational
**Risk Level:** Medium (God mode command is dangerous but unchanged)
**Key Enhancements:** Command discovery, input validation, conflict resolution, YAML-based chaining

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
- **NO CHANGES:** Keep exactly as originally designed

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
- **Dependencies:** Serena MCP server connected
- **Validation:** Test with multiple project types
- **Enhancement:** Add language detection

#### Task 2.1.4: Create /unfuck-this Command (Emergency Recovery)
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/unfuck-this.md
- **Content:**
  ```markdown
  # /unfuck-this - Emergency Rollback System
  
  CRITICAL: This will rollback to last known good state
  
  EXECUTE:
  1. Stop all running processes
  2. Restore from latest backup
  3. Reset MCP connections
  4. Clear caches
  5. Restart health monitoring
  
  ROLLBACK_DEPTH: $ARGUMENTS
  ```
- **Time Estimate:** 5 minutes
- **Dependencies:** Backup system from Phase 1
- **Features:**
  - Automatic state detection
  - Multiple rollback points
  - Preserves user data
  - Logs all actions
- **Testing:** Test with intentional break

#### Task 2.1.5: Create /build-agent Command
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/build-agent.md
- **Integration:** Archon agent builder
- **Parameters:**
  - Agent name
  - Purpose/description
  - Available tools
  - Trigger conditions
  - Model selection
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
- **NO CHANGES:** Keep exactly as originally designed

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
  - Encrypted option (using Pass/GPG)
  - Cloud upload option
- **Time Estimate:** 4 minutes
- **Dependencies:** Backup scripts from Phase 1
- **Validation:** Restore test
- **Integration:** Works with /unfuck-this

#### Task 2.1.9: Create /health-status Command
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/health-status.md
- **Dashboard Shows:**
  - MCP server status (via `claude mcp list`)
  - Docker container status
  - Pass store status
  - Recent errors
  - Performance metrics
- **Time Estimate:** 4 minutes
- **Dependencies:** Health check script from Phase 1
- **Output:** ASCII dashboard
- **Refresh:** Auto-refresh option

### Section 2.2: Command Discovery System (20 minutes)

#### Task 2.2.1: Create Command Discovery Engine
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/command-discovery.sh
- **Features:**
  ```bash
  #!/bin/bash
  # Built-in Claude listing approach
  
  discover_commands() {
    echo "=== Available Claude Commands ==="
    
    # List project commands
    if [ -d "./.claude/commands" ]; then
      echo "📁 Project Commands:"
      ls -1 ./.claude/commands/*.md | sed 's/.*\//  \//' | sed 's/.md$//'
    fi
    
    # List user commands
    echo "📁 User Commands:"
    ls -1 ~/.claude/commands/*.md | sed 's/.*\//  \//' | sed 's/.md$//'
    
    # List global commands
    if [ -d "/usr/local/claude/commands" ]; then
      echo "📁 Global Commands:"
      ls -1 /usr/local/claude/commands/*.md | sed 's/.*\//  \//' | sed 's/.md$//'
    fi
  }
  
  # Fuzzy search
  search_command() {
    find ~/.claude/commands -name "*$1*.md" -type f
  }
  ```
- **Time Estimate:** 8 minutes
- **Dependencies:** Command files exist
- **Integration:** Called by `/help` or `/commands`
- **Output:** Categorized command list

#### Task 2.2.2: Create /help Command Enhancement
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/help.md
- **Features:**
  ```markdown
  # Help System
  
  MODES:
  - No args: List all commands with descriptions
  - With command: Show detailed help for that command
  - With --search: Fuzzy search for commands
  
  EXECUTE:
  ~/.claude/scripts/command-discovery.sh $ARGUMENTS
  ```
- **Time Estimate:** 5 minutes
- **Dependencies:** Command discovery engine
- **Output Format:** Formatted markdown
- **Categories:** Core, Project, User, System

#### Task 2.2.3: Add Command Descriptions
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/metadata.json
- **Structure:**
  ```json
  {
    "commands": {
      "/activate-god-mode": {
        "description": "Enable full permissions (dangerous)",
        "category": "system",
        "risk": "high"
      },
      "/unfuck-this": {
        "description": "Emergency rollback to last good state",
        "category": "recovery",
        "risk": "medium"
      },
      "/daily-standup": {
        "description": "Generate daily status report",
        "category": "workflow",
        "risk": "low"
      }
    }
  }
  ```
- **Time Estimate:** 7 minutes
- **Dependencies:** All commands created
- **Usage:** Displayed in /help output
- **Maintenance:** Update when adding commands

### Section 2.3: Input Validation Framework (20 minutes)

#### Task 2.3.1: Create Input Validator
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/validate-input.sh
- **Implementation:**
  ```bash
  #!/bin/bash
  
  validate_input() {
    local input="$1"
    local type="$2"
    
    # Allow some special chars with escaping
    case $type in
      "path")
        # Allow paths but escape spaces and special chars
        echo "$input" | sed 's/ /\\ /g' | sed 's/[;&|<>]/\\&/g'
        ;;
      "alphanumeric")
        # Only letters, numbers, dash, underscore
        echo "$input" | grep -E '^[a-zA-Z0-9_-]+$' || return 1
        ;;
      "number")
        # Only digits
        echo "$input" | grep -E '^[0-9]+$' || return 1
        ;;
      "command")
        # Command names - no special chars except dash
        echo "$input" | grep -E '^[a-zA-Z][a-zA-Z0-9-]*$' || return 1
        ;;
      "arguments")
        # Allow some chars with proper escaping
        # Escape dangerous chars: ; | & < > ` $
        echo "$input" | sed 's/[;&|<>`$]/\\&/g'
        ;;
    esac
  }
  
  # Whitelist validation
  validate_whitelist() {
    local input="$1"
    local whitelist="$2"
    
    if echo "$whitelist" | grep -q "^$input$"; then
      return 0
    else
      echo "Error: '$input' not in allowed values"
      return 1
    fi
  }
  ```
- **Time Estimate:** 10 minutes
- **Dependencies:** None
- **Security:** Prevent injection attacks
- **Flexibility:** Allow some chars with escaping

#### Task 2.3.2: Integrate Validation with Commands
- **Status:** ⬜ Not Started
- **Modification:** Update command templates
- **Example Enhancement:**
  ```markdown
  # Command with validation
  
  VALIDATE:
  - type: path
  - required: true
  - whitelist: [configs, scripts, commands]
  
  EXECUTE:
  validated_input=$(~/.claude/scripts/validate-input.sh "$ARGUMENTS" "path")
  # Use validated_input safely
  ```
- **Time Estimate:** 5 minutes
- **Dependencies:** Validator script
- **Testing:** Test with malicious input
- **Documentation:** Add to command guide

#### Task 2.3.3: Create Validation Test Suite
- **Status:** ⬜ Not Started
- **File:** ~/.claude/tests/test-validation.sh
- **Test Cases:**
  ```bash
  # Test injection attempts
  test_injection() {
    assert_fails 'validate_input "; rm -rf /" "command"'
    assert_fails 'validate_input "| cat /etc/passwd" "arguments"'
    assert_fails 'validate_input "../../../etc/passwd" "path"'
  }
  
  # Test valid inputs
  test_valid() {
    assert_succeeds 'validate_input "my-command" "command"'
    assert_succeeds 'validate_input "/home/user/file.txt" "path"'
    assert_succeeds 'validate_input "123" "number"'
  }
  
  # Test escaping
  test_escaping() {
    result=$(validate_input "my file.txt" "path")
    assert_equals "$result" "my\\ file.txt"
  }
  ```
- **Time Estimate:** 5 minutes
- **Dependencies:** Validation framework
- **Coverage:** All validation types
- **CI Integration:** Run on commits

### Section 2.4: Conflict Resolution System (15 minutes)

#### Task 2.4.1: Create Conflict Detector
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/detect-conflicts.sh
- **Implementation:**
  ```bash
  #!/bin/bash
  
  detect_command_conflicts() {
    local command_name="$1"
    local locations=()
    
    # Check all command locations
    [ -f "./.claude/commands/${command_name}.md" ] && locations+=("project")
    [ -f "$HOME/.claude/commands/${command_name}.md" ] && locations+=("user")
    [ -f "/usr/local/claude/commands/${command_name}.md" ] && locations+=("global")
    
    if [ ${#locations[@]} -gt 1 ]; then
      echo "⚠️  Command conflict detected for '${command_name}'"
      echo "Found in: ${locations[@]}"
      echo ""
      echo "Please specify which to use:"
      echo "  /project:${command_name} - Use project version"
      echo "  /user:${command_name}    - Use user version"
      echo "  /global:${command_name}  - Use global version"
      return 1
    fi
    
    return 0
  }
  
  # Resolve with namespace
  resolve_command() {
    local namespaced="$1"
    
    if [[ "$namespaced" =~ ^/([^:]+):(.+)$ ]]; then
      namespace="${BASH_REMATCH[1]}"
      command="${BASH_REMATCH[2]}"
      
      case $namespace in
        "project") path="./.claude/commands/${command}.md" ;;
        "user")    path="$HOME/.claude/commands/${command}.md" ;;
        "global")  path="/usr/local/claude/commands/${command}.md" ;;
      esac
      
      if [ -f "$path" ]; then
        echo "$path"
        return 0
      fi
    fi
    
    return 1
  }
  ```
- **Time Estimate:** 8 minutes
- **Dependencies:** Command structure
- **User Interaction:** Required when conflict exists
- **Resolution:** Explicit namespace required

#### Task 2.4.2: Integrate Conflict Resolution
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/run-command.sh
- **Integration:**
  ```bash
  #!/bin/bash
  
  run_claude_command() {
    local cmd="$1"
    shift
    local args="$@"
    
    # Remove leading slash
    cmd="${cmd#/}"
    
    # Check for conflicts
    if ! detect_command_conflicts "$cmd"; then
      # Conflict exists, user must specify namespace
      echo "Use namespaced version to resolve conflict"
      return 1
    fi
    
    # Find and execute command
    # ... execution logic ...
  }
  ```
- **Time Estimate:** 5 minutes
- **Dependencies:** Conflict detector
- **Error Handling:** Clear user messages
- **Documentation:** Add to user guide

#### Task 2.4.3: Create Conflict Report
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/audit-conflicts.sh
- **Features:**
  ```bash
  #!/bin/bash
  
  echo "=== Command Conflict Audit ==="
  
  # Find all commands
  all_commands=$(find ~/.claude/commands ./.claude/commands /usr/local/claude/commands \
    -name "*.md" 2>/dev/null | xargs -n1 basename | sort -u)
  
  # Check each for conflicts
  for cmd in $all_commands; do
    cmd_name="${cmd%.md}"
    locations=()
    
    [ -f "./.claude/commands/${cmd}" ] && locations+=("project")
    [ -f "$HOME/.claude/commands/${cmd}" ] && locations+=("user")
    [ -f "/usr/local/claude/commands/${cmd}" ] && locations+=("global")
    
    if [ ${#locations[@]} -gt 1 ]; then
      echo "⚠️  /${cmd_name}: ${locations[@]}"
    fi
  done
  ```
- **Time Estimate:** 2 minutes
- **Dependencies:** Command files
- **Schedule:** Run during setup
- **Output:** Conflict list with locations

### Section 2.5: Command Chaining System (15 minutes)

#### Task 2.5.1: Create Chain Parser
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/parse-chain.py
- **Implementation:**
  ```python
  #!/usr/bin/env python3
  import yaml
  import subprocess
  import sys
  
  def parse_chain(chain_file):
      """Parse YAML chain configuration"""
      with open(chain_file, 'r') as f:
          config = yaml.safe_load(f)
      
      return config.get('chain', [])
  
  def execute_chain(chain):
      """Execute command chain with operators"""
      for step in chain:
          if isinstance(step, dict):
              cmd = step.get('command')
              operator = step.get('operator', 'sequential')
              on_failure = step.get('on_failure', 'stop')
          else:
              # Simple command
              cmd = step
              operator = 'sequential'
              on_failure = 'stop'
          
          # Execute command
          result = subprocess.run(cmd, shell=True, capture_output=True)
          
          # Handle operators
          if operator == '&&' and result.returncode != 0:
              if on_failure == 'stop':
                  print(f"Chain stopped: {cmd} failed")
                  return False
              elif on_failure == 'continue':
                  continue
          
          # Pipe output to next command if needed
          if operator == '|':
              # Pass stdout to next command's stdin
              pass
      
      return True
  ```
- **Time Estimate:** 8 minutes
- **Dependencies:** Python, PyYAML
- **Operators:** &&, ||, ;, |
- **Error Handling:** Configurable per step

#### Task 2.5.2: Create Chain Command Format
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/morning-routine.md
- **YAML Format:**
  ```markdown
  # Morning Routine Chain
  
  Execute daily morning tasks in sequence
  
  CHAIN:
    - command: /health-status
      operator: sequential
    - command: /daily-standup
      operator: &&
      on_failure: continue
    - command: /research-mode on
      operator: &&
    - command: /backup-now --incremental
      operator: sequential
      on_failure: notify
  
  FALLBACK:
    - /notify "Morning routine failed"
    - /health-status --verbose
  ```
- **Time Estimate:** 5 minutes
- **Dependencies:** Chain parser
- **Benefits:** Complex workflows
- **Testing:** Test each chain

#### Task 2.5.3: Create Chain Library
- **Status:** ⬜ Not Started
- **File:** ~/.claude/chains/
- **Pre-built Chains:**
  ```yaml
  # deployment.yaml
  chain:
    - command: /backup-now
    - command: /test-all
      operator: &&
    - command: /deploy
      operator: &&
    - command: /health-check
      operator: &&
  fallback:
    - command: /unfuck-this
    - command: /notify "Deployment failed"
  
  # debug.yaml
  chain:
    - command: /god-mode
    - command: /trace-on
    - command: /run-diagnostics
    - command: /safe-mode
  ```
- **Time Estimate:** 2 minutes
- **Dependencies:** Common workflows identified
- **Sharing:** Can be version controlled
- **Documentation:** README in chains/

### Section 2.6: Testing & Documentation (10 minutes)

#### Task 2.6.1: Create Command Test Framework
- **Status:** ⬜ Not Started
- **File:** ~/.claude/tests/test-commands.sh
- **Test Coverage:**
  ```bash
  #!/bin/bash
  
  # Test all commands exist
  test_commands_exist() {
    for cmd in activate-god-mode safe-mode unfuck-this daily-standup; do
      [ -f "$HOME/.claude/commands/${cmd}.md" ] || fail "Missing: $cmd"
    done
  }
  
  # Test discovery
  test_discovery() {
    output=$(~/.claude/scripts/command-discovery.sh)
    echo "$output" | grep -q "unfuck-this" || fail "Discovery failed"
  }
  
  # Test validation
  test_validation() {
    ~/.claude/scripts/validate-input.sh "test123" "alphanumeric" || fail "Valid input rejected"
    ~/.claude/scripts/validate-input.sh "; rm -rf" "command" && fail "Dangerous input accepted"
  }
  
  # Test conflicts
  test_conflicts() {
    # Create conflict
    touch ./.claude/commands/test.md
    touch ~/.claude/commands/test.md
    
    output=$(~/.claude/scripts/detect-conflicts.sh test)
    echo "$output" | grep -q "conflict" || fail "Conflict not detected"
    
    # Cleanup
    rm -f ./.claude/commands/test.md ~/.claude/commands/test.md
  }
  
  # Test chains
  test_chains() {
    # Test simple chain
    echo "chain: ['/echo test']" | python3 ~/.claude/scripts/parse-chain.py -
  }
  ```
- **Time Estimate:** 5 minutes
- **Dependencies:** All components ready
- **CI Integration:** GitHub Actions
- **Coverage:** All new features

#### Task 2.6.2: Create Enhanced Documentation
- **Status:** ⬜ Not Started
- **File:** ~/.claude/commands/README.md
- **Sections:**
  ```markdown
  # Claude Command System v2.0
  
  ## Features
  - Command Discovery with `/help`
  - Input Validation (injection protection)
  - Conflict Resolution (namespace support)
  - YAML-based Command Chains
  - Emergency Recovery with `/unfuck-this`
  
  ## Command List
  | Command | Description | Risk |
  |---------|-------------|------|
  | `/activate-god-mode` | Full permissions | HIGH |
  | `/safe-mode` | Revert to safe | LOW |
  | `/unfuck-this` | Emergency recovery | MEDIUM |
  | `/daily-standup` | Status report | LOW |
  | `/research-mode` | Research activation | LOW |
  | `/health-status` | System health | LOW |
  | `/backup-now` | Create backup | LOW |
  | `/build-agent` | Create AI agent | MEDIUM |
  
  ## Discovery
  - `/help` - List all commands
  - `/help <command>` - Get command details
  - `/help --search <term>` - Fuzzy search
  
  ## Conflict Resolution
  When multiple versions exist:
  - `/project:command` - Use project version
  - `/user:command` - Use user version
  - `/global:command` - Use global version
  
  ## Command Chains
  Create complex workflows in YAML:
  ```yaml
  chain:
    - command: /backup-now
      operator: &&
    - command: /deploy
      operator: &&
  fallback:
    - command: /unfuck-this
  ```
  
  ## Input Validation
  All inputs are validated and sanitized:
  - Paths: Spaces and special chars escaped
  - Commands: Alphanumeric + dash only
  - Arguments: Dangerous chars escaped
  
  ## Emergency Recovery
  If anything goes wrong:
  ```bash
  /unfuck-this        # Restore last good state
  /unfuck-this 2      # Restore 2 states back
  /safe-mode          # Exit god mode immediately
  ```
  ```
- **Time Estimate:** 5 minutes
- **Dependencies:** All features complete
- **Format:** Markdown with examples
- **Maintenance:** Update with new commands

## Validation Checklist

### Pre-Implementation
- [ ] Phase 1 complete and stable
- [ ] Pass/GPG configured for /unfuck-this
- [ ] Python 3 with PyYAML for chains
- [ ] Backup system operational

### Post-Implementation
- [ ] All 9 core commands created
- [ ] Command discovery working
- [ ] Input validation prevents injection
- [ ] Conflict detection accurate
- [ ] Command chains execute properly
- [ ] /unfuck-this tested and working
- [ ] Documentation complete
- [ ] All tests passing

## Risk Assessment

### High Risks
1. **God Mode Activation**
   - Risk: Accidental system damage
   - Mitigation: Auto-revert, confirmation prompt
   - Monitor: Audit log of god mode usage
   - **NO CHANGES**: Keep as designed

2. **Command Injection**
   - Risk: Malicious input execution
   - Mitigation: Input validation with escaping
   - Testing: Injection test suite

### Medium Risks
1. **Command Conflicts**
   - Risk: Wrong command executed
   - Mitigation: Require user resolution
   - Prevention: Namespace support

2. **Chain Failures**
   - Risk: Partial execution leaves bad state
   - Mitigation: Fallback commands, /unfuck-this
   - Logging: Full chain execution log

## Success Metrics
- **Primary:** All commands executable without errors
- **Secondary:** Zero injection vulnerabilities
- **Tertiary:** Conflict resolution < 5 seconds
- **Bonus:** 10+ useful command chains created

## Integration Points
- **With Phase 1:** Uses Pass for secure storage
- **With Phase 3:** Commands trigger automation
- **With Phase 4:** Commands emit events
- **With Phase 5:** Command usage feeds learning

## Next Steps
After Phase 2 completion:
1. Run full command test suite
2. Create command cheat sheet
3. Test /unfuck-this recovery
4. Build common chain library
5. Proceed to Phase 3 (Automation)

## Notes
- Commands use .md files for portability
- $ARGUMENTS allows parameter passing
- YAML chains are more maintainable than bash
- Validation allows some chars with escaping
- Conflicts require explicit user choice
- /unfuck-this is the universal undo