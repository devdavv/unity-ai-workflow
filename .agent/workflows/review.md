---
description: Standalone code review pass against the project's standards. Use after implementing code to catch issues before committing. Triggers on "/review", "review this", "code review", "check my code", or "pre-commit check". Also runs automatically as Step 8.5 inside /implement-feature.
---

# /review — Code Review Pass

Run a structured quality review on implemented code before it is committed.

## Agent
**QA Tester** — Load `qa-tester.md` persona.

## Skill
`code-review`

## Steps

### 1. Identify Scope
Ask (or infer from context):
> "Which file(s) or feature should I review? Or should I review everything changed since the last commit?"

In **automatic** mode: infer from recent changes (git diff) and proceed.

### 2. Read Target Files
Read all specified files. Also read:
- `docs/GDD.md` — to verify Gherkin scenario coverage
- `docs/GFD.md` — to verify game feel status (integrated or TODO filed)
- `.agent/rules/RULES.md` — the non-negotiable checklist source

### 3. Run Code Review Skill
Apply the full `code-review` checklist:
- RULES.md compliance
- Naming conventions
- Code quality
- Performance anti-patterns
- Test coverage (Gherkin → NUnit mapping)
- Game feel (integrated or TODO filed)

### 4. Output Structured Report
Produce the code-review report format:
```
## Code Review — {FeatureName or FileList}

### RULES.md          [✅ Pass | ⚠️ Issues]
### Naming            [✅ Pass | ⚠️ Issues]
### Code Quality      [✅ Pass | ⚠️ Issues]
### Performance       [✅ Pass | ⚠️ Issues]
### Test Coverage     [✅ X/Y scenarios | ⚠️ Missing]
### Game Feel         [✅ Integrated | ⏳ TODO filed | ⚠️ Missing]

Issues to fix before commit:
- [list each issue with file and line, or "None"]
```

### 5. Resolve Issues
- In **automatic** mode: fix flagged issues immediately, then re-run the relevant checklist items.
- In **mix/assistant** mode: present the report, wait for user confirmation, then fix.

Only mark the review complete when **Issues to fix** is "None".

### 6. Output
- Clean review report (all green)
- Code ready to commit
- If `prompt_logging: true`: append a log entry to `docs/PromptLog.md`
