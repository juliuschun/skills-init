# Init Project — Stack-Specific Overrides

기술 스택별 차이점만 정의. 뼈대(skeleton.md)에 적용합니다.

Only load the relevant section based on detected stack. Each override provides:
- Extra Principles (stack-specific, added to universal ones)
- Commands (exact commands with flags)
- Common Gotchas (stack-specific)

---

## Python + FastAPI

**Package Manager**: `uv` (pip/python 직접 호출 금지)

**Extra Principles**:
- 모든 함수에 type hints 필수
- async/await for all I/O operations
- Pydantic v2 models for request/response schemas

**Commands**:
```
- `uv run pytest` — Run all tests
- `uv run pytest tests/test_specific.py -v` — Run specific
- `uv run python -m uvicorn src.main:app --reload --port 8000` — Dev server
- `uv run ruff check .` — Lint
- `uv run ruff format .` — Format
```

**Common Gotchas**:
- `uv run` prefix required for all Python commands
- Pydantic v2 has breaking changes from v1 (model_validate, not parse_obj)
- FastAPI depends on parameter order in route decorators

---

## TypeScript + React (Vite)

**Extra Principles**:
- Named exports only (no default exports)
- ES modules throughout
- Strict TypeScript — zero `any`, zero `as` casts
- Components: function declarations, not arrow functions
- 불변 패턴 선호; 상태 변경은 명시적으로

**Commands**:
```
- `npm test` — Run all tests
- `npm run dev` — Start dev server
- `npm run build` — Production build
- `npm run lint` — ESLint check
- `npm run typecheck` — tsc --noEmit
```

---

## Next.js (App Router)

**Extra Principles**:
- Server Components by default; 'use client' only when needed
- Server Actions for mutations (not API routes)
- Strict TypeScript — zero `any`
- 에러를 삼키지 마세요; error.tsx로 바운더리 처리

**Commands**:
```
- `npm run dev` — Dev server (port 3000)
- `npm test` — Vitest
- `npx playwright test` — E2E tests
- `npm run build` — Production build (catches type errors)
```

**Common Gotchas**:
- `'use client'` must be first line in file, before imports
- Server Actions can't return non-serializable values
- Middleware runs on edge runtime (limited Node.js APIs)

---

## Rust

**Extra Principles**:
- 소유권과 수명은 명시적으로; .clone() 남용 금지
- Result<T, E> for all fallible operations; no unwrap() in production
- Error types: thiserror for libraries, anyhow for binaries

**Commands**:
```
- `cargo test` — Run all tests
- `cargo run` — Run binary
- `cargo clippy` — Lint
- `cargo fmt` — Format
```

**Common Gotchas**:
- Orphan rule: can't impl external trait for external type
- Async Rust: must pick runtime (tokio vs async-std), can't mix

---

## Go

**Extra Principles**:
- 에러는 즉시 처리하세요; if err != nil 패턴 준수
- Accept interfaces, return structs
- Table-driven tests

**Commands**:
```
- `go test ./...` — Run all tests
- `go build ./cmd/...` — Build
- `golangci-lint run` — Lint
```

**Common Gotchas**:
- nil interface vs nil pointer: `var x *T = nil; var i interface{} = x; i != nil` is true
- goroutine leaks: always ensure goroutines can exit

---

## Swift / SwiftUI

**Extra Principles**:
- Value types (struct) by default; class only for reference semantics
- @Observable over ObservableObject (iOS 17+)
- Strict concurrency: Sendable conformance, no data races

**Commands**:
```
- `swift test` — Run tests
- `swift build` — Build
- `swift run` — Run
```

---

## Flutter / Dart

**Extra Principles**:
- Immutable widgets; state in StatefulWidget or providers
- const constructors wherever possible
- Strong typing — no dynamic unless absolutely needed

**Commands**:
```
- `flutter test` — Run tests
- `flutter run` — Run on device/simulator
- `flutter analyze` — Lint
- `dart format .` — Format
```
