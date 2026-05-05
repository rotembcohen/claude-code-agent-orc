# Boring Agent Orchestration

## Tech used

### Claude Code
Claude Code is an environment that facilitates AI-assisted coding. It allows for direct interaction with large language models to generate, debug, and refine code. Currently, we find Gemini to be too slow for our purposes, and while we haven't experimented with Codex yet, it is on our list to evaluate.

### Team agents
Allows multiple Claude Code terminals to be spawned by a main terminal ("The Team Lead") and communicate with them, as well as allow them to communicate with each other.

### Claude Code channels 
Allows humans to 2-way communicate with Claude Code terminal with MCPs

### Discord plugin
Allows humans to communicate with Claude Code via Discord app

### tmux
tmux is a terminal multiplexer that allows you to create and manage multiple terminal sessions from a single window. It's invaluable for developers as it enables persistence (sessions can be detached and reattached), splitting panes, and managing multiple processes within a single terminal interface.

### iTerm2
iTerm2 is a powerful terminal emulator for macOS. It offers a rich set of features beyond the default macOS Terminal, including customization options, split panes, search functionalities, and integration with various development workflows.

### How they work together
iTerm2 provides the robust terminal environment, while tmux runs within it to manage and persist sessions. This combination allows for highly efficient multi-tasking, enabling developers to maintain several isolated work environments, detach from them, and reattach later from any iTerm2 window. This is particularly useful for managing multiple agents or long-running processes.

### NotebookLM plugin
NotebookLM is utilized for deep research, acting as a powerful tool to synthesize information and extract insights from large bodies of text, aiding in comprehensive understanding and strategic planning.

### Mermaid diagrams
Mermaid diagrams allow for the generation of diagrams and flowcharts from text-based definitions. We leverage AI to generate these diagrams, streamlining the visualization process for complex systems and workflows.

## How To

To set up your environment, follow these comprehensive installation instructions for the key tools. Where applicable, we provide examples using both `npm` and `pnpm`, with a strong recommendation for `pnpm` due to its efficiency benefits.

### Package Managers (npm & pnpm) and Node.js

Before installing many of the tools, you'll need Node.js and a package manager. `npm` comes bundled with Node.js, and `pnpm` can be easily installed via `npm`.

