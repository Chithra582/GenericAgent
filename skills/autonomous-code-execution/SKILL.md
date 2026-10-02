---
name: "autonomous-code-execution"
description: "Evaluates and executes Python, shell, and automation scripts with sandbox timeouts and stdout capture."
license: MIT
---

# Autonomous Code Execution

## Overview
This skill executes Python scripts and terminal shell commands directly within the local workspace environment, capturing standard output, error streams, and process exit codes.

## Key Capabilities
- **Multi-Runtime Execution**: Executes Python code or shell scripts (bash, powershell) dynamically.
- **Process Supervision**: Enforces configurable execution timeouts and terminates hanging child processes.
- **Stream Capture**: Captures and formats stdout and stderr for immediate agent consumption.

## Operational Workflow
1. **Script Preparation**: Format Python code or shell commands with required imports.
2. **Environment Check**: Set working directory and resolve temporary script paths.
3. **Execution Dispatch**: Launch sub-process runner with timeout bounds.
4. **Output Parsing**: Extract exit codes and return structured console outputs to the agent loop.
