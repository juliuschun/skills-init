# Init Project — Principles Library

원칙 기반 CLAUDE.md를 위한 핵심 원칙 모음.

## First Principles (근본 원칙)

These three truths govern all CLAUDE.md design:

1. **LLM은 상태가 없다 (Stateless)** — CLAUDE.md is the ONLY persistent memory across sessions. Everything not written here is forgotten.
2. **토큰은 유한하다 (Tokens are finite)** — Every line in CLAUDE.md steals from working context. Unnecessary context actively degrades performance.
3. **원칙은 규칙보다 이식 가능하다 (Principles > Rules)** — "Never swallow errors" works everywhere. "Log to /var/log" breaks when the environment changes.

## Universal Engineering Principles (범용 공학 원칙)

Pick 3-7 that apply to the project. Don't include all of them — that defeats the purpose.

| Category | Principle (EN) | 원칙 (KR) |
|----------|---------------|-----------|
| Error Handling | Never swallow errors; always log and propagate | 에러를 삼키지 마세요; 항상 로깅하고 전파 |
| Error Handling | Fail fast at boundaries, recover in core | 경계에서 빨리 실패, 핵심에서 복구 |
| Immutability | Prefer new values over mutations | 변경보다 새로운 값 반환 선호 |
| Simplicity | Don't abstract one-time operations | 일회성 작업에 추상화 금지 |
| Simplicity | Three similar lines > premature abstraction | 유사한 3줄 > 성급한 추상화 |
| Testing | Write tests before implementation (TDD) | 구현 전 테스트 먼저 |
| Testing | Mock external APIs; never call real services | 외부 API 모킹; 실제 서비스 호출 금지 |
| File Mgmt | Edit existing files; new files are last resort | 기존 파일 수정 우선 |
| File Mgmt | ONE file per concern, no `_v2` variants | 관심사당 하나의 파일 |
| Security | Secrets in .env, never in code or CLAUDE.md | .env에 시크릿 보관 |
| Security | Validate at system boundaries only | 시스템 경계에서만 검증 |
| Readability | Comments only where logic isn't self-evident | 자명하지 않은 로직에만 주석 |
| Readability | Type hints on all function signatures | 모든 함수에 type hints |

## Quality Checks (품질 검사)

Run these before finalizing any CLAUDE.md:

| Check | Question | Fail Action |
|-------|----------|-------------|
| **Deletion test** | 이 줄을 삭제해도 Claude 동작이 변하나요? | 변하지 않으면 삭제 |
| **Linter test** | 린터가 이미 다루는 규칙인가요? | 삭제, `.editorconfig` 참조 |
| **Obviousness test** | Claude가 이미 아는 지식인가요? | 삭제 |
| **Portability test** | 환경이 바뀌어도 유효한가요? | 원칙으로 재작성 |
| **Length check** | 100줄 미만인가요? | `.claude/rules/`로 분리 |
| **Gotcha test** | 이걸 모르면 30분+ 낭비하나요? | 포함 |

## Package Manager Defaults

| Language | Default PM | Notes |
|----------|-----------|-------|
| Python | `uv` | `pip`/`python` 직접 호출 금지 |
| JS/TS | Detect from lockfile | `package-lock.json`→npm, `pnpm-lock.yaml`→pnpm, `bun.lockb`→bun |
| Rust | `cargo` | — |
| Go | `go` | — |

## Memory Hierarchy (메모리 계층)

```
Level 1: CLAUDE.md            ← 범용 규칙 (auto-loaded every session)
Level 2: .claude/rules/*.md   ← 도메인별 규칙 (loaded by path glob)
Level 3: plan.md              ← 현재 진행 상태 (manual reference)
Level 4: codify.md            ← 역사적 학습 (manual reference)
Level 5: Auto memory          ← Claude 자체 학습 (~/.claude/projects/)
```

Level 1 loads automatically. Levels 2+ are loaded on demand.
