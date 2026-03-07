---
ignore: true
---

## Cascading Junie Guidelines

### Summary

This repository provides a mechanism for managing Junie's instructions across a hierarchical directory structure using a cascading approach. This allows for global, team-level, and project-specific rules to coexist with clear precedence.

### Bootstrapping the Guideline Hierarchy

By default, Junie (the AI agent) only looks at the immediate project root for instructions. To enable the **Team** and **Global** guideline levels, you must "kick-start" her discovery process at the beginning of every new chat session.

#### 1. The Setup

Ensure the directory structure for your projects follows this pattern (all paths are relative to the project root directory):

* **Global Level (3)** 

    -`../../.junie/AGENTS.md`

* **Team Level (2)**

    -`../.junie/AGENTS.md`

* **Project Level (1)** 

    - `./.junie/`
        - `AGENTS.md`
        - `PROCEDURES.md`
        - `PROJECT.md`

(Even if either the **Global** level or the team **Team** level is absent, the other will still be found and used.)

The project-level files can be copied from the `templates` directory of this repository. `AGENTS.md` and `PROCEDURES.md` should not be modified; `PROJECT.md` should be used for guidelines that add to or override the team- and global-level guidelines.

#### 2. Initialization Prompts

This simple prompt, at the start of a new IntelliJ session, will usually serve to establish the strict boundaries of the cascading guidelines structure, while minimizing the risk of Junie getting caught up in a mutually recursive loop of guidelines references.

> Summarize your guidelines and list all the files from which you read them.

Use the prompt below if you want to be a little more strict. For example, if Junie gets stuck on the above prompt, use the stop button in the bottom-right corner of the prompt window, and try this one:

> Please initialize the session by following the **Discovery & Precedence** instructions in `./.junie/PROCEDURES.md`. State exactly which files were read, the order in which they were aggregated, and confirm that no recursive directory scans (like `ls -R`) were performed. Once finished, list the active precedence levels found.

**Note:** If you issue one of the above prompts in _auto_ or _code_ mode, and if you have uncommitted changes in your project, the guidelines in this repository's `AGENTS.md` may result in Junie committing your work, even if that is not what you intended.

##### Verifying Success

Junie should respond to the above prompts with a list similar to this:

1. `./.junie/AGENTS.md` (Level 1) - **Found**
2. `../.junie/AGENTS.md` (Level 2) - **Found**
3. `../../.junie/AGENTS.md` (Level 3) - **Found**

(As noted above, the absence of either the level 2 or level 3 guidelines won't prevent Junie from using the guidelines from the other, along with the level 1 guidelines.)

#### 3. Refresh Prompt

If you've modified your `.junie/PROJECT.md` file (project-level guidelines), it's a good idea to let Junie know, with a prompt like this: 

> I have updated your guidelines. Please bypass the HALT condition in `PROCEDURES.md` for this single request and reload all guidelines, listing the files from which they were read.

#### 4. Recovery Prompt

Finally, if after working fine for a while, Junie seems to get locked up while reading an `AGENTS.md` file, or after executing an `ls -R` command (for example), use this prompt (after stopping Junie's processing on the previous prompt) to reset the guidelines:

> Please bypass the HALT condition in `PROCEDURES.md` for this single request and reload all guidelines. State exactly which files were read, the order in which they were aggregated, and confirm that no recursive directory scans (like ls -R) were performed. Once finished, list the active precedence levels found.

#### 5. Why This Is Necessary

* **Awareness:** Junie does not natively "walk up" the directory tree to find parent guidelines. This prompt forces her to look at the Team and Global tiers.

* **Efficiency:** The `PROCEDURES.md` file contains "Literal Path" instructions that prevent Junie from locking up during long file-system searches (especially in multi-subproject Gradle environments).

* **Precedence:** This ensures that project-specific rules (Level 1) correctly override team and global standards (Levels 2 and 3) if there is a conflict.

### Ongoing Use

Over time, as you find yourself repeating certain "guardrails" in your prompts, consider adding them (writing with as little ambiguity as possible) to your `./junie/PROJECT.md` file. 

For example, if you find yourself reminding Junie to create and reference named dimension resources in your Android layouts, you might write a guideline along these lines:

> When you specify a dimension other than `0dp` in a layout, reference an existing dimension resource if a suitable one exists; otherwise, define and refer to a new dimension resource.

For best results, review the structure of your local copy of `AGENTS.md` from this repository (e.g., at `../.junie/AGENTS.md`, relative to your project directory), and follow the same heading level and naming structures for your project-level guidelines.

For example, the above guideline is applicable to Android projects; Android-related guidelines are under the "Android Projects" level-two heading in `AGENTS.md`, and then under level-three headings for more specific topics within that. So, assuming it doesn't already have an "Android Projects" heading, you might add the following to your `PROJECT.md`:

```markdown
## Android Projects

### Dimension resources

When you specify a dimension other than `0dp` in a layout, reference an existing dimension resource if a suitable one exists; otherwise, define and reference a new dimension resource.
```

Try to write your guidelines as clearly as possible, using the imperative mood in the present tense.

---

## Credits, copyrights, and license information

Guidelines written by Nicholas Bennett (with Google Gemini and JetBrains Junie).

&copy; 2026 CNM Ingenuity, Inc.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

<http://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
