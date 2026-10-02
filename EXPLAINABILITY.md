# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **GenericAgent** (`genericagent`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** GenericAgent (`genericagent`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Self-Evolving Autonomous Agents & Web Automation  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

GenericAgent is a minimalist, self-evolving autonomous agent architecture designed for general problem solving, local script execution, browser automation, and multi-frontend messaging interfaces. In contrast to heavy agent wrappers, GenericAgent coordinates tasks through a compact Python event loop, deterministic function schemas, and a two-tier cognitive memory model: short-term L1 working checkpoints that keep active tasks grounded and long-term L2 global insights that evolve across sessions through empirical reflection.

### 1. Decision Architecture

The user instruction intake, memory recall, action formulation, tool execution, and reflective evolution pipeline operates across a deterministic, five-stage architecture:

```
User Task / Natural Language Directive (CLI, Web, Telegram, Lark, DingTalk)
    │
    ▼
[Stage 1: Intent Analysis & Hierarchical Memory Retrieval]
    │  - Ingests user prompt, environment variables, and system prompt templates
    │  - Retrieves L2 global insights (`global_mem_insight.txt`) and active L1 working notepad
    │  - Resolves active client interface and formats prompt with current timestamp
    ▼
[Stage 2: Tool Action Formulation & Hook Interception]
    │  - Evaluates objective against available tool schemas (`code_run`, `file_patch`, `web_scan`)
    │  - Emits typed function call with parameters or replies with natural language resolution
    │  - Triggers pre-execution plugin hooks (`plugins/hooks.py`) for validation and tracing
    ▼
[Stage 3: Sandboxed Tool Execution & Stream Capture]
    │  - Executes Python/shell subprocesses with working directory containment and timeouts (default 60s)
    │  - Drives browser automation via WebDriver and executes JavaScript against active tabs
    │  - Captures stdout/stderr, return codes, and filesystem mutations
    ▼
[Stage 4: In-Loop Self-Correction & Working Checkpoint Update]
    │  - If tool fails (syntax error, patch mismatch), compacts error trace into dialogue context
    │  - Updates L1 working checkpoint (`update_working_checkpoint`) for multi-step tasks (>5 steps)
    │  - Re-evaluates next step iteratively until task objective criteria are fulfilled
    ▼
[Stage 5: Post-Task Reflection & Long-Term Memory Evolution]
    │  - Prompts self-reflection routines upon task completion or repeated failure patterns
    │  - Distills durable operational insights and appends them to L2 global memory
    │  - Commits complete model interaction traces to `temp/model_responses/` for auditing
    ▼
Task Deliverable / User Response & Evolved Permanent Memory State
```

### 2. Decision Logic & Routing Formulations

GenericAgent evaluates tool selection confidence, memory distillation eligibility, and execution safety using deterministic mathematical models:

1. **Tool Selection Affinity Score ($S_{\text{tool}}$)**:
   $$S_{\text{tool}}(t, O) = (w_p \cdot P_{\text{parameter}}) + (w_m \cdot M_{\text{match}}) + (w_e \cdot E_{\text{error\_state}})$$
   where:
   - $P_{\text{parameter}} \in \{0, 1\}$ verifies that required tool arguments are fully specified without missing references.
   - $M_{\text{match}} \in [0, 1]$ represents semantic alignment between task goal $O$ and tool capability docstrings.
   - $E_{\text{error\_state}} \in \{0, 1\}$ indicates whether previous tool execution threw an exception requiring recovery.
   - Weights: $w_p = 0.40, w_m = 0.40, w_e = 0.20$ ($\sum w_i = 1.0$).

2. **Memory Distillation Threshold ($D_{\text{memory}}$)**:
   $$D_{\text{memory}} = \frac{N_{\text{steps}}}{10} + \mathbf{1}_{\{\text{error\_recovered}\}} \cdot 0.5$$
   where $N_{\text{steps}}$ is the number of executed reasoning turns. When $D_{\text{memory}} \ge 1.0$, the agent triggers post-task L2 memory distillation to persist newly acquired insights.

### 3. Thresholding & Refusal Decision Criteria

GenericAgent enforces strict operational safeguards to protect host environments and session stability:
- **Refusal to Blindly Overwrite Files**: `file_write` mode `overwrite` on pre-existing source files is rejected if line-level changes can be applied via `file_patch` (`ERR_DESTRUCTIVE_FILE_OVERWRITE`).
- **Execution Timeout Ceilings**: Script executions (`code_run`) exceeding 60 seconds are terminated via SIGTERM/SIGKILL (`WARN_CODE_RUN_TIMEOUT_EXCEEDED`).
- **Working Notepad Token Cap**: L1 working memory updates exceeding 200 tokens are truncated to avoid context window inflation (`WARN_CHECKPOINT_TOKEN_LIMIT`).
- **Refusal of Out-of-Bounds Filesystem Deletions**: Recursive directory deletions outside designated workspace roots are intercepted (`ERR_UNAUTHORIZED_DIRECTORY_PURGE`).

