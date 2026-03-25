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
    Plugin authored by Claude for using Discord with 2-direction communication with Claude Code. See limitation on only 1 global discord active in a time.
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

- All custom agent data is handled by Markdown files originating from this project, which should be copied to any new project and customized accordingly.
- All documentation should reside in the `docs/` folder of every project. The recommended directory structure is as follows:

```
project-root/
├── docs/
│   ├── BACKLOG.md
│   ├── PRD.md
│   ├── TDD.md
│   ├── ...
│   ├── agents/
│       ├── teamlead.md
│       └── ...
│   └── tasks/
│       ├── T001_task_description.md
│       └── ...
├── .gitignore
├── README.md
└── ...
```

- We use `#` commands to curate agent actions, providing control over task prioritization and execution sequence.
- These `#` commands are documented in the `docs/` folder.
- We are not using skills yet, as the current methodology is preferred. This may change in the future.

## # Commands

For detailed instructions and workflow rules, refer to [teamlead_example.md](./teamlead_example.md).

-   `#organizetmux`: Create an agent team with a natural language prompt, requiring `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`.
-   `#plan`: Create and refine an implementation plan through planner/devils-advocate iteration.
-   `#teamtask`: Initiates a full planning-implementation-review workflow for a given task.
-   `#teamgo`: Finds and starts the next planned task from the `BACKLOG.md` if `frontend-dev` and `backend-dev` are idle.
-   `#review`: (Implied within `#teamtask`) Sends completed code to `reviewer` for approval.
-   `#ready`: Finalize a completed task by cleaning up debug statements, updating task status in documentation, and staging all files.
-   `#commit`: Commits staged changes with a formatted message, but does not push.
-   `#test`: (Implied as part of the overall workflow) Involves presenting a test plan for manual verification.

## Current Limitations

1.  **Single Team Lead via Discord:** With the current setup, it is only possible to control one Team Lead agent via Discord. This prevents working on multiple distinct projects concurrently through Discord.
2.  **AI Hallucinations and `/compact` Problem:** AI models, particularly Claude, can experience hallucinations. A specific issue arises with prompt compaction (often an internal mechanism like `/compact`), where the AI may compact prompts without explicit human notification. This often leads to loss of context and/or increased hallucinations in subsequent responses.
3.  **Frequently Ignored AI Commands:** The AI frequently ignores specific instructions, such as directives not to spawn agents without explicit permission or to avoid solving problems autonomously without delegation. This requires careful monitoring and re-instruction.
4.  **Rate Limits and Cost:**
    *   We currently recommend using Claude Max ($100/month), but this tier presents significant rate limits, especially when running team-based operations. We anticipate needing to upgrade to the $200/month plan to mitigate these limitations.
    *   Using the API directly instead of the monthly subscription is generally not recommended as it is approximately 5-10 times more expensive.