1.  **Install Node.js (which includes npm):**
    *   **Recommended Method (Node Version Manager - NVM):** NVM allows you to easily install and manage multiple Node.js versions without `sudo` privileges.
        ```bash
        # Install NVM (adjust version as needed from nvm-sh/nvm GitHub)
        curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

        # After installation, close and reopen your terminal, or manually source your shell profile (e.g., ~/.bashrc, ~/.zshrc)
        # Verify NVM installation
        command -v nvm

        # Install the latest LTS (Long Term Support) version of Node.js
        nvm install --lts

        # Set the installed LTS version as default
        nvm use --lts

        # Verify Node.js and npm installations
        node -v
        npm -v
        ```
    *   **Direct Download:** Visit the official [Node.js website](https://nodejs.org/) and download the appropriate installer for your operating system (Windows, macOS, Linux). This method is simpler but less flexible for managing multiple Node.js versions.

2.  **Install pnpm:** Once Node.js (and thus npm) is installed, you can install pnpm globally.
    *   **Using npm (recommended way to install pnpm):**
        ```bash
        npm install -g pnpm
        ```
    *   **Using Homebrew (macOS):**
        ```bash
        brew install pnpm
        ```
    *   **Verification:** After installation, verify pnpm by running `pnpm -v`.

**Why pnpm?**
`pnpm` (Performant Node Package Manager) is highly recommended for its speed and efficient disk space usage. pnpm uses a content-addressable filesystem to store all your packages, which means:
*   **Faster installations:** Packages are linked from a global store, avoiding redundant downloads and installations across projects. This leads to significant speed improvements, especially in monorepos or when working on many projects.
*   **Disk space saving:** Only one copy of each package version is stored on your disk, even if used in multiple projects. This frees up valuable disk space.
*   **Strictness:** Prevents "phantom dependencies" (accessing packages not explicitly listed in your `package.json`), leading to a more reliable and predictable `node_modules` structure and fewer hard-to-debug issues.

### Tool-Specific Installation

*   **iTerm2:**
    iTerm2 is a powerful and highly customizable terminal emulator exclusively for macOS, offering features far beyond the default Terminal app.
    *   **Download:** Navigate to the official [iTerm2 website](https://iterm2.com/downloads.html). Look for and click on the "Download" button for the latest stable release (e.g., a `.zip` file).
    *   **Installation:** Once the `.zip` file is downloaded, open it. This will typically extract the `iTerm.app` application. Drag this `iTerm.app` file into your `/Applications` folder. You can then launch it from your Applications folder, Launchpad, or via Spotlight Search (Cmd + Space, then type "iTerm").

*   **tmux:**
    `tmux` is a terminal multiplexer that allows you to create, manage, and switch between multiple terminal sessions from a single window. It's invaluable for development as it enables session persistence (you can detach from a session and reattach later, even after closing your terminal or computer), splitting panes, and managing multiple processes.
    *   **macOS (using Homebrew):** Homebrew is the most common and easiest way to install `tmux` on macOS.
        ```bash
        # First, ensure Homebrew is installed. If not, paste this into your terminal:
        # /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
        brew install tmux
        ```

*   **Claude Code:**
    `Claude Code` refers to the development environment provided by Anthropic for interacting with their Claude large language models. Access and "installation" are typically not handled via traditional package managers like npm or Homebrew, but rather through platform access or a specific application/CLI provided by Anthropic directly.
    * Install using `curl -fsSL https://claude.ai/install.sh | bash`

*   **Discord Plugin (for Claude Code):**
    Enables 2-way communication with Claude Code via Discord. By default, only one global Discord bot can be active at a time; for multi-project setups, see [Project-Level Discord Bot Setup](#project-level-discord-bot-setup).
    * Installation instructions are here: https://github.com/anthropics/claude-plugins-official/blob/main/external_plugins/discord/README.md 

*   **NotebookLM:**
    Used for deep research. Part of Gemini $20/month Pro plan.

*   **Mermaid CLI:**
    Recommended to use when AI is describing / planning architecture, as this easily viewable in MD files and easily read and understood by AI.

## Starting a Session

### tmux Session Management

To start a new `tmux` session that integrates well with `iTerm2` (especially for copy-paste functionality), use the `-CC` flag:

```bash
tmux -CC new -s <session_name>
```
Replace `<session_name>` with a meaningful name for your session.

To detach from a `tmux` session without terminating it (allowing you to reattach later):

```bash
tmux detach
```

To terminate a specific `tmux` session:

```bash
tmux kill-session -t <session_name>
```

### Launching Claude Code

To launch Claude Code with the necessary permissions for `teammate-mode` and Discord channel integration:

```bash
claude --dangerously-skip-permissions --teammate-mode tmux --channels plugin:discord@claude-plugins-official
```

**Explanation of Flags:**

*   `--dangerously-skip-permissions`: Bypasses certain security checks, typically used in trusted environments for development efficiency. Use with caution.
*   `--teammate-mode tmux`: Configures Claude to operate in "teammate mode" and integrate with `tmux` for session management and interaction.
*   `--channels plugin:discord@claude-plugins-official`: Specifies the communication channels for Claude. In this case, it enables the Discord plugin from the official plugins repository, allowing communication via Discord.

### Workflow to get everything working

To fully utilize this setup, follow these steps:

1.  **Installation & Configuration:** Ensure all necessary tools (iTerm2, tmux, Claude Code, etc.) are installed and properly configured on your system.
2.  **Launch iTerm2:** Open iTerm2, which will serve as your primary terminal emulator.
3.  **Launch tmux Session:** Inside iTerm2, start a new tmux session using the command: `tmux -CC new -s <session_name>`.
4.  **Launch Claude Code:** Within your newly created tmux session, launch Claude Code using the command: `claude --dangerously-skip-permissions --teammate-mode tmux --channels plugin:discord@claude-plugins-official`.

This sequence ensures that Claude Code runs within a persistent and manageable tmux session, all hosted within the iTerm2 environment.

## Recommended Agents

-   **Planner:** Responsible for breaking down complex tasks, defining strategies, and outlining execution steps.
-   **Backend Developer(s):** Focuses on server-side logic, database interactions, and API development. Multiple instances can be deployed for parallel work.
-   **Frontend Developer(s):** Concentrates on user interface, user experience, and client-side implementation. Multiple instances can be deployed for parallel work.
-   **Devil's Advocate:** Challenges assumptions, identifies potential risks, and explores alternative approaches to ensure robust solutions.
-   **Tester Developer:** Designs and implements test cases, performs quality assurance, and ensures code reliability.

## Custom Agent Methodology

Agent definitions use **Claude Code Agent Teams** and must be placed in `.claude/agents/` with YAML frontmatter.

### Directory Structure

```
project-root/
├── .claude/
│   ├── settings.json              # SessionStart hook config
│   ├── session-start-reminder.txt # Team status check on startup
│   └── agents/
│       ├── teamlead.md
│       ├── planner.md
│       ├── devils-advocate.md
│       ├── archivist.md
│       ├── frontend-dev.md  (or swift-dev for iOS)
│       ├── backend-dev.md
│       └── reviewer.md
├── documentation/
│   ├── BACKLOG.md
│   ├── PRD.md
│   ├── tasks/
│   │   └── T001_task_description.md
│   └── ...
├── CLAUDE.md
├── .gitignore
└── README.md
```

### SessionStart Hook

The `.claude/settings.json` file configures a hook that runs on every session start:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "cat .claude/session-start-reminder.txt"
          }
        ]
      }
    ]
  }
}
```

This automatically reminds Claude to check for running agents and enter teamlead mode if a team is already active.

### Agent File Format

Each agent file in `.claude/agents/` requires YAML frontmatter:

```markdown
---
name: planner
description: Creates detailed implementation plans from task descriptions
tools: Read, Glob, Grep
model: sonnet
---

