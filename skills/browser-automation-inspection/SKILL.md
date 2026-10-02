---
name: "browser-automation-inspection"
description: "Controls live browser instances via WebDriver, scanning simplified DOM trees and executing JavaScript."
license: MIT
---

# Browser Automation and Inspection

## Overview
This skill connects GenericAgent to live web browsers via WebDriver, parsing complex webpages into clean, simplified HTML representations and driving interactions through native JavaScript execution.

## Key Capabilities
- **Simplified DOM Scanning**: Filters out hidden, offscreen, and clutter elements to produce compact representations.
- **Tab Management**: Switches between browser tabs and monitors page load mutations.
- **Direct JS Execution**: Executes custom JavaScript scripts to click, type, and extract data.

## Operational Workflow
1. **Target Navigation**: Open or switch to the target browser tab.
2. **Page Scanning**: Scan the page using `web_scan` to retrieve the simplified interactive layout.
3. **Action Execution**: Execute JavaScript DOM actions via `web_execute_js`.
4. **State Verification**: Monitor DOM mutations to verify successful state transitions.
