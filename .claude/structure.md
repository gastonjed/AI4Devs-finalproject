# Claude Code + Spec-Kit Reference

> Official structure reference for Claude Code and Spec-Kit
> Updated: 2026-01-06

## Quick Reference

| Type | Location | Invocation | Purpose | Used By |
|------|----------|------------|---------|---------|
| **Memory** | `CLAUDE.md` (root) | Auto-loaded | Project context & conventions | Claude (every session) |
| **Rules** | `.claude/rules/*.md` | Imported to CLAUDE.md | Coding standards documentation | Memory (via @import) |
| **Commands** | `.claude/commands/name.md` | `/name` | Simple prompt shortcuts | User |
| **Skills** | `.claude/skills/name/SKILL.md` | Auto OR `/name` | Complex multi-step workflows | Claude (auto), User, Agents |
| **Agents** | `.claude/agents/name/AGENT.md` | Auto-delegated | Specialized AI for subtasks | Claude (delegates tasks) |
| **Hooks** | `.claude/settings.json` | Automatic (events) | Enforce rules, automate tasks | Claude (on tool events) |
| **Settings** | `.claude/settings.json` | Auto-loaded | Permissions, env, model config | Claude (startup) |
| **Specs** | `specs/NNN-feature/` | Spec-kit workflow | Feature specifications | Commands, Skills |

## Official Directory Structure

```
project-root/
├── CLAUDE.md                          # Main project memory
├── CLAUDE.local.md                    # Personal overrides (gitignored)
│
├── .claude/
│   ├── settings.json                  # Project settings
│   ├── settings.local.json            # Personal settings (gitignored)
│   ├── .mcp.json                      # MCP servers (optional)
│   │
│   ├── commands/                      # User-invoked slash commands
│   │   └── name.md                    # /name
│   │
│   ├── skills/                        # Auto OR user-invoked workflows
│   │   └── skill-name/
│   │       └── SKILL.md
│   │
│   ├── agents/                        # Specialized AI agents
│   │   └── agent-name/
│   │       └── AGENT.md
│   │
│   ├── rules/                         # Documentation (imported to CLAUDE.md)
│   │   └── *.md
│   │
│   └── hooks/                         # Hook scripts (optional)
│       └── *.sh
│
├── specs/                             # Spec-kit feature specs
│   ├── constitution.md                # Project principles (create first!)
│   └── NNN-feature/
│       ├── spec.md                    # Requirements
│       ├── plan.md                    # Implementation design
│       └── tasks.md                   # Task breakdown
│
└── .specify/                          # Spec-kit framework files
    ├── memory/                        # Spec-kit memory
    ├── templates/                     # Spec templates
    └── scripts/                       # Spec-kit scripts
```

## Component Formats

### Commands
**File:** `.claude/commands/name.md`

```markdown
---
description: Brief description
argument-hint: [args]
allowed-tools: Bash, Read
---

Prompt template.
Use $ARGUMENTS or $1, $2 for params.
Use @file to import.
Use !`command` for bash.
```

### Skills
**File:** `.claude/skills/name/SKILL.md`

```markdown
---
name: skill-name
description: When Claude should use this
allowed-tools: Read, Edit, Bash
model: sonnet
---

# Instructions
Detailed workflow steps...
```

### Agents
**File:** `.claude/agents/name/AGENT.md`

```markdown
---
name: agent-name
description: When to delegate to this agent
tools: Read, Grep, Glob
model: sonnet
skills: skill1, skill2
---

System prompt for agent...
```

### Rules
**File:** `.claude/rules/standards.md`

```markdown
# Coding Standards

- Standard 1
- Standard 2
```

**Import in CLAUDE.md:**
```markdown
@.claude/rules/standards.md
```

### Hooks
**File:** `.claude/settings.json`

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write $FILE"
          }
        ]
      }
    ]
  }
}
```

**Hook types:** `PreToolUse`, `PostToolUse`, `PermissionRequest`

### Memory
**File:** `CLAUDE.md` (project root)

```markdown
# Project Name

## Context
Project description...

## Conventions
- Coding standards
- Commit format
- Testing approach

