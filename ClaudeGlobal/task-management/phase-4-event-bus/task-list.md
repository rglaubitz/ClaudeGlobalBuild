# Phase 4: Event Bus & Resilience - Task List

## Phase Overview
**Duration:** 1.5 hours
**Priority:** HIGH
**Dependencies:** Phases 1-3 operational
**Risk Level:** Medium (event loops and state management complexity)

## Tasks

### Section 4.1: Event Bus Infrastructure (45 minutes)

#### Task 4.1.1: Create Event Bus Core
- **Status:** ⬜ Not Started
- **File:** ~/.claude/event-bus/event-bus.py
- **Architecture:**
  ```python
  class EventBus:
      def __init__(self):
          self.subscribers = defaultdict(list)
          self.event_queue = Queue()
          self.dead_letter_queue = Queue()
          self.circuit_breakers = {}
      
      def publish(self, event_type, payload):
          # Async event publishing with retry
      
      def subscribe(self, event_type, handler):
          # Register event handlers
  ```
- **Time Estimate:** 10 minutes
- **Dependencies:** Python asyncio
- **Features:**
  - Async event processing
  - Priority queue support
  - Event replay capability
  - Dead letter queue

#### Task 4.1.2: Implement State Machine
- **Status:** ⬜ Not Started
- **File:** ~/.claude/event-bus/state-machine.py
- **State Definitions:**
  ```yaml
  States:
    - INITIALIZING: System starting up
    - HEALTHY: All systems operational
    - DEGRADED: Some services down
    - RECOVERING: Attempting recovery
    - FAILED: Critical failure
    - MAINTENANCE: Planned downtime
  
  Transitions:
    INITIALIZING -> HEALTHY: All checks pass
    HEALTHY -> DEGRADED: Service failure
    DEGRADED -> RECOVERING: Auto-recovery triggered
    RECOVERING -> HEALTHY: Recovery successful
    ANY -> MAINTENANCE: Manual trigger
  ```
- **Time Estimate:** 10 minutes
- **Dependencies:** Event bus core
- **Persistence:** State saved to ~/.claude/state/current.json
- **History:** Last 100 transitions logged

#### Task 4.1.3: Create Circuit Breaker System
- **Status:** ⬜ Not Started
- **File:** ~/.claude/event-bus/circuit-breaker.py
- **Configuration:**
  ```python
  class CircuitBreaker:
      def __init__(self, failure_threshold=5, 
                   timeout=60, recovery_timeout=30):
          self.failure_count = 0
          self.last_failure = None
          self.state = "CLOSED"  # CLOSED, OPEN, HALF_OPEN
      
      def call(self, func, *args, **kwargs):
          # Execute with circuit breaker protection
  ```
- **Time Estimate:** 8 minutes
- **Dependencies:** None
- **States:**
  - CLOSED: Normal operation
  - OPEN: Blocking calls (too many failures)
  - HALF_OPEN: Testing recovery
- **Per-Service Configuration:** Each MCP server gets own breaker

#### Task 4.1.4: Setup Event Routing Rules
- **Status:** ⬜ Not Started
- **File:** ~/.claude/event-bus/routing-rules.json
- **Event Types:**
  ```json
  {
    "routing": {
      "mcp.server.down": ["alert", "recovery", "log"],
      "command.executed": ["analytics", "learning", "log"],
      "pattern.learned": ["evaluation", "storage", "notification"],
      "error.critical": ["alert", "recovery", "kill-switch"],
      "backup.completed": ["verification", "log", "cleanup"]
    }
  }
  ```
- **Time Estimate:** 7 minutes
- **Dependencies:** Event bus core
- **Priority Levels:** CRITICAL > HIGH > NORMAL > LOW
- **Filtering:** Regex-based event filtering

#### Task 4.1.5: Implement Event Persistence
- **Status:** ⬜ Not Started
- **File:** ~/.claude/event-bus/persistence.py
- **Storage Backend:** SQLite
- **Schema:**
  ```sql
  CREATE TABLE events (
      id INTEGER PRIMARY KEY,
      timestamp DATETIME,
      event_type VARCHAR(255),
      payload JSON,
      status VARCHAR(50),
      retry_count INTEGER
  );
  ```
