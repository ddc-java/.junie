---
ignore: true
---
# Cascading Guidelines for Junie

This repository provides a mechanism for managing Junie's instructions across a hierarchical directory structure using a cascading approach. This allows for global, team-level, and project-specific rules to coexist with clear precedence.

## Overview

When working in a project, Junie aggregates instructions from multiple `.junie/AGENTS.md` files:

1. The project-specific guidelines (`./.junie/AGENTS.md`).
 
2. The parent directory guidelines (`../.junie/AGENTS.md`).

3. The grandparent directory guidelines (`../../.junie/AGENTS.md`).

This hierarchy ensures that specialized project rules can override or extend broader organizational standards.

## Getting Started

### 1. Repository Structure

To implement this system, organize your projects in a nested structure. For example:
- `org-root/` (Organizational standards)
  - `.junie/AGENTS.md`
  - `team-alpha/` (Team-specific standards)
    - `.junie/AGENTS.md`
    - `project-x/` (Project-specific standards)
      - `.junie/AGENTS.md`

If you don't have as deeply nested a structure as shown above&mdash;e.g., extending only one level closer to the root than the current project&mdash;that's fine: Junie will still find the parent guidelines.

### 2. Bootstrapping a Project

To enable Junie to "look upward" for guidelines, you must include the bootstrap instructions at the top of your project's `.junie/AGENTS.md` file.

1. Locate the `AGENTS.md.template` file in this repository.
 
2. Copy its contents.
 
3. Paste it at the very beginning of your project's `.junie/AGENTS.md`.

### 3. Usage and Conflict Resolution

Junie handles conflicting rules (e.g., "Use tabs" in the root vs. "Use spaces" in the project) based on your current mode:

*   **Brave Mode:** Junie automatically applies the precedence logic (deeper files override higher files).

*   **Standard Mode:** Junie will detect the conflict and ask if you want to apply the standard precedence logic for the duration of the request or resolve the conflict manually.

## Directory Search Limits

The search for ancestor guidelines is limited to **two steps** above the current project directory to ensure performance and prevent the accidental inclusion of unrelated system-level guidelines.

## Best Practices

- **Granularity:** Keep root-level guidelines broad (e.g., license headers, general naming conventions).

- **Specificity:** Use project-level guidelines for technical specifics (e.g., library versions, specific architectural patterns).

- **Clarity:** When overriding a rule, explicitly state that it supersedes the parent rule to help Junie identify the intent.

---

## Credits, copyrights, and license information

Guidelines written by Nicholas Bennett (with Google Gemini and JetBrains Junie).

&copy; 2026 CNM Ingenuity, Inc.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

<http://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