## Imports
@docs/TECH_STACK.md
@.claude/rules/standards.md
```

### Settings
**File:** `.claude/settings.json`

```json
{
  "model": "claude-sonnet-4-5",
  "permissions": {
    "Bash": "ask",
    "Edit": "allow",
    "Write": "ask"
  },
  "env": {
    "NODE_ENV": "development"
  },
  "hooks": {}
}
```

## Spec-Kit Workflow

| Command | Output | Description |
|---------|--------|-------------|
| `/speckit.constitution` | `specs/constitution.md` | Project principles |
| `/speckit.specify` | `specs/NNN-feature/spec.md` | Feature requirements |
| `/speckit.clarify` | Updated spec | Resolve ambiguities |
| `/speckit.plan` | `specs/NNN-feature/plan.md` | Implementation design |
| `/speckit.analyze` | Analysis report | Validate consistency |
| `/speckit.tasks` | `specs/NNN-feature/tasks.md` | Task breakdown |
| `/speckit.implement` | Code | Execute tasks |

## Decision Matrix

| Use Case | Type to Use |
|----------|-------------|
| Simple prompt I repeat | Command |
| Complex multi-step workflow | Skill |
| Delegate to specialized AI | Agent |
| Document coding standards | Rules (in CLAUDE.md) |
| Enforce rules automatically | Hooks |
| Share project context | Memory (CLAUDE.md) |
| Configure permissions | Settings |

## Workflow for Custom Modifications

### Quick Routing (What to Create)

| I Need To... | Create |
|--------------|--------|
| Add context/conventions | `CLAUDE.md` |
| Document coding standards | Rules (`.claude/rules/*.md`) |
| Repeat same prompt often | Command |
| Multi-step workflow | Skill |
| Delegate to specialist AI | Agent |
| Auto-enforce rules | Hook |

### Progressive Path (When to Upgrade)

```
CLAUDE.md → Rules → Command → Skill → Agent + Hook
(context)    (docs)   (repeat)  (complex) (delegate + enforce)
```

**When to upgrade:**

- **Rules:** CLAUDE.md has >200 lines or many standards to document
- **Command:** Repeating same prompt 3+ times
- **Skill:** Command gets >20 lines or needs auto-trigger
- **Agent:** Need specialized analysis/review
- **Hook:** Need automatic enforcement

### Example: TypeScript Support Evolution

```markdown
# Iteration 1: CLAUDE.md
Use TypeScript strict mode. No `any` types.

# Iteration 2: Rules (.claude/rules/typescript.md + import in CLAUDE.md)
# TypeScript Standards
- Use strict mode
- No `any` types
- Prefer interfaces over types

# Iteration 3: Command (.claude/commands/ts-review.md)
---
description: Review TypeScript best practices
---
Check: type safety, no `any`, error handling

# Iteration 4: Skill (.claude/skills/ts-review/SKILL.md)
---
name: ts-review
description: Auto-review TypeScript code
---
[Multi-step analysis workflow...]

# Iteration 5: Hook (.claude/settings.json)
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Edit",
      "hooks": [{"type": "command", "command": "tsc --noEmit"}]
    }]
  }
}
```

## Key Differences

### Commands vs Skills
- **Commands:** Simple prompts, always user-invoked (`/command`)
- **Skills:** Complex workflows, auto-invoked OR user-invoked (`/skill`)

### Rules vs Hooks
- **Rules:** Documentation files (imported to CLAUDE.md for context)
- **Hooks:** Enforcement in settings.json (auto-runs on events)

### Skills vs Agents
- **Skills:** Workflows Claude performs (e.g., generate spec)
- **Agents:** Specialized AI Claude delegates to (e.g., review spec)

## Configuration Hierarchy

Precedence (highest to lowest):

1. Enterprise (admin-managed)
2. Command-line flags
3. Local project (`.claude/settings.local.json`, `CLAUDE.local.md`)
4. Project (`.claude/settings.json`, `CLAUDE.md`)
5. User (`~/.claude/settings.json`, `~/.claude/CLAUDE.md`)

## User-Level Config

**Location:** `~/.claude/` (applies to ALL projects)

```
~/.claude/
├── CLAUDE.md              # Personal preferences
├── settings.json          # Personal settings
├── commands/              # Personal commands
├── skills/                # Personal skills
└── agents/                # Personal agents
```

## Official Documentation

- Claude Code: https://platform.claude.com/docs/en/home
- Spec-Kit: https://github.com/github/spec-kit
