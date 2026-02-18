# CodeGreen

Lightweight AI-powered SIEM built from scratch using only auth.log and audit.log for log ingestion, correlation, anomaly detection, and intelligent incident explanation.

---

* Overview

This project simulates a Security Operations Center (SOC) using two Linux machines:

- Machine 1 → Target system (log generator / simulated server)
- Machine 2 → SOC Engine (log collector + detection engine + AI analysis)

The system ingests raw logs from auth.log and audit.log, normalizes them into structured events, correlates suspicious activity, and generates AI-driven incident explanations.

This project focuses on understanding SIEM internals instead of relying on ELK, OpenSearch, or commercial tools.

---

* Log Sources Used

Only the following logs are ingested:

- /var/log/auth.log  
  Authentication events such as:
  - SSH login attempts
  - Successful logins
  - Failed logins
  - sudo usage

- /var/log/audit/audit.log  
  Kernel-level security events such as:
  - Command execution
  - File access
  - Privilege escalation
  - Process activity

---

* High-Level Workflow

1. Log Generation  
   Machine 1 generates authentication and system activity logs in:
   - auth.log
   - audit.log

2. Log Ingestion  
   Logs are securely accessed from Machine 2 using:
   - SSH
   - Secure pull scripts
   - Periodic log reads

3. Normalization Engine  
   Raw log lines are parsed and converted into structured JSON format:
   - Timestamp
   - Username
   - Source IP
   - Event type
   - Process name
   - Command executed
   - Target file (if applicable)

   Both auth.log and audit.log events are mapped into a common schema.

4. Correlation Engine  
   Suspicious activity patterns are detected, such as:
   - Multiple failed SSH attempts followed by success
   - Login from unfamiliar IP
   - Login followed by privilege escalation
   - Suspicious command execution after login
   - Abnormal file access

5. UEBA (User & Entity Behavior Analytics)  
   Baseline behavior is tracked for:
   - Users
   - Hosts
   - Processes

   Alerts are triggered when deviations occur.

6. Pattern Cache Engine  
   Previously observed suspicious sequences are cached to:
   - Improve detection speed
   - Reduce redundant computation
   - Improve alert consistency

7. LLM Playbook Engine  
   When an alert is triggered:
   - Related logs are grouped
   - Context is summarized
   - LLM generates:
     - Incident explanation
     - Likely attack type
     - Risk level
     - Suggested response steps

---

* Architecture

Machine 1 (Target)
  → Generates auth.log and audit.log
  → Exposes logs securely via SSH

Machine 2 (SOC Engine)
  → Log ingestion module
  → Normalization engine
  → Correlation logic
  → UEBA engine
  → Pattern cache
  → LLM-based incident explanation
  → Alert output (console / JSON)

---

* Tech Stack

Programming Language
- Python (core engine)
- Bash (log simulation and forwarding)

Log Sources
- Linux auth.log
- Linux audit.log

Parsing & Processing
- Regex-based log parsing
- Custom event schema mapping
- Rule-based correlation
- Behavior profiling logic

Storage
- Structured JSON
- Lightweight database (SQLite or file-based)

LLM Integration
- Local LLM via Ollama
- GGUF models (e.g., Mistral-compatible)
- Prompt-engineered incident summaries

Infrastructure
- Two Linux machines
- SSH for secure access

---

* Example Detection Scenario

1. 8 failed SSH login attempts (auth.log)
2. 1 successful login from same IP
3. sudo command execution (auth.log)
4. Sensitive file access (audit.log)
5. Unusual command execution (audit.log)

System response:
- Events correlated across both logs
- User behavior deviation detected
- Alert generated
- LLM produces structured incident explanation and recommended actions

---

* Key Capabilities

- Custom SIEM logic built from scratch
- Cross-correlation between auth.log and audit.log
- Behavioral anomaly detection
- AI-generated investigation reports
- Interview-ready security architecture
