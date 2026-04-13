---
type: Note
related_to: "[[tolaria]]"
---
# AI in Tolaria

Tolaria embraces AI without making it a requirement. You have options depending on your workflow.

## AI Works Without Integration
Because Tolaria is a file-based vault with AGENTS.md documenting the format, any AI CLI can work with it. Claude Code, Codex, or your own assistant—all understand the vault structure. Treat Tolaria notes like any other codebase and prompt naturally.

## Built-in Integration
Tolaria has optional built-in AI integration that uses CLI assistants you already have:

### Quick Prompt Mode
Press Cmd+K, then type a space to enter quick prompt mode in the Command Palette. Add wikilinks to pass note context. Great for disposable questions and one-off tasks.

### AI Panel
Press Cmd+Shift+K (or click the AI icon in the breadcrumb bar) to open the full AI panel. This gives you persistent conversation history with Claude Code. Use for:
- Summarizing notes
- Generating outlines
- Refactoring content
- Answering questions about your vault

### MCP Tools
Lightweight MCP tools ship with Tolaria. They're not for file operations—your notes are just files. Instead, they let AI show what it's doing, like opening a specific note in the editor or running commands.

### Connection Status
The bottom bar shows Claude Code connection status so you always know if AI features are available.

## When to Use Which
- **Cmd+K quick prompt** — For fast, disposable questions
- **AI panel** — For structured conversations, iterative work, and tasks that need context across multiple messages