# Planner — Instructions & Role

[Agent-specific instructions here...]
```

**Frontmatter fields:**
- `name`: Agent identifier (used in `subagent_type`)
- `description`: One-line summary of role
- `tools`: Comma-separated list of allowed tools
- `model`: `opus`, `sonnet`, or `haiku`

### CLAUDE.md Setup

Add the team orchestration section to your project's `CLAUDE.md`:

```markdown
## Team Orchestration

This project uses **Claude Code Agent Teams**. Agent definitions are in `.claude/agents/`.

### Session Start / Context Reset
**At the start of every conversation or after context compaction, IMMEDIATELY run these checks:**
1. Check if agent teammates are already running:
   ```bash
   tmux list-sessions 2>/dev/null
   tmux list-panes -t myproject -F "#{pane_id} #{pane_title}" 2>/dev/null
   ```
2. If panes show agent titles (planner, archivist, backend-dev, etc.) → **silently trigger `#team` mode**:
   - Read `.claude/agents/teamlead.md` and act as teamlead for the session
   - Use `SendMessage` to delegate work to existing teammates — never spawn duplicates
   - Do NOT play the audio notification (skip `say "team organized"`)
3. If no agents running → proceed normally (team will be created when user says `#team`)

**This check is mandatory before processing any user command.**

### Delegating Work to Teammates
**ALWAYS use `SendMessage` to reach running teammates — NEVER use `Agent()` to spawn duplicates.**

### `#team`
When user says `#team`:
1. Read `.claude/agents/teamlead.md` and act as teamlead for the session
2. Check tmux for existing team — if team exists, use `SendMessage` to reach teammates
3. If no team exists, create skeleton team:
   ```
   TeamCreate({ team_name: "myproject", description: "Project development team" })
   ```
