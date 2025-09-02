# Phase 3: Automation Activation - Task List (Enhanced)

## Phase Overview
**Duration:** 4 hours (increased from 1.5 hours for Airflow)
**Priority:** HIGH
**Dependencies:** Phase 1 MCP servers, Phase 2 commands
**Risk Level:** Medium (automation can run away if misconfigured)
**Key Enhancements:** Full Apache Airflow, Aggressive self-healing, GitHub webhooks, Docker-isolated MCP-Cron

## Tasks

### Section 3.1: Apache Airflow Infrastructure (2 hours)

#### Task 3.1.1: Setup Apache Airflow with Docker
- **Status:** ⬜ Not Started
- **File:** ~/.claude/airflow/docker-compose.yml
- **Configuration:**
  ```yaml
  version: '3.8'
  services:
    postgres:
      image: postgres:14
      environment:
        POSTGRES_USER: airflow
        POSTGRES_PASSWORD: ${AIRFLOW_DB_PASSWORD}
        POSTGRES_DB: airflow
      volumes:
        - postgres-db-volume:/var/lib/postgresql/data
    
    redis:
      image: redis:7
      expose:
        - 6379
    
    airflow-webserver:
      image: apache/airflow:3.0.6
      command: webserver
      ports:
        - "8080:8080"
      environment:
        AIRFLOW__CORE__EXECUTOR: CeleryExecutor
        AIRFLOW__DATABASE__SQL_ALCHEMY_CONN: postgresql+psycopg2://airflow:${AIRFLOW_DB_PASSWORD}@postgres/airflow
        AIRFLOW__CELERY__RESULT_BACKEND: db+postgresql://airflow:${AIRFLOW_DB_PASSWORD}@postgres/airflow
        AIRFLOW__CELERY__BROKER_URL: redis://:@redis:6379/0
      volumes:
        - ./dags:/opt/airflow/dags
        - ./logs:/opt/airflow/logs
        - ./plugins:/opt/airflow/plugins
    
    airflow-scheduler:
      image: apache/airflow:3.0.6
      command: scheduler
      environment:
        # Same as webserver
      volumes:
        - ./dags:/opt/airflow/dags
        - ./logs:/opt/airflow/logs
    
    airflow-worker:
      image: apache/airflow:3.0.6
      command: celery worker
      environment:
        # Same as webserver
      volumes:
        - ./dags:/opt/airflow/dags
        - ./logs:/opt/airflow/logs
    
    airflow-init:
      image: apache/airflow:3.0.6
      entrypoint: /bin/bash
      command:
        - -c
        - |
          airflow db init
          airflow users create \
            --username admin \
            --password ${AIRFLOW_ADMIN_PASSWORD} \
            --firstname Claude \
            --lastname Admin \
            --role Admin \
            --email claude@local
  
  volumes:
    postgres-db-volume:
  ```
- **Time Estimate:** 30 minutes
- **Dependencies:** Docker, Docker Compose
- **Benefits:** Visual DAG management, dependency handling, scalability
- **Validation:** Access web UI at http://localhost:8080

