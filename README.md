# 🧠 AI Memory Bank (Submodule)

> This folder is a **git submodule** and serves as the persistent, modular "brain" for your project. It is managed and accessed by the parent project (e.g., `aimemoryi`) for robust, context-aware memory.

---

## Purpose

- Stores all project memory, context, and progress in a modular Markdown structure.
- Enables persistent, rules-driven context for Cursor, agents, and CLI workflows.
- Designed for safe, auditable, and version-controlled project memory.

---

## Structure

```text
memory-bank/
  core/           # Project vision, context, and active focus
  systemPatterns/ # Architecture, design patterns, scanning
  techContext/    # Stack, dependencies, environment
  progress/       # Current progress, history, roadmap
```

---

## Usage

- **Do not edit files here directly** unless you know what you are doing.
- Use the parent project's CLI or MCP tools to read, update, or repair memory files.
- All changes should be committed and pushed from the parent repo to keep submodule pointers in sync.

---

## Updating the Submodule

To update this submodule to the latest commit:

```sh
cd memory-bank
git checkout aimemory  # or the appropriate branch
git pull
cd ..
git add memory-bank
git commit -m "⬆️ Update memory-bank submodule"
```

---

## Safety & Best Practices

- Always back up your memory bank before major git operations (branch switch, rebase, etc.).
- Never delete or overwrite files in this folder without understanding the consequences.
- For more info, see the parent project's documentation and submodule best practices.

---

> *Happy memory banking!*
