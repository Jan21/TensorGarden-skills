---
name: summarize-recent-chats
description: Use when the user wants to summarize freshly exported LLM chat transcripts from `/home/jan/projects/personalWiki/labMemory/chatgpt_chats`. Walks each subfolder (ChatGPT, Claude, Gemini, Grok, etc.), picks the most recently modified `.md` file in each, keeps only those whose mtime is within the last 5 minutes, and writes a `summary.md` in the current working directory. Any free-form text passed after the skill invocation is used as a guidance / steering prompt that biases the summary (focus, tone, audience, length, etc.).
---

# summarize-recent-chats

## Purpose

Produce a single `summary.md` in `$PWD` that distills only the **just-arrived** LLM chat transcripts found under `/home/jan/projects/personalWiki/labMemory/chatgpt_chats/*/`. Older transcripts are ignored. The user's free-form guidance text (passed as args after the skill name) shapes the summary.

## When to Use

- User says: "summarize the latest chats", "/summarize-recent-chats ...", "summarize what I just exported from ChatGPT/Claude/Gemini/Grok".
- A new `.md` chat export was just dropped into one of the subfolders of `/home/jan/projects/personalWiki/labMemory/chatgpt_chats/`.
- User wants a fast roll-up they can paste into a meeting note, project log, or lab journal entry.

## When NOT to Use

- User wants to summarize a specific file by path: read it directly instead.
- User wants a summary of all historical chats (no 5-minute freshness filter): use a different workflow.
- Source directory does not exist or is empty.

## Inputs

1. **Base directory** (hard-coded): `/home/jan/projects/personalWiki/labMemory/chatgpt_chats`
2. **Freshness window** (hard-coded): 5 minutes (300 seconds).
3. **Output path**: `./summary.md` in the directory where the user invoked the skill (`$PWD`).
4. **Guidance text** (optional): everything the user typed after the skill name. May be empty. Treat as steering for the summary's focus, tone, length, or audience.

## Procedure

Execute these steps in order. Do not skip the freshness check.

### Step 1 — Locate fresh transcripts

Run this single command and capture its output. It lists, per subfolder, the newest `.md` file, but only if its mtime is within the last 300 seconds.

```bash
BASE=/home/jan/projects/personalWiki/labMemory/chatgpt_chats
NOW=$(date +%s)
for d in "$BASE"/*/; do
  [ -d "$d" ] || continue
  newest=$(find "$d" -maxdepth 1 -type f -name '*.md' -printf '%T@ %p\n' 2>/dev/null \
           | sort -nr | head -1)
  [ -z "$newest" ] && continue
  mtime=${newest%% *}
  path=${newest#* }
  mtime_int=${mtime%.*}
  age=$(( NOW - mtime_int ))
  if [ "$age" -le 300 ]; then
    printf '%s\t%s\n' "$age" "$path"
  fi
done
```

The output is `age_seconds<TAB>absolute_path`, one fresh file per subfolder. If output is empty, stop and tell the user: "No transcripts modified in the last 5 minutes; nothing to summarize."

### Step 2 — Read the fresh files

For each path emitted in Step 1, read the full file with the `Read` tool. Note which subfolder (ChatGPT / Claude / Gemini / Grok / ...) each came from. Use the parent directory name as the source label.

### Step 3 — Compose the summary

Build `summary.md` with this structure:

```markdown
# Chat summary — <YYYY-MM-DD HH:MM local time>

> Guidance: <verbatim guidance text, or "(none)">

## <Source-folder-name> — <basename of file>

<2–6 paragraph synthesis: what was discussed, key decisions, useful code/links, follow-ups>

## <next source>

...

## Cross-cutting themes

<bullet list, only if there are ≥2 source files and themes actually overlap; omit otherwise>
```

Apply the user's guidance text as steering: e.g. "focus on code I should reuse", "make it short", "rewrite as a lab-meeting bullet list", "include only what's actionable for project X". If guidance is empty, default to a concise neutral synthesis: ~150–250 words per file.

Do **not** dump raw chat content. Summarize. Quote sparingly (≤2 short quotes per file) and only when wording matters.

### Step 4 — Write the file

Write `./summary.md` (in `$PWD`, the user's invocation directory — NOT in the skill folder, NOT in the chatgpt_chats tree). Overwrite if it exists.

### Step 5 — Report

Reply with a one-line confirmation: number of files summarized, output path, and any subfolders skipped because their newest file was stale.

## Quick Reference

| Step | Action |
|------|--------|
| 1 | `find`-based freshness filter, mtime ≤ 300s |
| 2 | `Read` each fresh `.md` file fully |
| 3 | Compose `summary.md` with one section per source folder |
| 4 | Write to `$PWD/summary.md` |
| 5 | Brief confirmation back to user |

## Common Mistakes

- **Writing to the wrong directory.** `summary.md` goes in `$PWD`, not in the chats source tree, not in the skill folder.
- **Ignoring the 5-minute window.** Stale files (mtime > 300s) must be skipped, even if they are the newest in a folder.
- **Echoing whole transcripts.** This is a summary; condense.
- **Forgetting the guidance text.** If args are non-empty, they steer the summary — don't drop them.
- **Picking multiple files per folder.** Only the single newest `.md` per subfolder is considered.

## Edge Cases

- Subfolder has no `.md` files: skip silently.
- Subfolder's newest `.md` is older than 5 min: skip; mention in the final report.
- All subfolders are stale: do **not** write `summary.md`; tell user nothing was fresh.
- `$PWD/summary.md` already exists: overwrite.
- Guidance text contains shell metacharacters: treat as plain text, never `eval` it.