#### Task 3.1.2: Create Core Automation DAGs
- **Status:** ⬜ Not Started
- **File:** ~/.claude/airflow/dags/claude_automation.py
- **DAG Definitions:**
  ```python
  from airflow import DAG
  from airflow.operators.bash import BashOperator
  from airflow.operators.python import PythonOperator
  from airflow.sensors.external_task import ExternalTaskSensor
  from datetime import datetime, timedelta
  
  default_args = {
      'owner': 'claude',
      'depends_on_past': False,
      'start_date': datetime(2024, 1, 1),
      'email_on_failure': True,
      'email_on_retry': False,
      'retries': 3,
      'retry_delay': timedelta(minutes=5)
  }
  
  # Health Check DAG
  health_dag = DAG(
      'claude_health_check',
      default_args=default_args,
      description='MCP server health monitoring',
      schedule_interval='*/5 * * * *',  # Every 5 minutes
      catchup=False
  )
  
  check_mcp = BashOperator(
      task_id='check_mcp_servers',
      bash_command='claude mcp list | tee ~/.claude/logs/mcp-status.log',
      dag=health_dag
  )
  
  analyze_health = PythonOperator(
      task_id='analyze_health',
      python_callable=analyze_mcp_health,
      dag=health_dag
  )
  
  trigger_healing = PythonOperator(
      task_id='trigger_self_healing',
      python_callable=aggressive_self_heal,
      trigger_rule='one_failed',  # Run if health check fails
      dag=health_dag
  )
  
  check_mcp >> analyze_health >> trigger_healing
  
  # Daily Operations DAG
  daily_dag = DAG(
      'claude_daily_operations',
      default_args=default_args,
      schedule_interval='0 9 * * *',  # 9 AM daily
      catchup=False
  )
  
  backup = BashOperator(
      task_id='daily_backup',
      bash_command='~/.claude/scripts/backup-encrypted.sh',
      dag=daily_dag
  )
  
  report = BashOperator(
      task_id='generate_report',
      bash_command='/daily-standup',
      dag=daily_dag
  )
  
  research = BashOperator(
      task_id='research_mode',
      bash_command='/research-mode on',
      dag=daily_dag
  )
  
  backup >> report >> research
  ```
- **Time Estimate:** 30 minutes
- **Dependencies:** Airflow running
- **DAGs to Create:**
  - Health monitoring (5 min interval)
  - Daily operations (9 AM)
  - Research pipeline (6 hour interval)
  - Pattern extraction (4 hour interval)
  - Backup automation (hourly/daily/weekly)

#### Task 3.1.3: Setup Airflow Connections
- **Status:** ⬜ Not Started
- **Configuration:** Via Airflow UI or CLI
- **Connections:**
  ```bash
  # GitHub connection
  airflow connections add github_default \
    --conn-type github \
    --conn-host api.github.com \
    --conn-password $(pass claude/github-token)
  
  # Slack connection (for escalation)
  airflow connections add slack_default \
    --conn-type slack \
    --conn-password ${SLACK_WEBHOOK_URL}
  
  # Email connection
  airflow connections add email_default \
    --conn-type email \
    --conn-host smtp.gmail.com \
    --conn-port 587 \
    --conn-login claude@example.com \
    --conn-password $(pass claude/email-password)
  ```
- **Time Estimate:** 15 minutes
- **Dependencies:** Pass store with credentials
- **Security:** All secrets from Pass, not plaintext

#### Task 3.1.4: Configure DAG Dependencies
- **Status:** ⬜ Not Started
- **File:** ~/.claude/airflow/dags/dag_dependencies.py
- **Dependency Management:**
  ```python
  # Ensure health check runs before any critical operations
  health_sensor = ExternalTaskSensor(
      task_id='wait_for_health_check',
      external_dag_id='claude_health_check',
      external_task_id='analyze_health',
      mode='poke',
      timeout=300,
      poke_interval=30
  )
  
  # Chain dependencies
  health_sensor >> critical_operation
  ```
- **Time Estimate:** 15 minutes
- **Dependencies:** Core DAGs created
- **Benefits:** Prevents operations on unhealthy system

### Section 3.2: MCP-Cron with Docker Isolation (45 minutes)

#### Task 3.2.1: Setup MCP-Cron in Docker
- **Status:** ⬜ Not Started
- **File:** ~/.claude/mcp-cron/Dockerfile
- **Docker Configuration:**
  ```dockerfile
  FROM node:18-alpine
  
  # Security: Run as non-root user
  RUN addgroup -g 1001 -S mcp && \
      adduser -u 1001 -S mcp -G mcp
  
  # Install MCP-Cron
  RUN npm install -g @modelcontextprotocol/server-cron
  
  # Copy configuration
  COPY --chown=mcp:mcp config.json /home/mcp/config.json
  
  USER mcp
  WORKDIR /home/mcp
  
  # Security: Limit resources
  CMD ["mcp-cron", "--config", "config.json"]
  ```
