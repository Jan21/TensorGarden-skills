# TensorGarden Skills

Repository of skills used at the **TensorGarden** lab.

Skills here cover lab workflows such as scientific writing, paper drafting, and other research productivity tasks.

## Skills

- [`ml-paper-writing`](./ml-paper-writing) — drafting, restructuring, and critiquing ML research papers.
- [`summarize-recent-chats`](./summarize-recent-chats) — summarizes freshly exported LLM chat transcripts (last 5 min) from `personalWiki/labMemory/chatgpt_chats` into a `summary.md` in the current directory, steered by free-form guidance text.

## Usage

Skills are invoked via the `Skill` tool in Claude Code, or by typing `/<skill-name>` in a session. To make a skill available globally, symlink or copy its directory into `~/.claude/skills/`:

```bash
ln -s "$PWD/ml-paper-writing" ~/.claude/skills/ml-paper-writing
```

## Contributing

Add new skills as top-level directories, each containing a `SKILL.md` with YAML frontmatter (`name`, `description`) describing when the skill applies.
