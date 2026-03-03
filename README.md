---
ignore: true
---
# Cascading Guidelines for Junie

This repository provides a mechanism for managing Junie's instructions across a hierarchical directory structure using a cascading approach. This allows for global, team-level, and project-specific rules to coexist with clear precedence.

## Overview

Junie automatically aggregates instructions from `.junie/AGENTS.md` files found in the project root and up to two parent directories. This hierarchy ensures that specialized project rules can override or extend broader organizational standards.

Files deeper in the directory structure (closer to the project) take precedence over those higher up. This search is limited to **two steps** above the current project directory to ensure performance and prevent the accidental inclusion of unrelated system-level guidelines.

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

To maintain consistency and clarify precedence for both Junie and other developers, every project's `.junie/AGENTS.md` should begin with the standard 3-line header found in `AGENTS.md.template`.

1. Copy the contents of `AGENTS.md.template`.
 
2. Paste it at the very beginning of your project's `.junie/AGENTS.md`.

### 3. Usage and Conflict Resolution

The system defaults to **Brave Mode**, meaning Junie will automatically resolve conflicts using the "deepest wins" precedence logic without pausing for confirmation.

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