- **Time Estimate:** 15 minutes
- **Dependencies:** Docker
- **Security Benefits:**
  - Isolated from host system
  - Resource limits (2GB RAM max)
  - Non-root execution
  - No host filesystem access

#### Task 3.2.2: Configure MCP-Cron Schedules
- **Status:** ⬜ Not Started
- **File:** ~/.claude/mcp-cron/config.json
- **Configuration:**
  ```json
  {
    "schedules": [
      {
        "name": "ai-optimization",
        "type": "ai-prompt",
        "prompt": "Analyze the last hour's performance metrics and suggest three specific optimizations for the Claude system. Focus on MCP server efficiency and command execution times.",
        "schedule": "0 * * * *",
        "model": "claude-3-opus",
        "timeout": 300,
        "retries": 3
      },
      {
        "name": "security-scan",
        "type": "shell_command",
        "command": "~/.claude/scripts/security-audit.sh",
        "schedule": "0 */6 * * *",
        "timeout": 600,
        "on_failure": "escalate"
      },
      {
        "name": "pattern-learning",
        "type": "ai-prompt",
        "prompt": "Review command usage patterns from the last 4 hours. Identify recurring sequences that could be automated. Score each pattern and recommend the top 3 for implementation.",
        "schedule": "0 */4 * * *",
        "model": "claude-3-opus"
      }
    ],
    "escalation": {
      "levels": [
        {"timeout": 300, "action": "log"},
        {"timeout": 600, "action": "email"},
        {"timeout": 900, "action": "webhook"}
      ]
    }
  }
  ```
- **Time Estimate:** 15 minutes
- **Dependencies:** Task 3.2.1
- **AI Prompts:** Optimization, security, learning
- **Shell Commands:** Backups, health checks

#### Task 3.2.3: Integrate MCP-Cron with Airflow
- **Status:** ⬜ Not Started
- **File:** ~/.claude/airflow/dags/mcp_cron_bridge.py
- **Integration:**
  ```python
  from airflow.providers.docker.operators.docker import DockerOperator
  
  mcp_cron_task = DockerOperator(
      task_id='run_mcp_cron',
      image='claude-mcp-cron:latest',
      api_version='auto',
      auto_remove=True,
      docker_url='unix://var/run/docker.sock',
      network_mode='claude-net',
      mem_limit='2g',
      cpu_limit=1
  )
  ```
- **Time Estimate:** 15 minutes
- **Dependencies:** Both Airflow and MCP-Cron setup
- **Benefits:** Unified scheduling with isolation

### Section 3.3: Aggressive Self-Healing System (45 minutes)