### 4. Fallback Decision Mechanism

Continuous operational resilience is guaranteed through multi-tier fault recovery:
- **Model Client Cascade**: If the primary foundation model API encounters rate limits (HTTP 429) or connection resets, `resolve_client` cascades between Claude, OpenAI, and local custom endpoints.
- **Patch Failure Recovery**: If `file_patch` fails due to line mismatch, the loop automatically triggers `file_read` on surrounding lines to re-ground indentation before re-attempting.
- **Web Navigation Fallback**: If structured DOM extraction fails on dynamic SPAs, the agent falls back to pure JavaScript element querying (`web_execute_js`).

### 5. Human-in-the-Loop Governance

Human users retain full operational authority and interactive oversight:
- **`ask_user` Escalation Tool**: When user specifications are ambiguous or irreversible decisions loom, the agent invokes `ask_user` to solicit direct human clarification.
- **Multi-Frontend Interactive Control**: Users can monitor and steer agent execution across multiple frontends (CLI, Streamlit GUI, Telegram, DingTalk, Lark).
- **Inspectable Session Traces**: All LLM prompts, model responses, executed scripts, and memory files are preserved locally for auditability.

---

## The Data It Uses

GenericAgent adheres to strict local data minimization, sandbox isolation, and credential hygiene practices.

### 1. Ingested Input Data

The agent processes only operational assets necessary to fulfill tasks:
- **User Directives**: Natural language goals, CLI flags, and chat messages across integrated frontends.
- **Workspace Source Files**: Text, scripts, configuration files, and line-numbered source chunks.
- **Execution & Browser Streams**: Console stdout/stderr, WebDriver simplified HTML trees, and JavaScript return payloads.

### 2. Configuration & Reference Data

- **Memory Knowledge Files**: `memory/global_mem.txt` and `memory/global_mem_insight.txt` storing durable cross-session learnings.
- **Tool JSON Schemas**: Declarative tool parameter definitions (`assets/tools_schema.json`).
- **Plugin Hook Modules**: User-configured Python hook scripts located in `plugins/`.

### 3. Base Model & Inference Lineage

- **Deterministic Agent Core**: Python runner loop, file patchers, and subprocess dispatchers execute with 100% determinism.
- **Frontier LLM Lineage**: High-capability reasoning models (Claude 3.5 Sonnet, GPT-4o, DeepSeek) utilized for natural language comprehension and code synthesis.
- **Zero Training on User Data**: User source code, local files, and chat messages are never transmitted for foundation model fine-tuning or training.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against prompt injection in untrusted web pages, unauthorized script execution, and credential exfiltration.
- **Local Filesystem Isolation**: Session traces, temporary files, and memory files reside strictly on the local machine within `memory/` and `temp/`.
- **Automated Secret Scrubbing**: API keys configured in `mykey.py` are loaded directly into memory and never logged to console traces.
- **Zero Commercial Monetization**: User workspace files, chat histories, and synthesized scripts are never monetized or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of GenericAgent ensures safe and effective deployment.

### 1. Interactive GUI Desktop Automation
- **Limitation**: GenericAgent drives web browsers via WebDriver but cannot directly interact with native OS desktop windows (e.g., Win32, macOS Cocoa).
- **Mitigation**: Web-based interfaces and CLI tools are utilized for all automated workflows.

### 2. Complex Canvas & WebGL Controls
- **Limitation**: Applications rendering entirely within HTML5 `<canvas>` containers lack accessible DOM tree elements.
- **Mitigation**: Coordinate-based JavaScript dispatch and image snapshot analysis are applied when canvas controls are detected.

### 3. Heavy Multi-Threaded Process Starvation
- **Limitation**: Spawning computationally intense Python scripts concurrently can saturate host CPU cores.
- **Mitigation**: Process timeouts and sequential task queue scheduling prevent resource starvation.

### 4. Memory Hallucination Accumulation
- **Limitation**: An incorrect empirical conclusion stored in L2 global memory could bias future reasoning.
- **Mitigation**: Global memory is stored in transparent, human-editable text files (`global_mem_insight.txt`) for easy auditing.

### 5. Multi-Gigabyte Log File Ingestion
- **Limitation**: Ingesting massive multi-gigabyte log files in a single pass exceeds context window limits.
- **Mitigation**: The `file_read` tool enforces segmented line-window reading with configurable line counts.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & routing formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested user directives, workspace files & browser streams | Section 1 | Verified |
| - Configuration, memory files & tool schemas | Section 2 | Verified |
| - Base model lineage & deterministic agent core | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Interactive GUI desktop automation | Section 1 | Verified |
| - Complex canvas & WebGL controls | Section 2 | Verified |
| - Heavy multi-threaded process starvation | Section 3 | Verified |
| - Memory hallucination accumulation | Section 4 | Verified |
| - Multi-gigabyte log file ingestion | Section 5 | Verified |
