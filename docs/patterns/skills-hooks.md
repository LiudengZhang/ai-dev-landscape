# Skills and Hooks

## Skills

### What They Are

Markdown files that extend Claude Code's capabilities. A skill is a structured prompt template — it tells Claude Code *how* to do something.

### Where They Live

```
~/.claude/skills/          # Global skills (available everywhere)
.claude/skills/            # Project skills (checked into git)
```

### Anatomy of a Skill

```markdown
---
description: Short description shown in skill list
user_invocable: true       # Can be called with /skill-name
---

# Skill Name

## When to Use
[Conditions for invoking this skill]

## Process
[Step-by-step instructions]

## Templates
[Code templates, file structures, etc.]
```

### Skills I Use

| Skill | What It Does |
|-------|-------------|
| `awesome-list-builder` | Full pipeline for creating awesome lists + MkDocs sites |
| `subagent-driven-development` | Execute plans via fresh subagent per task with two-stage review |
| `writing-plans` | Create detailed implementation plans with TDD |
| `code-reviewer` | Review implementation against plan and standards |

### Key Insight

Skills are **codified expertise**. Instead of explaining your workflow every session, write it once as a skill. The skill becomes part of your toolbox permanently.

## Hooks

### What They Are

Shell commands that run automatically in response to Claude Code events.

### Configuration

In `~/.claude/settings.json` or `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "command": "echo 'About to write a file'"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Bash",
        "command": "echo 'Command executed'"
      }
    ]
  }
}
```

### Event Types

| Event | When | Use Case |
|-------|------|----------|
| `PreToolUse` | Before a tool runs | Validation, confirmation |
| `PostToolUse` | After a tool runs | Auto-format, lint, notify |
| `Notification` | On notification | Desktop alerts, Slack messages |

### Practical Examples

**Auto-format Python files after write:**
```json
{
  "PostToolUse": [{
    "matcher": "Write",
    "command": "if [[ \"$TOOL_INPUT\" == *.py ]]; then ruff format \"$TOOL_INPUT\"; fi"
  }]
}
```

**Block writes to sensitive files:**
```json
{
  "PreToolUse": [{
    "matcher": "Write",
    "command": "if [[ \"$TOOL_INPUT\" == *.env* ]]; then echo 'BLOCKED: cannot write .env files' >&2; exit 1; fi"
  }]
}
```

## Skills + Hooks Together

Skills define *what* to do. Hooks automate the *surrounding* workflow. Together they make Claude Code sessions consistent and efficient.