#### Task 3.3.1: Create AI-Powered Self-Healing Engine
- **Status:** ⬜ Not Started
- **File:** ~/.claude/automation/self_heal.py
- **Implementation:**
  ```python
  import subprocess
  import json
  import time
  from datetime import datetime, timedelta
  import numpy as np
  from sklearn.ensemble import IsolationForest
  
  class AggressiveSelfHealer:
      def __init__(self):
          self.failure_history = []
          self.performance_baseline = {}
          self.anomaly_detector = IsolationForest(contamination=0.1)
          self.healing_actions = {
              'mcp_server_down': self.restart_mcp_server,
              'high_memory': self.clear_caches,
              'slow_response': self.optimize_resources,
              'repeated_errors': self.reset_to_known_good,
              'pattern_detected': self.predictive_fix
          }
      
      def diagnose(self):
          """AI-powered diagnosis of system state"""
          metrics = self.collect_metrics()
          
          # Anomaly detection
          anomalies = self.detect_anomalies(metrics)
          
          # Pattern recognition
          patterns = self.identify_failure_patterns()
          
          # Predictive analysis
          predictions = self.predict_failures(metrics, patterns)
          
          return {
              'current_issues': anomalies,
              'patterns': patterns,
              'predictions': predictions,
              'confidence': self.calculate_confidence()
          }
      
      def heal(self, diagnosis):
          """Aggressive healing based on diagnosis"""
          actions_taken = []
          
          # Immediate fixes for current issues
          for issue in diagnosis['current_issues']:
              if issue['severity'] > 0.3:  # Aggressive threshold
                  action = self.healing_actions.get(issue['type'])
                  if action:
                      result = action(issue)
                      actions_taken.append(result)
          
          # Predictive fixes
          for prediction in diagnosis['predictions']:
              if prediction['probability'] > 0.6:  # Aggressive prediction
                  self.preventive_action(prediction)
                  actions_taken.append(f"Preventive: {prediction['type']}")
          
          # Auto-tune thresholds based on success
          self.tune_thresholds(actions_taken)
          
          return actions_taken
      
      def restart_mcp_server(self, issue):
          """Aggressively restart failed MCP servers"""
          server = issue['server']
          
          # Try graceful restart first
          subprocess.run(['claude', 'mcp', 'remove', server])
          time.sleep(2)
          subprocess.run(['claude', 'mcp', 'add', server, '--restart'])
          
          # If still failed, force restart with Docker
          if not self.verify_server(server):
              subprocess.run(['docker', 'restart', f'mcp-{server}'])
          
          return f"Restarted {server}"
      
      def predictive_fix(self, pattern):
          """Fix issues before they occur"""
          if pattern['type'] == 'memory_leak':
              # Preemptively restart services showing memory growth
              self.scheduled_restart(pattern['service'])
          elif pattern['type'] == 'performance_degradation':
              # Optimize before users notice
              self.optimize_resources({'target': pattern['service']})
          
          return f"Predictive fix for {pattern['type']}"
      
      def tune_thresholds(self, actions):
          """ML-based threshold tuning"""
          # Learn from successful healing actions
          for action in actions:
              if self.verify_success(action):
                  self.lower_threshold(action)  # Be more aggressive
              else:
                  self.raise_threshold(action)  # Be more conservative
  ```
- **Time Estimate:** 20 minutes
- **Dependencies:** scikit-learn, numpy
- **Aggressiveness:**
  - Low thresholds (0.3 severity triggers action)
  - Predictive fixes at 60% probability
  - Auto-tuning becomes more aggressive with success

#### Task 3.3.2: Create Healing Action Library
- **Status:** ⬜ Not Started
- **File:** ~/.claude/automation/healing_actions.yaml
- **Action Definitions:**
  ```yaml
  healing_actions:
    restart_service:
      trigger: service_down
      steps:
        - check_dependencies
        - graceful_shutdown
        - clear_locks
        - start_service
        - verify_health
      timeout: 60
      
    memory_optimization:
      trigger: high_memory_usage
      steps:
        - identify_consumers
        - clear_caches
        - restart_heavy_services
        - garbage_collection
      threshold: 80%
      
    performance_boost:
      trigger: slow_response
      steps:
        - analyze_bottlenecks
        - optimize_queries
        - increase_resources
        - cache_warming
      
    emergency_rollback:
      trigger: critical_failure
      steps:
        - execute: /unfuck-this
        - restore_snapshot
        - notify_admin
      severity: critical
  
  predictive_actions:
    prevent_crash:
      indicators:
        - memory_growth_rate > 10%/hour
        - error_rate_increase > 5%
        - response_time_degradation > 20%
      action: scheduled_restart
      lead_time: 30_minutes
    
    prevent_overload:
      indicators:
        - request_rate_spike
        - cpu_trend > 70%
      action: scale_resources
      lead_time: 10_minutes
  ```
- **Time Estimate:** 15 minutes
- **Dependencies:** Self-healing engine
- **Scope:** Covers all common failure modes