- **Time Estimate:** 6 minutes
- **Dependencies:** sqlite3
- **Retention:** 30 days rolling window
- **Indexing:** On event_type and timestamp

#### Task 4.1.6: Create Event Replay System
- **Status:** ⬜ Not Started
- **File:** ~/.claude/event-bus/replay.py
- **Features:**
  - Replay events from timestamp
  - Filter by event type
  - Replay at different speeds
  - Dry-run mode
- **Time Estimate:** 4 minutes
- **Dependencies:** Event persistence
- **Use Cases:**
  - Debugging issues
  - Testing new handlers
  - Recovery from failures

### Section 4.2: Resilience Patterns (30 minutes)

#### Task 4.2.1: Implement Retry Logic
- **Status:** ⬜ Not Started
- **File:** ~/.claude/resilience/retry-handler.py
- **Strategy:**
  ```python
  class RetryHandler:
      strategies = {
          "exponential": lambda n: 2 ** n,
          "linear": lambda n: n * 5,
          "fibonacci": lambda n: fib(n),
          "fixed": lambda n: 10
      }
      
      def retry_with_backoff(self, func, strategy="exponential",
                            max_retries=5, max_delay=300):
          # Intelligent retry with jitter
  ```
- **Time Estimate:** 8 minutes
- **Dependencies:** None
- **Jitter:** Random 0-25% added to prevent thundering herd
- **Dead Letter:** After max retries, send to DLQ

#### Task 4.2.2: Create Bulkhead Pattern
- **Status:** ⬜ Not Started
- **File:** ~/.claude/resilience/bulkhead.py
- **Resource Isolation:**
  ```python
  class Bulkhead:
      def __init__(self, max_concurrent=10, 
                   queue_size=50, timeout=30):
          self.semaphore = Semaphore(max_concurrent)
          self.queue = Queue(maxsize=queue_size)
      
      async def execute(self, func, *args, **kwargs):
          # Isolated execution with limits
  ```
- **Time Estimate:** 7 minutes
- **Dependencies:** asyncio
- **Per-Service Limits:** Each MCP server gets resource pool
- **Monitoring:** Track rejections and timeouts

#### Task 4.2.3: Setup Timeout Management
- **Status:** ⬜ Not Started
- **File:** ~/.claude/resilience/timeout-manager.py
- **Timeout Tiers:**
  ```yaml
  Timeouts:
    critical_path: 5s
    normal_operations: 30s
    batch_processing: 5m
    research_tasks: 30m
  ```
- **Time Estimate:** 5 minutes
- **Dependencies:** asyncio
- **Cascading:** Child timeouts < parent timeout
- **Grace Period:** 10% buffer before hard kill

#### Task 4.2.4: Implement Fallback Mechanisms
- **Status:** ⬜ Not Started
- **File:** ~/.claude/resilience/fallback.py
- **Fallback Chain:**
  ```python
  fallbacks = {
      "mcp.filesystem": ["local_fs", "cached_fs", "readonly_fs"],
      "mcp.github": ["git_cli", "local_git", "cached_data"],
      "web_search": ["cached_search", "local_docs", "error_message"]
  }
  ```
- **Time Estimate:** 6 minutes
- **Dependencies:** None
- **Cache Strategy:** LRU cache with 1-hour TTL
- **Degraded Mode:** Automatic when fallback active

#### Task 4.2.5: Create Health Check Framework
- **Status:** ⬜ Not Started
- **File:** ~/.claude/resilience/health-checks.py
- **Check Types:**
  - Liveness: Is service running?
  - Readiness: Can service handle requests?
  - Startup: Is service initialized?
- **Time Estimate:** 4 minutes
- **Dependencies:** None
- **Probe Intervals:**
  - Liveness: 30s
  - Readiness: 10s
  - Startup: 5s during first 2 minutes

### Section 4.3: Integration & Testing (15 minutes)

#### Task 4.3.1: Create Event Bus CLI
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/event-cli.sh
- **Commands:**
  ```bash
  event-cli publish <type> <payload>
  event-cli subscribe <type> <handler>
  event-cli replay --from <timestamp>
  event-cli stats [--live]
  event-cli breaker <service> [open|close|reset]
  ```
