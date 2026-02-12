# Init Project — Universal Skeleton

모든 기술 스택에 공통으로 적용되는 CLAUDE.md, plan.md, codify.md 뼈대.

## CLAUDE.md Skeleton

Generate by filling in `[placeholders]` based on detected stack. Apply stack-specific overrides from `@references/stack-overrides.md` to the Principles and Commands sections.

```markdown
# [Project Name]

[1-2 sentence purpose — what it does and why it exists]

## Stack
[Detected stack — language, framework, versions, package manager, test framework, DB if any]

## Structure
[Only non-obvious directories. Skip standard ones like node_modules/, .git/, __pycache__/]

## Principles
[Pick 3-7 from principles.md that fit this stack. Add stack-specific ones from overrides.]

## Commands
[Detected commands with exact flags — test, dev, build, lint, format]

## Gotchas
[Project-specific gotchas found during analysis. Include friction signals detected.]

## Decision Hygiene (결정 위생)
- "I'll just..." / "for now..." → 멈추고 진짜 결정을 내리세요
- 새 모듈/디렉토리 생성 시 → "비슷한 코드가 어디 있나?" 먼저 확인
- 커밋 전: 어떻게 고장 났는지 알 수 있나? 항상 참이어야 할 것은? 왜 이렇게 결정했나?

## References
- plan.md — Architecture and roadmap
- codify.md — 배운 것들 (learnings, append-only)
```

---

## plan.md Skeleton

```markdown
# Plan — [Project Name]

## Effectiveness Contract (효과성 계약)
- **Outcome**: [1 sentence — what success looks like]
- **Evidence**: [how we prove it worked]
- **Non-goals**: [what we won't touch]
- **Risks**: [top 2 things that could go wrong]
- **Rollback**: [how to undo if it fails]
- **Timebox**: [when to reassess approach]

## Architecture Decisions
- **[Decision]**: [Rationale] (YYYY-MM-DD)
  - Context: [what led to this]
  - Options considered: [alternatives]
  - Why this: [reasoning]

## Current Phase
[What's being actively worked on]

## Phases

### Phase 1: [Name]
- [ ] Task 1
- [ ] Task 2

## Open Questions
- [ ] [Unresolved decision or question]

## Friction Log (마찰 기록)
<!-- Record friction signals as you encounter them -->
<!-- Example: 2026-02-12: Can't explain auth flow in 1 minute → needs simplification -->
```

---

## codify.md Skeleton

```markdown
# Codify - 배운 것들

> Append-only. 삭제하지 마세요. 새로운 발견은 아래에 추가.
> 잘못된 정보는 삭제 대신 정정 항목을 추가하세요.

---

(아직 학습 기록이 없습니다. 프로젝트를 진행하면서 발견한 것들을 여기에 기록하세요.)

<!-- 템플릿:
## [YYYY-MM-DD]: [Topic]
### 발견 (Finding)
- **상황 (Situation)**: When/where this applies
- **방법 (Method)**: How to do it
- **예시 (Example)**: Code or concrete example
- **주의 (Warning)**: Common mistakes to avoid
-->
```

---

## .claude/rules/ Templates (Optional, for complex projects)

Generate these only when the project has enough complexity to warrant splitting. Signal: CLAUDE.md exceeding 80 lines.

### testing.md
```markdown
---
globs: ["tests/**", "**/*test*", "**/*spec*"]
---
# Testing Conventions
[Stack-specific testing rules]
```

### api-patterns.md
```markdown
---
globs: ["src/routers/**", "src/api/**", "app/api/**"]
---
# API Conventions
[Stack-specific API rules]
```