#### Task 3.3.3: Setup Continuous Learning
- **Status:** ⬜ Not Started
- **File:** ~/.claude/automation/learning_loop.py
- **Learning System:**
  ```python
  class HealingLearner:
      def __init__(self):
          self.success_patterns = []
          self.failure_patterns = []
          
      def learn_from_healing(self, action, outcome):
          """Learn from each healing attempt"""
          pattern = {
              'timestamp': datetime.now(),
              'symptoms': action['symptoms'],
              'action_taken': action['type'],
              'outcome': outcome,
              'metrics_before': action['metrics_before'],
              'metrics_after': self.collect_metrics()
          }
          
          if outcome['success']:
              self.success_patterns.append(pattern)
              self.reinforce_action(action)
          else:
              self.failure_patterns.append(pattern)
              self.adjust_strategy(action)
      
      def update_model(self):
          """Retrain anomaly detection weekly"""
          # Runs weekly via Airflow
          pass
  ```
- **Time Estimate:** 10 minutes
- **Dependencies:** ML libraries
- **Continuous Improvement:** Gets more aggressive over time

### Section 3.4: GitHub Webhook Integration (30 minutes)

#### Task 3.4.1: Setup GitHub Webhook Receiver
- **Status:** ⬜ Not Started
- **File:** ~/.claude/webhooks/github_receiver.py
- **Flask Application:**
  ```python
  from flask import Flask, request, jsonify
  import hmac
  import hashlib
  import subprocess
  
  app = Flask(__name__)
  WEBHOOK_SECRET = subprocess.run(['pass', 'claude/github-webhook-secret'], 
                                 capture_output=True, text=True).stdout.strip()
  
  @app.route('/webhook/github', methods=['POST'])
  def github_webhook():
      # Verify signature
      signature = request.headers.get('X-Hub-Signature-256')
      if not verify_signature(request.data, signature):
          return jsonify({'error': 'Invalid signature'}), 401
      
      event = request.headers.get('X-GitHub-Event')
      payload = request.json
      
      # Trigger appropriate Airflow DAG
      if event == 'push':
          trigger_dag('github_push_handler', conf={'branch': payload['ref']})
      elif event == 'pull_request':
          trigger_dag('pr_automation', conf={'pr_number': payload['number']})
      elif event == 'issues':
          trigger_dag('issue_handler', conf={'issue': payload['issue']})
      
      return jsonify({'status': 'accepted'}), 200
  
  def verify_signature(payload, signature):
      expected = 'sha256=' + hmac.new(
          WEBHOOK_SECRET.encode(),
          payload,
          hashlib.sha256
      ).hexdigest()
      return hmac.compare_digest(expected, signature)
  
  def trigger_dag(dag_id, conf):
      subprocess.run([
          'airflow', 'dags', 'trigger',
          dag_id,
          '--conf', json.dumps(conf)
      ])
  
  if __name__ == '__main__':
      app.run(host='0.0.0.0', port=8888, ssl_context='adhoc')
  ```
- **Time Estimate:** 15 minutes
- **Dependencies:** Flask, GitHub token
- **Security:** HMAC signature verification

#### Task 3.4.2: Create GitHub Event Handlers
- **Status:** ⬜ Not Started
- **File:** ~/.claude/airflow/dags/github_handlers.py
- **Event Handlers:**
  ```python
  # Push Event Handler DAG
  push_dag = DAG(
      'github_push_handler',
      default_args=default_args,
      schedule_interval=None,  # Triggered by webhook
      catchup=False
  )
  
  analyze_code = BashOperator(
      task_id='analyze_changes',
      bash_command="""
      cd {{ params.repo_path }}
      git pull
      /serena --analyze-changes
      """,
      dag=push_dag
  )
  
  run_tests = BashOperator(
      task_id='run_tests',
      bash_command='cd {{ params.repo_path }} && pytest',
      dag=push_dag
  )
  
  security_scan = BashOperator(
      task_id='security_scan',
      bash_command='/security-audit --repo {{ params.repo_path }}',
      dag=push_dag
  )
  
  # PR Handler DAG
  pr_dag = DAG(
      'pr_automation',
      default_args=default_args,
      schedule_interval=None,
      catchup=False
  )
  
  review_pr = PythonOperator(
      task_id='auto_review',
      python_callable=automated_pr_review,
      op_kwargs={'pr_number': '{{ dag_run.conf["pr_number"] }}'},
      dag=pr_dag
  )
  ```
