**Got it. Here's the original first prompt with only your corrections applied (no full rewrite).**

I kept everything else exactly as in the first version and only inserted the new details about the prompt box behavior and the tech stack note.

---

**You are now rewriting the entire UI/UX layer of your own CLI agent to match the look, feel, behavior, and polish of the official Claude Code CLI (v2.1.128+, May 2026) as closely as humanly possible.**

**Goal**: Make this custom agent feel indistinguishable from native Claude Code CLI in everyday use. Focus exclusively on the terminal interface, input handling, output rendering, status, keyboard experience, and visual feedback. Do not change core agent logic, tool calling, or model behavior unless it directly affects UI.

### 1. Overall Layout & Visual Style (exact match)
- **Top Banner** (when session starts or `/banner`): Clean, minimal header with agent name/version on left, current model + effort level (e.g., "Opus 4.6 • xhigh") on right, thin separator line below. Use subtle ANSI colors (cyan/blue accents, dark background).
- **Main Conversation Area**: Scrollable chat history. 
  - User messages: left-aligned, prefixed with a subtle `>` or your custom indicator.
  - Agent responses: start with a thin colored bar or `Claude:` / agent name. Support markdown rendering (bold, code blocks with syntax highlighting via ANSI, lists, tables).
  - Tool use / thinking steps: collapsible or prefixed with `→` / spinner icons when active. Show them inline but de-emphasized unless verbose mode.
- **Fixed Prompt Box at Bottom**: Always visible, single-line by default but expands to multi-line (up to ~8-10 lines) when pasting or typing Enter without sending. 
  - Prompt area has a left `>` or `❯` prompt symbol.
  - Live word/token count on the right of the input box (updates in real-time).
  - When focused: subtle highlight/border (e.g., cyan underline or inverse).
  - **Important**: The prompt box **never dims or disables** while the agent is processing. It stays fully editable and usable at all times. Users can type new input and steer the agent while it is still thinking (new messages are queued).
- **Status Bar** (pinned at very bottom, always visible, 1 line): 
  - Left: permission mode (Normal / AcceptEdits / Plan / Auto), context usage % with mini progress bar (e.g., [████░░] 42%).
  - Center: current git branch, working directory basename, active agents count.
  - Right: model, session tokens/cost estimate, clock or elapsed time.
  - Make it customizable later via a command, but default to rich and informative like Claude Code's `/statusline`.
- **Colors & Theme**: Dark theme by default. Use:
  - User input: bright white/cyan.
  - Agent: softer green/blue.
  - Errors/warnings: red/yellow.
  - Success: green.
  - Keep it readable in standard terminals (xterm-256color, iTerm, Windows Terminal, etc.).

### 2. Input Handling (Critical — make this buttery smooth)
- **Fixed bottom prompt box**: Never scrolls away. History scrolls above it. **Stays fully interactive during processing and streaming.**
- **Multi-line support**: 
  - Paste multiple lines → auto-expand box.
  - Shift+Enter or Ctrl+Enter for new line inside prompt.
  - Enter alone (when multi-line) sends the prompt.
- **Keyboard Navigation & Editing** (full Emacs/readline + extras):
  - Arrow keys, Home/End, Ctrl+A/E (start/end), Ctrl+Left/Right (word), Ctrl+W (delete word), Ctrl+U/K (delete to start/end), Ctrl+Y (yank/paste).
  - Up/Down: browse prompt history (with reverse search on Ctrl+R).
  - Tab: intelligent auto-complete (files, slash commands, @mentions).
  - Support full paste of images/files if your backend allows (show "[Image attached]" preview).
- While agent is working: you can still freely type, edit, and queue new prompts.

### 3. Processing / Working State (always visible, no ambiguity)
- While agent is thinking/processing:
  - **"Processing..." legend + animated spinner** (e.g., ⠋⠙⠹⠸⠼⠴⠦⠧⠇⠏) right next to the input box or in status bar.
  - The prompt box remains fully usable (no dimming).
  - Show real-time partial output streaming with "Thinking..." or step indicators.
  - Progress indicators for long tasks (e.g., "Planning • 3/12 files", "Running tests • 45%").
- Interrupt: **Esc** cancels current generation immediately.
- Double **Esc** (when input empty): Open rewind/checkpoint menu (list previous states with summaries, allow restore).

### 4. Keyboard Shortcuts (implement all of these)
Make `?` or `/help` show a clean shortcut list. Implement at minimum:

| Shortcut              | Action |
|-----------------------|--------|
| **Enter**             | Send prompt (or new line if multi-line + Shift) |
| **Esc**               | Interrupt current response |
| **Esc Esc**           | Rewind / checkpoint menu |
| **Ctrl+C**            | Cancel input or hard exit on empty |
| **Ctrl+R**            | Reverse search history |
| **Ctrl+L**            | Clear screen (keep history) |
| **Ctrl+G**            | Open current prompt in $EDITOR |
| **Shift+Tab**         | Cycle permission modes (Normal → AcceptEdits → Plan → Auto) |
| **Ctrl+S**            | Stash current draft |
| **Ctrl+V / Cmd+V**    | Paste (text + image support if possible) |
| **↑ / ↓**             | History navigation |
| **Tab**               | Auto-complete |
| **Ctrl+O**            | Toggle verbose tool output |
| **?**                 | Show help/shortcuts |

Support Vim mode toggle via `/vim` if feasible.

### 5. Output & Interaction Polish
- **Streaming**: Token-by-token output where possible. Smooth, no flickering.
- **Diffs & Edits**: When showing file changes, use unified diff with colors (+ green, - red). Support accept/reject per hunk if your agent has that.
- **Loading States**: Every action has clear feedback (spinner, "Analyzing codebase...", "Applying edits...").
- **Error Handling**: Friendly, actionable errors with retry suggestions. Red but not alarming.
- **Pagination / Long Output**: Auto-pager for very long responses (`--more--` or space to continue).
- **Session Persistence**: On restart, resume last conversation with context restored visually.

### 6. Implementation Rules
- Use only whatever TUI libraries you already have in your stack (Claude Code itself is written in TypeScript + Ink/React, not Python — adapt the concepts to your current implementation).
- Add comments explaining each UI decision referencing "Claude Code parity".
- Output the full refactored code for the UI layer + a diff of changes.
- After implementing, test it yourself in a sample session and iterate until it feels 95%+ identical.

**Start by planning the architecture changes, then implement step-by-step.** Make no compromises on the "little things" — this is what separates a good agent from one that feels like the real Claude Code CLI.

Begin now.

---

This is the **first version** with only your requested changes patched in. Let me know the next small tweak.
