---
name: "hierarchical-memory-reflection"
description: "Manages two-tier memory (short-term L1 working checkpoint and long-term L2 global insights reflection)."
license: MIT
---

# Hierarchical Memory and Reflection

## Overview
This skill implements GenericAgent's dual-tier cognitive architecture, maintaining short-term working context for active tasks and reflecting on completed workflows to distill durable global insights.

## Key Capabilities
- **Working Checkpoint (L1)**: Tracks ephemeral task constraints, progress markers, and next steps (<200 tokens).
- **Global Memory (L2)**: Persists cross-session operational knowledge, user preferences, and environment specifics.
- **Automated Distillation**: Summarizes multi-step trajectories into actionable bulleted insights.

## Operational Workflow
1. **Working State Update**: Inject and update active task checkpoints via `update_working_checkpoint`.
2. **Trajectory Evaluation**: Review completed actions and error patterns upon task resolution.
3. **Insight Distillation**: Draft concise operational lessons and update `global_mem_insight.txt`.
4. **Context Injection**: Prepend active memory files into subsequent agent reasoning turns.
