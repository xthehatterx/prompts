# Expert Prompt: Build a Production-Grade Claude Code CLI Replica (npm + Node.js Compatible)

**You are a senior Claude Code engineer at Anthropic.** Create a near pixel-perfect replica of Claude Code’s interactive terminal CLI using a stack that works well with **npm and Node.js** (no Bun required).

## Required Tech Stack (Maximum Fidelity + Practical)
- **Language**: TypeScript (strict)
- **Runtime**: Node.js (latest LTS)
- **UI Framework**: React + Ink (the same library real Claude Code uses for its polished terminal UI)
- **Package Manager**: npm
- **Build/Run**: Use `ts-node` for development + `pkg` or `tsc` + `node` for distribution if needed
- Use modern React patterns (hooks, context, components)

This gives you almost identical UI behavior to real Claude Code while staying compatible with your work environment.

## Core UI/UX Requirements (Exact Claude Code Behavior)

### Persistent Prompt Box (Most Important)
- Always visible and fully editable at the bottom of the screen.
- Never hidden during streaming, agent thinking, background tasks, or tool execution.
- Users can type and submit new prompts **at any time** to steer agents live.

### Large Paste Handling (Exact Micro-Copy)
- When pasting > ~10,000 characters, collapse to: `[Pasted text (N lines, M chars)]`
- Show hint below the box: `(Esc again to clear • Ctrl+V again to expand)`
- Full content preserved internally and sent on submit.

### Input Behavior
- `Enter` = submit prompt
- New line: `Shift+Enter`, `Ctrl+J`, or `\` + `Enter`
- Full Vim editing mode (toggleable via config) with NORMAL / INSERT / VISUAL modes
- `Ctrl+G` → open in external `$EDITOR` (with optional last response as context)
- Image paste (`Ctrl+V`) → insert `[Image #N]` chips
- Real-time line/char/token counter

### Live Steering & Concurrency
- Prompt box stays active while agents run in background.
- Status bar shows: “X agents running • Type to steer”
- `Ctrl+B` to background current task
- Support parallel sub-agents with distinct colors

### Other Exact Details
- Bottom status bar: model, cwd, git branch, context %, permission mode badge, progress
- Smooth reactive updates via Ink
- Beautiful diffs with word-level highlights
- Permission dialogs as overlays (prompt box remains visible)
- Slash command fuzzy menu, `@` file autocomplete, `!` shell mode
- `?` help, `Ctrl+R` history search, `Ctrl+S` stash, `Esc Esc` rewind, etc.

## Project Structure & Deliverables
Generate a complete npm-ready project with:
- `package.json` (with necessary Ink, React, Ink dependencies)
- `tsconfig.json`
- `src/` folder with clean React + Ink components (`PromptBox.tsx`, `Transcript.tsx`, `StatusBar.tsx`, etc.)
- Config system (`settings.json`, keybindings)
- Clear instructions for `npm install && npm run dev`

Prioritize:
1. Always-visible + responsive prompt box
2. Live steering during agent work
3. Exact large-paste behavior and micro-copy
4. Overall polish that feels like real Claude Code

Make it delightful and production-ready.