- **Time Estimate:** 5 minutes
- **Dependencies:** Event bus running
- **Output:** JSON or formatted table
- **Interactive Mode:** REPL for testing

#### Task 4.3.2: Setup Event Monitoring Dashboard
- **Status:** ⬜ Not Started
- **File:** ~/.claude/scripts/event-monitor.sh
- **Display:**
  ```
  ┌─────────────────────────────────────┐
  │     Event Bus Monitor v1.0          │
  ├─────────────────────────────────────┤
  │ Events/sec:    ████████░░ 142       │
  │ Queue Depth:   ██░░░░░░░░ 23        │
  │ Circuit Status:                      │
  │   filesystem:  ● CLOSED              │
  │   github:      ● CLOSED              │
  │   memory:      ⚠ HALF_OPEN          │
  │ State: HEALTHY                       │
  └─────────────────────────────────────┘
  ```
- **Time Estimate:** 5 minutes
- **Dependencies:** tmux/screen
- **Refresh:** Real-time with 1s updates
- **Alerts:** Visual + audio on state change

#### Task 4.3.3: Create Resilience Test Suite
- **Status:** ⬜ Not Started
- **File:** ~/.claude/tests/test-resilience.sh
- **Test Scenarios:**
  - Service failures
  - Network partitions
  - Resource exhaustion
  - Cascading failures
  - Recovery procedures
- **Time Estimate:** 3 minutes
- **Dependencies:** All resilience patterns
- **Chaos Engineering:** Controlled failure injection
- **Report:** Success rate and recovery times

#### Task 4.3.4: Document Event Architecture
- **Status:** ⬜ Not Started
- **File:** ~/.claude/event-bus/README.md
- **Contents:**
  - Event flow diagrams
  - State machine diagram
  - Event type catalog
  - Handler registration guide
  - Troubleshooting procedures
- **Time Estimate:** 2 minutes
- **Dependencies:** All tasks complete
- **Format:** Mermaid diagrams + descriptions
- **Examples:** Common patterns and anti-patterns

## Validation Checklist

### Pre-Implementation
- [ ] Python 3.8+ installed
- [ ] SQLite available
- [ ] Phases 1-3 stable
- [ ] Sufficient memory for queues

### Post-Implementation
- [ ] Event bus processes events
- [ ] State machine transitions work
- [ ] Circuit breakers trip correctly
- [ ] Retry logic functions
- [ ] Dashboard displays accurately
- [ ] All tests pass

## Risk Assessment

### High Risks
1. **Event Loop Deadlock**
   - Risk: Circular dependencies cause deadlock
   - Mitigation: Timeout all operations
   - Monitor: Deadlock detection

2. **Memory Leak in Queue**
   - Risk: Unbounded queue growth
   - Mitigation: Queue size limits, TTL
   - Monitor: Memory usage alerts

### Medium Risks
1. **Event Storm**
   - Risk: Too many events overwhelm system
   - Mitigation: Rate limiting, sampling
   - Monitor: Queue depth alerts

2. **State Corruption**
   - Risk: Invalid state transitions
   - Mitigation: State validation, rollback
   - Monitor: State history audit

## Success Metrics
- **Primary:** 99.9% event delivery rate
- **Secondary:** <100ms event latency (p95)
- **Tertiary:** Zero deadlocks in 24h
- **Bonus:** 50% reduction in cascade failures

## Integration Points
- **With Phase 3:** Receives automation events
- **With Phase 5:** Sends learning events
- **With Phase 6:** Included in validation
- **With Commands:** Command events published

## Next Steps
After Phase 4 completion:
1. Load test event bus capacity
2. Tune circuit breaker thresholds
3. Optimize event routing rules
4. Create event pattern library
5. Proceed to Phase 5 (Learning)

## Notes
- Consider RabbitMQ/Kafka for production scale
- Event sourcing provides audit trail
- CQRS pattern for read/write separation
- Consider distributed tracing (OpenTelemetry)
- Plan for event schema evolution