---
name: init-project
description: Bootstrap or audit a project with principled CLAUDE.md + plan.md + codify.md. Auto-detects tech stack, plan-first.
invocation: user
---

# Init Project

프로젝트 초기화 및 감사 스킬. 원칙 기반 3-파일 시스템을 생성하거나 기존 파일을 개선합니다.

## Mode Detection

On invocation, detect which mode to run:

- **If CLAUDE.md does NOT exist** → **Init Mode** (create new files)
- **If CLAUDE.md already exists** → **Audit Mode** (score + upgrade existing files)

The user can override with `/init-project audit` or `/init-project init`.

---

## Init Mode

### Phase 1: Analyze (분석)

1. Scan the current project directory for:
   - Package files: `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `Gemfile`, `pom.xml`
   - Config files: `.env`, `tsconfig.json`, `vite.config.*`, `next.config.*`, `.eslintrc.*`
   - Existing docs: `README.md`, `CLAUDE.md`, `plan.md`, `codify.md`
   - Source structure: `src/`, `lib/`, `app/`, `tests/`, `cmd/`
   - Git history: recent commits, branch structure

2. Detect:
   - Primary language(s) and framework(s)
   - Package manager (`uv` for Python, `npm`/`pnpm`/`bun` for JS/TS, etc.)
   - Test framework (`pytest`, `jest`, `vitest`, etc.)
   - Build system and commands
   - Existing conventions (naming, structure)

3. Run **Start-of-Project Ritual** (from /tech-lead P1, P2, P6):
   - P1: What is this project's success criteria? What are explicit non-goals?
   - P2: What's the biggest unknown that could derail work?
   - P6: Does similar already exist in the codebase or user's other projects?

### Phase 2: Plan (계획)

4. Present the user a structured plan showing:
   - Detected stack summary
   - Proposed CLAUDE.md sections (with preview of each)
   - Proposed plan.md structure (with Effectiveness Contract)
   - Proposed codify.md template
   - Any `.claude/rules/` files if the project warrants modular rules
   - What will be CREATED vs what already EXISTS (never overwrite without asking)

5. Ask the user to approve, adjust, or reject before generating anything.

### Phase 3: Generate (생성)

6. After approval, generate files using the skeleton from `@references/skeleton.md` customized to the detected stack. Apply stack-specific overrides from `@references/stack-overrides.md`.

### Phase 4: Validate (검증)

7. After generating, run automated checks:
   - **Line count**: Is CLAUDE.md < 100 lines? Warn if over.
   - **Deletion test**: Sample 3 lines and ask "does removing this change Claude's behavior?"
   - **Secret scan**: grep for `sk-`, `AKIA`, `password=`, `secret`, `token=` patterns
   - **Command check**: Do Commands match the detected package manager?
   - **Consistency check**: Do Principles contradict each other?
8. Report findings to user. Fix issues if needed.

---

## Audit Mode (기존 파일 감사)

When CLAUDE.md already exists, run an upgrade analysis:

### Step 1: Score Existing CLAUDE.md

Apply the 6 quality checks from `@references/principles.md`:

| Check | How |
|-------|-----|
| **Deletion test** | Pick 5 lines. Would Claude behave differently without each? Flag lines that fail. |
| **Linter test** | Flag rules that `.editorconfig` or linters already handle. |
| **Obviousness test** | Flag framework knowledge Claude already has (e.g., "React uses JSX"). |
| **Portability test** | Flag environment-specific rules. Suggest principle-based rewrites. |
| **Length check** | Count lines. If > 100, propose what to split into `.claude/rules/`. |
| **Gotcha test** | Are there known gotchas NOT documented? Check for common pain points. |

### Step 2: Score Against Structure

Check if these sections exist and are well-formed:
1. Project Purpose (1-2 sentences?)
2. Tech Stack (with versions?)
3. Key Structure (only non-obvious?)
4. Principles (3-7, portable?)
5. Commands (exact flags?)
6. Gotchas (real time-savers?)
7. Decision Hygiene (present?)

### Step 3: Propose Improvements

Show a **diff preview** of proposed changes:
- Lines to DELETE (failed quality checks)
- Lines to ADD (missing sections)
- Lines to REWRITE (rules → principles)
- Sections to EXTRACT (to `.claude/rules/` if over 100 lines)

### Step 4: Validate

Same as Init Mode Phase 4. Also check:
- Does plan.md exist? Propose creation if not.
- Does codify.md exist? Propose creation if not.
- Are there friction signals in the codebase? (TODO/FIXME without owner, catch-all handlers, etc.)

---

## Critical Rules

- **NEVER overwrite** existing files without explicit user consent
- If files exist, show a diff and ask to merge or replace
- CLAUDE.md must be **under 100 lines** — split to `.claude/rules/` if needed
- Every line must pass **the deletion test**: if removing it doesn't change Claude's behavior, delete it
- **Principles over rules**: write portable guidance, not environment-specific commands
- **Korean + English mix**: section headers in English, descriptions can include Korean
- For Python projects, **always** specify `uv` as the package manager
- Include TDD reminders when test frameworks are detected

## Decision Hygiene Block (결정 위생)

This is the standard block to include as CLAUDE.md section 7. Defined once here — templates reference this, never duplicate it.

```
## Decision Hygiene (결정 위생)
- "I'll just..." / "for now..." → 멈추고 진짜 결정을 내리세요
- 새 모듈/디렉토리 생성 시 → "비슷한 코드가 어디 있나?" 먼저 확인
- 커밋 전: 어떻게 고장 났는지 알 수 있나? 항상 참이어야 할 것은? 왜 이렇게 결정했나?
```

## CLAUDE.md Structure (7 sections max)

```
1. Project Purpose    — 1-2 sentences, what + why
2. Tech Stack         — Language, framework, versions (detected)
3. Key Structure      — Directory layout (only non-obvious parts)
4. Principles         — 3-7 portable engineering principles
5. Commands           — Exact commands with flags (test, build, dev, lint)
6. Gotchas            — Things that will waste 30+ minutes if you don't know
7. Decision Hygiene   — Standard block from above
```

## What NOT to Include in CLAUDE.md

- Linter-enforceable rules (use `.editorconfig`, `.eslintrc` instead)
- Secrets or API keys (use `.env`)
- Obvious framework knowledge (Claude already knows React, FastAPI, etc.)
- Historical context that doesn't affect current work
- Vague instructions ("write clean code", "be careful")
- Anything over 100 lines (split into `.claude/rules/` or `@references/`)

## References

- @references/principles.md — Core principles and quality checks
- @references/skeleton.md — Universal CLAUDE.md skeleton (single source)
- @references/stack-overrides.md — Stack-specific customizations (compact)
