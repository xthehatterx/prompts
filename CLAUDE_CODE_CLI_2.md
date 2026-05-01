# Expert Prompt: Build a Production-Grade Claude Code CLI Replica (Ultra-Specific)

**You are a senior Claude Code engineer at Anthropic** who owns the terminal CLI interface. Implement (or generate complete, production-ready code for) a **pixel-perfect replica** of Claude Code’s interactive terminal experience, with obsessive attention to UI/UX details, micro-interactions, and copy.

## Core Philosophy
Replicate the exact feeling of using real Claude Code: fast, keyboard-first, powerful, calm, and delightful. The prompt box must feel like a first-class editor that is **always available**.

## Overall UI / TUI Requirements
- Pure terminal TUI using **Rust + ratatui + crossterm + tokio** (preferred) or Python + Textual.
- Flicker-free alternate screen / fullscreen rendering mode.
- Main area: scrollable conversation transcript (user/assistant messages, code blocks, diffs, tool outputs).
- **Persistent input box** anchored at the very bottom — **never disappears**, even during generation, background tasks, or streaming.
- Bottom status bar (always visible): model name, cwd, git branch, context % / tokens, active agents count, current permission mode (with colored badge), spinner when thinking.
- Top bar or inline indicators for running tasks.
- Support custom themes (JSON) with tokens for accents, diffs, backgrounds, etc.
- Smooth mouse scrolling and selection.

## Persistent Prompt Box – Extremely Detailed UX
- Always visible at bottom with a clear border (use brand accent color).
- Multi-line, auto-growing (with max height before internal scroll).
- Cursor is highly visible.
- When focused, show subtle "Type your message..." placeholder that disappears on input.
- **Large paste handling** (exact Claude Code behavior):
  - Pasting > ~10,000 characters collapses the input to a single line: `[Pasted text (N lines, M chars)]`.
  - Show hint below or inside: `(Esc again to clear • Ctrl+V again to expand)`.
  - Full content is preserved internally and sent on submit.
- Image paste (`Ctrl+V` / `Cmd+V`): inserts `[Image #1]` chip at cursor position with preview hint.
- Real-time counters in corner: lines / chars / ~tokens.

## Input Behavior & Shortcuts (Exact Parity)
- **Submission**: `Enter` submits.
- **Newline**: `Shift+Enter`, `Ctrl+J`, `\` + `Enter`, or `Option+Enter` (macOS Meta). Show temporary hint on first use.
- Full readline/Emacs bindings: `Ctrl+A/E`, `Ctrl+K/U/W/Y`, `Alt+B/F`, etc.
- **Vim mode** (toggleable in `/config`):
  - Complete NORMAL / INSERT / VISUAL modes.
  - All standard motions, operators, text objects, visual selections.
  - History navigation when in NORMAL mode at top/bottom edge.
- `Ctrl+G` → open current prompt (optionally + last assistant response as commented context) in `$EDITOR`.
- `Ctrl+R` → reverse history search with live highlighting and scope switching.
- `Ctrl+S` → stash prompt (with restore).
- `Ctrl+L` → redraw screen + clear input if pressed twice quickly.

## Live Steering & Concurrency (Critical)
- The prompt box **remains fully usable** while agents are thinking, streaming, or running background tasks.
- Users can type and send new prompts **at any time** to steer: correct direction, add constraints, cancel, or pivot.
- Visual indicator in status bar when background work is active (e.g., "2 agents running • Type to steer").
- `Ctrl+B` backgrounds the current task and returns focus to prompt.
- Support multiple parallel sub-agents with distinct colors.
- Task list (`Ctrl+T`) always accessible.

## Micro-copy & Polish Details (Claude Code Exact Style)
- After large paste collapse: `(Esc again to clear or Ctrl+V again to expand)`
- When background tasks run: subtle status message.
- On first multi-line use: temporary help like "Shift+Enter for new line • Enter to send".
- Permission prompts: clean overlay dialogs with arrow navigation, but prompt box stays visible below.
- Loading states: elegant spinner with "Claude is thinking..." or task-specific messages.
- Success/error states with colored accents (`success` / `error` tokens).
- Diffs: beautiful side-by-side or unified view with + / − coloring and word-level highlights.

## Additional Features & Shortcuts
- `?` → rich help overlay with all keybindings.
- `Esc Esc` → rewind / checkpoint menu (conversation only, code only, or both).
- `Ctrl+O` → toggle verbose transcript (tool calls, MCP, execution logs).
- `Shift+Tab` → cycle permission modes (default → plan → accept-edits → auto).
- Model switch (`Alt+P`), extended thinking (`Alt+T`), fast mode toggle.
- Slash command menu with fuzzy filtering.
- `@` file path autocomplete.
- `!` prefix for direct shell commands.
- Full command history (per project + global) with search.

## Technical & Extensibility
- Async-first so input loop is never blocked.
- Cross-platform + tmux support.
- Configurable keybindings (`keybindings.json`).
- Hooks system, skills, CLAUDE.md support.
- Pipe/non-interactive mode + resume.

**Output Instructions**:
- Provide full project structure and key source files.
- Heavy comments explaining persistent input, live steering, large paste handling, and Vim implementation.
- Include setup, example configs, and theme system.
- Make it feel **indistinguishable** from real Claude Code.

Build the ultimate Claude Code-style CLI. Obsess over every UX detail.
