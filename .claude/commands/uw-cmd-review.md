# /uw-cmd-review — Code Review Pass

Run a structured quality review on implemented code before it is committed.

> **Note**: This is a Unity-specific review. For general code review, Claude Code also offers native PR review via GitHub App integration.

## Skills
Load `uw-code-review` from `.claude/skills/uw-code-review/SKILL.md`.

## Steps

### 1. Identify Scope
Ask (or infer from context):
> "Which file(s) or feature should I review? Or should I review everything changed since the last commit?"

In **autonomous** mode: infer from recent changes (`git diff`) and proceed.

### 2. Read Target Files
Read all specified files. Also read:
- `docs/GDD.md` — to verify Gherkin scenario coverage
- `docs/GFD.md` — to verify game feel status (integrated or TODO filed)

### 3. Run Code Review Checklist
Apply the full `uw-code-review` checklist:
- CLAUDE.md rules compliance
- Naming conventions
- Code quality
- Performance anti-patterns
- Test coverage (Gherkin -> NUnit mapping)
- Game feel (integrated or TODO filed)

### 4. Output Structured Report
```
## Code Review — {FeatureName or FileList}

### Rules Compliance  [Pass | Issues]
### Naming            [Pass | Issues]
### Code Quality      [Pass | Issues]
### Performance       [Pass | Issues]
### Test Coverage     [X/Y scenarios | Missing]
### Game Feel         [Integrated | TODO filed | Missing]

Issues to fix before commit:
- [list each issue with file and line, or "None"]
```

### 5. Resolve Issues
- In **autonomous** mode: fix flagged issues immediately, then re-run.
- In **guided** mode: present the report, wait for user confirmation, then fix.

Only mark the review complete when **Issues to fix** is "None".

### 6. Output
- Clean review report (all green)
- Code ready to commit

$ARGUMENTS
