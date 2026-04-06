# Claude Code — CLI Tool for AI-Powered Development

> This is an example raw file. Try telling Claude Code "compile" to process this into the wiki!

## What is Claude Code?

Claude Code is Anthropic's official CLI tool that gives Claude direct access to your local file system, terminal, and development environment. It runs in your terminal and can read, write, and execute commands on your behalf.

## Key Features

- **File System Access**: Read and write files directly on your computer
- **Terminal Execution**: Run shell commands and see output
- **CLAUDE.md**: Reads a CLAUDE.md file in the project root every session for persistent instructions
- **Context Awareness**: Understands your project structure by reading files
- **Multi-turn Conversations**: Maintains context across a session

## How It Works with Obsidian RAG

1. You open a terminal in your Obsidian vault folder
2. Run `claude` to start Claude Code
3. Claude Code reads `CLAUDE.md` and understands it's a knowledge base librarian
4. You give commands like "compile", "query", or "audit"
5. Claude Code reads/writes markdown files in the vault

## Installation

```bash
npm install -g @anthropic-ai/claude-code
```

## Basic Commands

- Start a session: `claude`
- Resume last session: `claude --continue`
- One-shot command: `claude "your prompt here"`
