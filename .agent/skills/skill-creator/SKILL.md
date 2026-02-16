---
name: Skill Creator
description: Guide for creating new skills that extend agent capabilities with specialized knowledge, workflows, or tool integrations. Use when users want to create a new skill, when agents identify repetitive task patterns without an existing skill, or when updating an existing skill. Triggers on requests like "create a new skill", "make a skill for X", "this pattern should be a skill", or when agents self-identify skill gaps during repetitive work.
---

# Skill Creator

Create or update skills following Anthropic's official guidelines, adapted for this Unity workflow.

## When to Create a Skill
- You're repeating the **same code pattern** across multiple tasks.
- A workflow requires **domain-specific knowledge** that isn't obvious.
- A task needs **specific tool integrations** or MCP patterns.
- The user explicitly asks for a new skill.

## Core Principles

### 1. Concise is Key
The context window is shared. Only add information the AI doesn't already know. Challenge each paragraph: *"Does the AI really need this?"*

### 2. Degrees of Freedom
- **High freedom** (text instructions): Multiple valid approaches, context-dependent.
- **Medium freedom** (pseudocode): Preferred pattern exists but variation OK.
- **Low freedom** (exact scripts): Fragile operations, consistency critical.

### 3. Progressive Disclosure
Three-level loading — only load what's needed:
1. **Metadata** (name + description) — always in context (~100 words)
2. **SKILL.md body** — loaded when triggered (<500 lines, ideally <100)
3. **references/** — loaded only when the AI determines it's needed

## Anatomy of a Skill

```
skill-name/
├── SKILL.md              (required)
│   ├── YAML frontmatter  (name + description)
│   └── Markdown body     (instructions)
└── references/           (optional, for variants/detailed docs)
    ├── variant-a.md
    └── variant-b.md
```

## Creation Steps

### 1. Identify the Skill
Ask:
- What repetitive task does this automate?
- What non-obvious knowledge does it encode?
- What triggers should activate it?

### 2. Write the Frontmatter
The `description` is the **primary trigger mechanism**. Include:
- What the skill does
- When to use it (specific trigger scenarios)
- Example phrases that should activate it

```yaml
---
name: My New Skill
description: [What it does]. Use when [specific scenarios]. Triggers on requests like "[phrase 1]", "[phrase 2]", or [general category]. [Prerequisites or conditions].
---
```

### 3. Write the Body
- Keep under 100 lines (hard limit: 500).
- Only include what the AI doesn't already know.
- Use imperative form ("Generate", "Read", "Create").
- If supporting multiple variants, keep core workflow inline and split variant-specific details into `references/`.

### 4. Split Variants (if needed)
When a skill supports multiple frameworks or options:
```
my-skill/
├── SKILL.md                  (core workflow + variant selection)
└── references/
    ├── framework-a.md        (loaded only when framework A is chosen)
    └── framework-b.md
```

### 5. Test
Use the skill on a real task. If it struggles, iterate on the SKILL.md.

## Do NOT Include
- README.md, CHANGELOG.md, or other auxiliary docs
- Information the AI already knows (basic C# syntax, common patterns)
- "When to Use This Skill" sections in the body (that belongs in the description)

## File Location
All skills go in `.agent/skills/{skill-name}/SKILL.md`.
