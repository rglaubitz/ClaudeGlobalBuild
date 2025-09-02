# Claude Configuration Project - User Preferences & Decisions

## Overall Philosophy
- **Simplicity over complexity**: User consistently chose simpler solutions
- **Security conscious**: But practical (Pass over Vault)
- **Avoid information overload**: No complex monitoring dashboards
- **Maintain user control**: Especially for AI learning features
- **Practical implementation**: Willing to invest time for right solution (Airflow)

## Phase-Specific Decisions

### Phase 1: Infrastructure
- **Secrets Management**: Pass chosen over Vault
  - Reason: 15 min vs 2-4 hour setup, simpler for single user
  - Quote: "Pass is simpler setup"
- **Platform**: macOS/Darwin environment
- **Docker**: Yes to Docker Compose for consistency
- **GitHub CLI**: Already installed (gh), use it
- **Backups**: Encrypted with Pass/GPG

### Phase 2: Commands
**Keep Unchanged:**
- God mode command - exactly as designed
- Daily standup - exactly as designed

**Add:**
- Command discovery with built-in listing
- Input validation WITH escaping (not blocking)
- Conflict resolution requiring user interference
- YAML command chains

**Explicitly Rejected:**
- Interactive command builder
- Command marketplace
- Voice commands
- Sandbox mode
- Internationalization
- Separate undo system (use /unfuck-this as universal undo)

### Phase 3: Automation
**User Wants:**
- Full Apache Airflow (despite 2+ hour setup time)
- GitHub integration ONLY (no other VCS)
- Aggressive self-healing (0.3 severity threshold)
- GitHub webhooks

**User Rejected:**
- Complex monitoring dashboards ("too much data")
- Canary deployments
- Only wants escalation policies for alerts

### Phase 4: Event Bus
**User Decision:** "You have my full blessing to make it 10/10"
- Gave full autonomy for optimization
- Trusts assistant judgment completely for this phase
- Result: NATS JetStream, CloudEvents, OpenTelemetry, chaos engineering

### Phase 5: Learning Pipeline
**Most Important to User:** This phase concerned them most
**User Wants:**
- Federated learning
- Causal inference (if built well)
- Pattern explanation
- RL optimization
- Pattern composition
- **Strong user control**: Either /command to release OR user confirmation for context additions
- Containerization for safety

### Phase 6: Validation
**User Confusion:** "I'm not even sure exactly what this phase is for"
- Needed clarification it's for monitoring and testing
- Once understood, wanted comprehensive validation

## Communication Style Preferences
- User uses lowercase, casual style
- Appreciates clear explanations of purpose
- Wants to understand what things are for
- Values practical over theoretical

## Project Management Approach
- Reviews phase by phase
- Wants to understand overlaps and dependencies
- Concerned about doing tasks twice
- Wants clear implementation strategy
- Values proper sequencing

## Key Quotes
- "lets begin reviews phase by phase"
- "i dont want that much data"
- "you have my full blessing to make it 10/10"
- "im not even sure exactly what this phase is for"
- "we will need to still deploy the original task lists and the enhancements too"

## Implementation Notes
- User discovered enhanced lists don't include all original tasks
- Wants both original AND enhanced deployed
- Needs clear strategy to avoid overlaps
- Values verification of approach before starting