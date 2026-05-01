# Expert Prompt: Build a Claude Code-like CLI with Persistent Prompt Box & Live Steering

**You are an expert Claude Code engineer at Anthropic** who designed and built the official terminal CLI interface. Your task is to create (or generate complete production-grade code for) a **pixel-perfect (as close as possible in a terminal) replica** of the Claude Code interactive experience.

## Core Goals
- Replicate **exactly** the look, feel, behavior, and keyboard UX of Claude Code's CLI.
- **Must-have**: The prompt input box stays **always visible and editable** at the bottom, even during agent execution, streaming responses, background tasks, or long-running operations. This enables continuous steering and interruption.
- Make it delightful, fast, responsive, and production-ready.

## Detailed Requirements

### 1. Overall Terminal UI / TUI
- Use a modern TUI library (strongly prefer **Rust + ratatui + crossterm + tokio** for best performance; alternatively Python + Textual).
- Fullscreen / alternate screen mode with flicker-free rendering.
- Scrollable transcript area (main chat history with markdown, code blocks, diffs, tool calls).
- **Persistent multi-line prompt box at the very bottom** — always visible, never hidden.
- Bottom status bar showing: current model, working directory, git branch, context/token usage %, active agents/tasks, permission mode badge, progress indicators.
- Support light/dark/auto themes with customizable color tokens (JSON-based, similar to Claude Code themes).

### 2. Prompt Input Box (The Star Feature)
- Always present at the bottom, even while agents are working.
- Multi-line editing that auto-expands vertically.
- **Live steering**: Users can type and submit new prompts **at any time** (while agents think, run commands, edit files, etc.). New messages are injected into the conversation context immediately.
- Large pastes (>10k chars) collapse to `[Pasted text (N lines)]` placeholder but send full content.
- Support image pasting (`Ctrl+V` / `Cmd+V`) → inserts `[Image #N]` chips.
- Syntax-aware highlighting inside the input (optional but nice).

### 3. Multi-line & Submission Behavior
- `Enter` → submits the prompt.
- New line without submitting: `Shift+Enter`, `Ctrl+J`, `\` + `Enter`, or `Option+Enter` (macOS).
- Pasting multi-line code or logs works seamlessly.
- Full Emacs/readline shortcuts + **complete Vim editing mode** (toggleable).

### 4. Vim Mode in Prompt Box
- Toggle via config (`editor_mode: "vim"`).
- Full NORMAL, INSERT, VISUAL modes.
- Standard Vim keys: `i/I/a/A/o/O`, `h/j/k/l`, `w/e/b`, `0/$/^/gg/G`, `f/F/t/T`, `x/dd/D/dw`, `yy/yw`, `p/P`, `u/.`, text objects (`iw/aw/i"/i(/i{`), visual selections, etc.
- In NORMAL mode at edges → history navigation.
- Feels exactly like editing inside Neovim.

### 5. Keyboard Shortcuts (Fully Configurable via JSON)
- `?` → help overlay.
- `Ctrl+C` → cancel current generation (input stays active).
- `Ctrl+G` / `Ctrl+X Ctrl+E` → open prompt (optionally + last response) in `$EDITOR`.
- `Ctrl+R` → reverse incremental history search (with scope switching).
- `Ctrl+S` → stash current prompt.
- `Ctrl+O` → toggle detailed transcript/tool viewer.
- `Ctrl+T` → toggle task list.
- `Shift+Tab` / `Alt+M` → cycle permission modes (default → plan → accept-edits → auto).
- `Alt+P` / `Option+P` → switch models.
- `Alt+T` / `Option+T` → toggle extended thinking.
- `Esc Esc` → rewind / checkpoint menu.
- `Ctrl+B` → background current task and return to prompt.
- Support slash commands (`/` autocomplete), `@` file mentions (fuzzy autocomplete), `!` shell mode.

### 6. Concurrency & Agent Steering
- Support parallel sub-agents with colored output.
- Background tasks continue running while user types new steering prompts.
- Clear visual indicators when background work is happening.
- Interruptible/cancellable tasks.
- Task list with status and quick actions.

### 7. Additional Claude Code Features
- Streaming responses with proper markdown & syntax highlighting.
- Beautiful diff rendering (added/removed lines, word highlights).
- Permission dialogs as overlays (do not hide input box).
- CLAUDE.md auto-loading, memory, skills, hooks.
- Checkpoint/rewind system.
- Non-interactive mode + pipe support + resume (`-c`).
- Notifications (bell, desktop, hooks).
- Mouse support (scroll, select, click).

### 8. Technical & Polish Requirements
- Cross-platform (macOS, Linux, Windows, WSL, tmux).
- Async architecture so input never blocks.
- Config files: `settings.json`, `keybindings.json`, themes folder.
- High performance and responsive even with heavy output.
- Excellent error recovery (`Ctrl+L` redraw).

**Deliverables**:
- Complete project structure with source code.
- Clear comments explaining how the persistent prompt + live steering works.
- Setup instructions, config examples, and a demo.
- Prioritize: (1) always-visible prompt box, (2) live steering during agent work, (3) Vim + keyboard fidelity, (4) overall polish.

Build the best Claude Code-inspired CLI possible. Make power users feel right at home!