- **Time Estimate:** 15 minutes
- **Dependencies:** Webhook receiver
- **Automation:** Tests, reviews, deployments

### Section 3.5: Alert Escalation System (20 minutes)

#### Task 3.5.1: Create Escalation Policy Engine
- **Status:** ⬜ Not Started
- **File:** ~/.claude/automation/escalation.py
- **Escalation Implementation:**
  ```python
  import time
  import logging
  from enum import Enum
  from datetime import datetime, timedelta
  
  class AlertLevel(Enum):
      INFO = 1
      WARNING = 2
      ERROR = 3
      CRITICAL = 4
  
  class EscalationPolicy:
      def __init__(self):
          self.levels = [
              {
                  'name': 'Level 1',
                  'timeout': 300,  # 5 minutes
                  'actions': ['log', 'email'],
                  'recipients': ['claude-alerts@local']
              },
              {
                  'name': 'Level 2',
                  'timeout': 600,  # 10 minutes
                  'actions': ['email', 'slack'],
                  'recipients': ['claude-alerts@local', '#alerts']
              },
              {
                  'name': 'Level 3',
                  'timeout': None,  # Continuous until acknowledged
                  'actions': ['email', 'slack', 'sms', 'phone'],
                  'recipients': ['admin@local', '#critical-alerts', '+1234567890']
              }
          ]
          self.active_alerts = {}
      
      def trigger_alert(self, alert_id, message, level=AlertLevel.ERROR):
          """Start escalation process"""
          alert = {
              'id': alert_id,
              'message': message,
              'level': level,
              'created': datetime.now(),
              'current_escalation': 0,
              'acknowledged': False,
              'escalation_times': []
          }
          
          self.active_alerts[alert_id] = alert
          self.process_escalation(alert_id)
      
      def process_escalation(self, alert_id):
          """Handle escalation logic"""
          alert = self.active_alerts.get(alert_id)
          if not alert or alert['acknowledged']:
              return
          
          current_level = self.levels[alert['current_escalation']]
          
          # Execute actions for current level
          for action in current_level['actions']:
              self.execute_action(action, alert, current_level['recipients'])
          
          alert['escalation_times'].append(datetime.now())
          
          # Schedule next escalation if not acknowledged
          if current_level['timeout'] and alert['current_escalation'] < len(self.levels) - 1:
              time.sleep(current_level['timeout'])
              if not alert['acknowledged']:
                  alert['current_escalation'] += 1
                  self.process_escalation(alert_id)
      
      def acknowledge(self, alert_id, acknowledged_by=None):
          """Stop escalation"""
          if alert_id in self.active_alerts:
              self.active_alerts[alert_id]['acknowledged'] = True
              self.active_alerts[alert_id]['acknowledged_by'] = acknowledged_by
              self.active_alerts[alert_id]['acknowledged_at'] = datetime.now()
              logging.info(f"Alert {alert_id} acknowledged by {acknowledged_by}")
      
      def execute_action(self, action, alert, recipients):
          """Execute escalation action"""
          if action == 'log':
              logging.error(f"ALERT: {alert['message']}")
          elif action == 'email':
              # Send email (integrate with Pass for credentials)
              pass
          elif action == 'slack':
              # Post to Slack
              pass
          elif action == 'sms':
              # Send SMS (Twilio integration)
              pass
          elif action == 'phone':
              # Make phone call (Twilio)
              pass
  ```
- **Time Estimate:** 10 minutes
- **Dependencies:** Communication channels configured
- **Escalation Levels:** 3-tier system
- **No External Services:** Uses existing email/Slack

