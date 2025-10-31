# Engineering Code Quality & Review Guidelines
**Version:** 2025-10-31  
**Applies to:** .NET 8 (C#), Vue 3 + TypeScript + Vite, Python (FastAPI), plus general repo hygiene

> Purpose: keep our codebase maintainable, consistent, and friendly to new contributors. These rules are realistic to follow day‑to‑day and tied to tooling in CI so they don’t rely on memory or vibes.

---

## 0) TL;DR (non-negotiables)
- Automated formatters & linters must pass before code review.
- All code is covered by tests at appropriate levels; changed code adds/updates tests.
- No direct pushes to `main`. Use short‑lived feature branches + PRs.
- PRs are small (ideally < 300 LOC net diff), focused, and linked to an issue.
- Reviews are respectful, constructive, and evidence‑based. Follow the checklist below.
- Security, performance, and data privacy are part of “done”, not nice‑to‑haves.

---

## 1) Repository & Branching
- **Default branch:** `main` (deployable at all times).  
- **Branching:** trunk‑based with short‑lived feature branches: `feat/<scope>`, `fix/<scope>`, `chore/<scope>`.
- **Issue links:** every PR links an issue (e.g., `Closes #123`).  
- **Conventional Commits:** `type(scope): short summary`  
  - Types: `feat, fix, docs, style, refactor, perf, test, build, ci, chore`.  
- **Protected branches:** require PR, 1+ review, status checks green, linear history.

---

## 2) Style, Formatting & Static Analysis
### Shared
- **.editorconfig** in repo root is the single source of truth for whitespace, line length (120), and EOLs.
- **Pre-commit hooks** (optional locally, enforced in CI) run formatters, linters, and secret scanning.
- **Secret safety:** never commit secrets. Use `.env.example` and a secret manager. CI blocks common keys.

### C# / .NET 8
- Enable **nullable reference types** and **treat warnings as errors** for main projects.
- Tools: `dotnet format`, Roslyn Analyzers, `StyleCop.Analyzers` (rule set tuned to be pragmatic).
- Architecture: follow **Clean/Hexagonal** boundaries; no domain logic in controllers. Dependency Injection via `IServiceCollection`.
- Naming: PascalCase for types/methods, camelCase for locals/fields, `_camelCase` for private fields.
- Exceptions: use specific exception types; don’t swallow exceptions; log with context.
- Async: use async/await end‑to‑end; never block on async (`.Result`/`.Wait()`).
- Logging: structured logs (`ILogger` with named properties), no string concatenation.

### Vue 3 + TypeScript + Vite
- Tools: ESLint (`@typescript-eslint`), TypeScript strict mode, Prettier, `vue-tsc` for type checking.
- Components: **script setup**, one component per file, **PascalCase** component names.
- State: Pinia or Vue Query; avoid ad‑hoc event buses.
- Styling: Tailwind or scoped CSS; no global CSS leaks.
- Imports: use alias `@/` for src. No deep relative spaghetti.
- API: encapsulate HTTP in services; never call `fetch` in components.

---

## 3) Testing Strategy
### Coverage & Levels
- **Targets:** line coverage ≥ 80% overall, and **changed lines** ≥ 90% (enforced in CI).  
- Test pyramid:
  - **Unit** (fast, isolated): C# xUnit + NSubstitute; JS Vitest; Python pytest.
  - **Integration**: test adapters/DB; use Testcontainers for DBs/queues.
  - **E2E/UI**: Playwright or Cypress for critical user flows (happy path + 1 edge).

### Guidelines
- Given‑When‑Then naming, one logical assertion per test (multiple checks ok if one behavior).  
- Avoid time/date flakiness: inject clocks. Avoid network calls: mock boundaries.  
- Snapshots sparingly (UI only, stable structure).

---

## 4) Documentation & Comments
- Public APIs must have usage docs or examples. Complex functions get a one‑liner “why” comment, not a novel.
- README at project root with quickstart, `make` or `just` commands, and “How to run tests”.
- ADRs (Architecture Decision Records) for significant decisions in `docs/adrs/ADR-YYYYMMDD-<slug>.md` (1 page).

---

## 5) Security, Privacy, and Dependencies
- **Dependencies:** update regularly; CI runs `npm audit`/`pnpm audit`, `pip-audit`, `dotnet list package --vulnerable`.
- **Input validation:** validate at the boundary (DTOs/Pydantic/FluentValidation).  
- **AuthN/Z:** centralized middleware/guards; no ad‑hoc checks in handlers.  
- **Logging & PII:** never log secrets or sensitive personal data.  
- **Third‑party services:** wrap SDKs behind interfaces for easy swapping & mocking.

---

## 6) Performance & Reliability
- Budget per endpoint/component documented (latency, memory, payload sizes).  
- Avoid N+1 queries; prefer pagination and projection (DTOs).  
- Caching policy defined per service (in‑memory vs. Redis) with explicit TTLs.  
- Feature flags for risky changes; circuit breakers for network calls.

---

## 7) Code Review Policy
### PR Size & Scope
- Prefer PRs ≤ 300 LOC net diff; if larger, explain why and how to review.  
- No “mixed changes”: refactor vs feature should be separate PRs.

### What Authors Must Do
- Fill PR template, including risk, test plan, and screenshots for UI.
- Ensure all checks green; re‑request review after addressing feedback.
- Mark non‑functional changes clearly (e.g., formatting only).

### What Reviewers Look For (Checklist)
- **Correctness:** requirements met, specs covered by tests.  
- **Clarity:** names clear, functions short, cyclomatic complexity reasonable.  
- **Consistency:** matches existing patterns.  
- **Safety:** error handling, nullability, edge cases.  
- **Security/Privacy:** authz, input validation, secrets.  
- **Performance:** allocations, DB calls, N+1, unnecessary I/O.  
- **DX:** logs/metrics in place, docs updated.  
- **Tests:** meaningful, deterministic, boundaries mocked, coverage thresholds met.

### How to Give Feedback
- Be kind, be specific, be actionable. Prefer questions (“what about…”) over commands.
- Disagree? Provide references (docs, benchmarks, style guide). Escalate to ADR if needed.
- Approvals: “LGTM” only if you’d be happy to own the code later.

---

## 8) CI/CD Gates (enforced)
- Formatters & linters pass (dotnet/ESLint/black+ruff+mypy).  
- Tests pass; coverage thresholds met (overall and changed lines).  
- Secret scan: pass (detect common keys).  
- Build artifacts produced; container images scan clean (critical vulns fail).

---

## 9) Project Scaffolding (recommended)
- `docs/` (handbook, ADRs), `.github/` (PR template, workflows), `tools/` (scripts), `Makefile` or `justfile`.  
- Monorepo: use workspace tooling (pnpm workspaces, Poetry, or .NET solution filters).

---

## 10) Ownership & Where This Lives
- This document **lives in each repo** at `docs/code-quality/Guidelines.md` and is linked from `README.md`.  
- Org‑wide mirror lives in the knowledge base (e.g., Confluence/SharePoint) with a short link.  
- **CODEOWNERS** defines reviewers per area; bots enforce checks.

---

## 11) Exceptions
- Temporary exceptions must be documented in the PR description with an expiry issue created.  
- CI “allow‑fail” jobs require an issue link and a plan to remove the allowance.

---

## 12) Appendix — Tooling Quickstart

### .NET
```bash
dotnet tool restore
dotnet format --verify-no-changes
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=cobertura
dotnet list package --vulnerable
```

### Vue + TS
```bash
pnpm install
pnpm lint && pnpm typecheck && pnpm test
pnpm build
```
---
