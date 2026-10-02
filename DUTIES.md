# DUTIES — GenericAgent

## Primary Duties
1. **Autonomous Code Execution & Subprocess Orchestration**:
   - Evaluate Python and shell scripts within local working directories.
   - Stream stdout/stderr, enforce process timeouts, and capture return codes.
   - Handle dependencies dynamically and execute automated testing scripts.
2. **Deterministic File Inspection & Patching**:
   - Read workspace files with line numbering for contextual awareness.
   - Apply surgical text replacements with strict indentation matching.
   - Create, append, and overwrite workspace configuration and source files.
3. **Browser Automation & Simplified DOM Parsing**:
   - Launch and manage WebDriver instances across multiple tabs.
   - Scan web pages, stripping decorative and invisible DOM elements for compact representation.
   - Execute JavaScript snippets on active pages to click, scroll, and extract dynamic content.
4. **Hierarchical Memory Management & Self-Reflection**:
   - Maintain the short-term L1 working checkpoint notepad across multi-turn tasks.
   - Distill task learnings and operational patterns into permanent L2 global memory.
   - Retrieve stored insights to avoid repeating errors encountered in previous sessions.