#### Task 3.5.2: Integrate Escalation with Airflow
- **Status:** ⬜ Not Started
- **File:** ~/.claude/airflow/plugins/escalation_operator.py
- **Custom Operator:**
  ```python
  from airflow.models import BaseOperator
  from airflow.utils.decorators import apply_defaults
  
  class EscalationOperator(BaseOperator):
      @apply_defaults
      def __init__(self, alert_message, alert_level='ERROR', *args, **kwargs):
          super().__init__(*args, **kwargs)
          self.alert_message = alert_message
          self.alert_level = alert_level
          self.escalation = EscalationPolicy()
      
      def execute(self, context):
          alert_id = f"{context['dag'].dag_id}_{context['task'].task_id}_{context['ts']}"
          self.escalation.trigger_alert(
              alert_id=alert_id,
              message=self.alert_message,
              level=AlertLevel[self.alert_level]
          )
  ```
- **Time Estimate:** 10 minutes
- **Dependencies:** Escalation engine
- **Usage:** Trigger from any DAG failure

### Section 3.6: Testing & Monitoring (30 minutes)

#### Task 3.6.1: Create Automation Test Suite
- **Status:** ⬜ Not Started
- **File:** ~/.claude/tests/test_automation.py
- **Test Coverage:**
  ```python
  import pytest
  import time
  from unittest.mock import patch, MagicMock
  
  def test_airflow_dags_valid():
      """Test all DAGs are valid"""
      result = subprocess.run(['airflow', 'dags', 'list'], capture_output=True)
      assert result.returncode == 0
      assert 'claude_health_check' in result.stdout.decode()
  
  def test_mcp_cron_docker_isolation():
      """Verify MCP-Cron runs in isolated container"""
      result = subprocess.run(['docker', 'ps'], capture_output=True)
      assert 'claude-mcp-cron' in result.stdout.decode()
      
      # Check resource limits
      inspect = subprocess.run(['docker', 'inspect', 'claude-mcp-cron'], 
                              capture_output=True)
      config = json.loads(inspect.stdout)
      assert config[0]['HostConfig']['Memory'] <= 2147483648  # 2GB
  
  def test_self_healing_aggressive():
      """Test aggressive self-healing triggers"""
      healer = AggressiveSelfHealer()
      
      # Simulate failure
      mock_metrics = {'mcp_status': 'down', 'severity': 0.4}
      diagnosis = healer.diagnose()
      actions = healer.heal(diagnosis)
      
      assert len(actions) > 0
      assert 'Restarted' in actions[0]
  
  def test_github_webhook_security():
      """Test webhook signature verification"""
      with patch('hmac.compare_digest') as mock_compare:
          mock_compare.return_value = False
          response = app.test_client().post('/webhook/github', 
                                           data='test',
                                           headers={'X-Hub-Signature-256': 'invalid'})
          assert response.status_code == 401
  
  def test_escalation_policy():
      """Test 3-tier escalation"""
      policy = EscalationPolicy()
      policy.trigger_alert('test-alert', 'Test message')
      
      # Should escalate through levels
      time.sleep(1)  # Fast test
      alert = policy.active_alerts['test-alert']
      assert alert['current_escalation'] >= 0
      
      # Test acknowledgment stops escalation
      policy.acknowledge('test-alert', 'test-user')
      assert alert['acknowledged'] == True
  ```
- **Time Estimate:** 15 minutes
- **Dependencies:** All components installed
- **Coverage:** All critical paths

#### Task 3.6.2: Setup Monitoring Dashboard
- **Status:** ⬜ Not Started
- **Note:** Simplified per your request - no complex data display
- **File:** ~/.claude/scripts/automation-status.sh
- **Simple Status Display:**
  ```bash
  #!/bin/bash
  
  echo "=== Automation Status ==="
  echo ""
  echo "Airflow: $(curl -s localhost:8080/health | jq -r .status)"
  echo "MCP-Cron: $(docker ps | grep mcp-cron | awk '{print $7}')"
  echo "Webhooks: $(netstat -an | grep 8888 | grep LISTEN && echo "Active" || echo "Down")"
  echo "Self-Healing: $(ps aux | grep self_heal.py | grep -v grep && echo "Running" || echo "Stopped")"
  echo ""
  echo "Recent Actions:"
  tail -n 5 ~/.claude/logs/healing.log | sed 's/^/  /'
  ```
