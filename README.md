# TensorGarden Skills

Repository of skills used at the **TensorGarden** lab.

Skills here cover lab workflows such as scientific writing, paper drafting, and other research productivity tasks.

## Skills

- [`ml-paper-writing`](./ml-paper-writing) — drafting, restructuring, and critiquing ML research papers.

## Usage

Skills are invoked via the `Skill` tool in Claude Code, or by typing `/<skill-name>` in a session. To make a skill available globally, symlink or copy its directory into `~/.claude/skills/`:

```bash
ln -s "$PWD/ml-paper-writing" ~/.claude/skills/ml-paper-writing
```

## Contributing

Add new skills as top-level directories, each containing a `SKILL.md` with YAML frontmatter (`name`, `description`) describing when the skill applies.
