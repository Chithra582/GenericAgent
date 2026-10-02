---
name: "file-manipulation-patching"
description: "Reads, surgically patches, and writes files with line-level accuracy and diff validations."
license: MIT
---

# File Manipulation and Patching

## Overview
This skill implements high-precision filesystem modifications, favoring surgical string replacements with indentation matching over destructive full-file overwrites.

## Key Capabilities
- **Line-Numbered Inspection**: Reads targeted file segments with line number annotations.
- **Surgical Text Patching**: Replaces unique text blocks accurately, verifying context stability.
- **Bulk Authoring**: Creates new files and appends structured content using dynamic templates.

## Operational Workflow
1. **Context Inspection**: Read target file lines using `file_read` to verify current contents and indentation.
2. **Patch Formulation**: Author unique `old_content` and replacement `new_content` strings.
3. **Patch Application**: Execute `file_patch` and verify substitution success.
4. **Validation**: Re-read modified lines if necessary to confirm clean file state.