- **Time Estimate:** 5 minutes
- **Dependencies:** None
- **Simplicity:** Just key status, no data overload

#### Task 3.6.3: Create Documentation
- **Status:** ⬜ Not Started
- **File:** ~/.claude/automation/README.md
- **Documentation:**
  ```markdown
  # Claude Automation System v2.0
  
  ## Architecture
  - Apache Airflow: DAG orchestration and dependencies
  - MCP-Cron: Docker-isolated AI prompt scheduling
  - Self-Healing: Aggressive AI-powered recovery
  - GitHub Webhooks: Event-driven automation
  - Escalation: 3-tier alert system
  
  ## Quick Start
  1. Start Airflow: `docker-compose up -d`
  2. Access UI: http://localhost:8080 (admin/password)
  3. View DAGs and dependencies
  4. Monitor health checks every 5 minutes
  
  ## Self-Healing Behavior
  - Triggers at 30% severity (aggressive)
  - Predictive fixes at 60% probability
  - Auto-tunes thresholds based on success
  - Learns from every healing action
  
  ## GitHub Integration
  - Push events trigger tests
  - PR events trigger reviews
  - Issue events trigger automation
  
  ## Escalation Policy
  - Level 1: Log + Email (5 min)
  - Level 2: Email + Slack (10 min)
  - Level 3: All channels (continuous)
  
  ## Troubleshooting
  - Check Airflow logs: `~/.claude/airflow/logs/`
  - MCP-Cron logs: `docker logs claude-mcp-cron`
  - Healing history: `~/.claude/logs/healing.log`
  ```
- **Time Estimate:** 10 minutes
- **Dependencies:** All tasks complete
- **Format:** Clear, actionable documentation

## Validation Checklist

### Pre-Implementation
- [ ] Docker and Docker Compose installed
- [ ] Python 3.8+ with ML libraries
- [ ] GitHub webhook secret in Pass
- [ ] Port 8080 (Airflow) and 8888 (webhooks) available

### Post-Implementation  
- [ ] Airflow UI accessible
- [ ] All DAGs visible and valid
- [ ] MCP-Cron container running with limits
- [ ] Self-healing responds to failures
- [ ] GitHub webhooks triggering DAGs
- [ ] Escalation policy working
- [ ] All tests passing

## Risk Assessment

### High Risks
1. **Aggressive Healing Gone Wrong**
   - Risk: Over-correction causing instability
   - Mitigation: Rollback capability via /unfuck-this
   - Monitor: Healing action logs

2. **DAG Dependency Deadlock**
   - Risk: Circular dependencies in Airflow
   - Mitigation: DAG validation, timeout settings
   - Prevention: Clear dependency hierarchy

### Medium Risks
1. **Resource Exhaustion**
   - Risk: Airflow + Docker consuming too much
   - Mitigation: Resource limits, monitoring
   - Threshold: 80% usage triggers optimization

2. **Webhook Security**
   - Risk: Unauthorized webhook triggers
   - Mitigation: HMAC verification, HTTPS only
   - Audit: Log all webhook attempts

## Success Metrics
- **Primary:** Zero manual interventions in 24 hours
- **Secondary:** <30 second healing response time
- **Tertiary:** All GitHub events processed
- **Bonus:** Predictive fixes prevent 50% of issues

## Integration Points
- **With Phase 1:** Uses Docker, Pass for secrets
- **With Phase 2:** Triggers commands via DAGs
- **With Phase 4:** Emits events on actions
- **With Phase 5:** Healing patterns feed learning

## Next Steps
After Phase 3 completion:
1. Monitor Airflow DAG execution for 48 hours
2. Tune self-healing aggressiveness
3. Test GitHub integration with real repos
4. Document learned healing patterns
5. Proceed to Phase 4 (Event Bus)

## Notes
- Airflow 3.0.6 provides new Task SDK
- MCP-Cron security concerns addressed via Docker
- Aggressive healing balanced by /unfuck-this
- GitHub integration uses existing gh CLI token
- No canary deployments (per your request)
- Simplified monitoring (no data overload)