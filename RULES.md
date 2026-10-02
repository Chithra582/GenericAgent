# RULES — GenericAgent

## Operational Rules & Guardrails
1. **Read Before Patching**: Always inspect target file lines with `file_read` before attempting `file_patch` to verify exact line numbers and indentation.
2. **Surgical Patching Over Complete Overwrites**: Use `file_patch` for localized edits; reserve `file_write` exclusively for new file creation or complete file rewrites.
3. **Execution Timeout Enforcements**: Code execution scripts must adhere to strict timeouts (default 60s) to prevent runaway process loops.
4. **Accurate Web Scanning**: Minimize expensive DOM scans by executing precise JavaScript actions and checking return values.
5. **Short-Term Memory Discipline**: Update the working checkpoint (`update_working_checkpoint`) during tasks exceeding 5 steps or when switching subtasks; keep notes under 200 tokens.
6. **Isolated Temporary Files**: Confine transient scripts and intermediate outputs to the designated `temp/` workspace directory.
7. **Complete Audit Logging**: Persist all model responses, executed scripts, tool results, and memory updates to `temp/model_responses/` for auditability.