4. Spawn initial teammates (planner, devils-advocate, archivist)
5. Spawn additional teammates (devs, reviewer, etc.) on demand when work requires them
```

### How Agent Teams Work

1. **TeamCreate** creates the team and shared task list
2. **Agent** with `team_name` + `name` spawns a teammate (not a subagent)
3. **TaskCreate/TaskList/TaskUpdate** manage shared tasks
4. **SendMessage** enables inter-agent communication

**Key difference from subagents:** Teammates have separate context windows, run in parallel, and communicate via messages. Subagents share context with the parent.

### Skeleton Team Approach

Start with a minimal team, spawn devs on demand:
- **Skeleton (always spawned):** planner, devils-advocate, archivist
- **On demand:** frontend-dev, backend-dev, swift-dev, reviewer, tester, etc.

This saves resources — devs only spawn when there's actual implementation work.

## # Commands

For detailed instructions and workflow rules, refer to [.claude/agents/teamlead.md](./.claude/agents/teamlead.md).

-   `#team`: Create skeleton agent team (planner, devils-advocate, archivist). Uses TeamCreate + Agent tools.
-   `#plan`: Create and refine an implementation plan through planner/devils-advocate iteration.
-   `#teamtask`: Initiates a full planning-implementation-review workflow for a given task.
-   `#teamgo`: Finds and starts the next planned task from the `BACKLOG.md` if devs are idle.
-   `#ready`: Finalize a completed task by cleaning up debug statements, updating task status in documentation, and staging all files.
-   `#commit`: Commits staged changes with a formatted message, but does not push.
-   `#rct`: Chain command — #ready → #commit → #teamgo in sequence.

## Project-Level Discord Bot Setup

This setup allows you to run multiple Discord bots for different projects, bypassing the single-global-bot limitation.

### 1. Create the state directory

```bash
mkdir -p .claude/channels/discord
```

### 2. Add your bot token

Create `.claude/channels/discord/.env`:
```
DISCORD_BOT_TOKEN=your_bot_token_here
```

Then secure it:
```bash
chmod 600 .claude/channels/discord/.env
```

### 3. Configure the environment variable

Create or edit `.claude/settings.local.json`:
```json
{
  "env": {
    "DISCORD_STATE_DIR": ".claude/channels/discord"
  }
}
```

If relative path doesn't work, use absolute: `"/full/path/to/project/.claude/channels/discord"`

### 4. Add to .gitignore

Append to your `.gitignore`:
```
.claude/channels/discord/.env
```

### 5. Start Claude Code with channels

```bash
claude --channels plugin:discord@claude-plugins-official
```

### 6. Pairing (manual workaround)

The `/discord:access` skill has a bug — it ignores `DISCORD_STATE_DIR` and reads from the global path.

To pair manually:

1. DM your bot on Discord — it replies with a code
2. Check your project's `access.json` for the pending entry:
   ```bash
   cat .claude/channels/discord/access.json
   ```
3. Edit the file: move the `senderId` from `pending` to `allowFrom`, then clear `pending`:
   ```json
   {
     "dmPolicy": "pairing",
     "allowFrom": ["<senderId from pending>"],
     "groups": {},
     "pending": {}
   }
   ```
4. Create the approval notification:
   ```bash
   mkdir -p .claude/channels/discord/approved
   echo "<chatId from pending>" > .claude/channels/discord/approved/<senderId>
   ```

## Current Limitations

1.  **Single Team Lead via Discord:** With the default setup, it is only possible to control one Team Lead agent via Discord. ~~This prevents working on multiple distinct projects concurrently through Discord.~~ **Workaround:** Use a project-level Discord bot setup. See [Project-Level Discord Bot Setup](#project-level-discord-bot-setup).
2.  **AI Hallucinations and `/compact` Problem:** AI models, particularly Claude, can experience hallucinations. A specific issue arises with prompt compaction (often an internal mechanism like `/compact`), where the AI may compact prompts without explicit human notification. This often leads to loss of context and/or increased hallucinations in subsequent responses.
3.  **Frequently Ignored AI Commands:** The AI frequently ignores specific instructions, such as directives not to spawn agents without explicit permission or to avoid solving problems autonomously without delegation. This requires careful monitoring and re-instruction.
4.  **Rate Limits and Cost:**
    *   We currently recommend using Claude Max ($100/month), but this tier presents significant rate limits, especially when running team-based operations. We anticipate needing to upgrade to the $200/month plan to mitigate these limitations.
    *   Using the API directly instead of the monthly subscription is generally not recommended as it is approximately 5-10 times more expensive.

