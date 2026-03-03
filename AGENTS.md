# Vritti Workspace (Codex Rules)

This is the Codex equivalent of the Claude workspace guidance.

## Project Map

| Folder | Stack | Purpose |
|--------|-------|---------|
| `cloud-server/` | NestJS + Fastify + Drizzle | Cloud backend API |
| `api-nexus/` | NestJS + Fastify + Drizzle | Nexus backend API (web-nexus) |
| `api-sdk/` | NestJS library | Shared server SDK (`@vritti/api-sdk`) |
| `cloud-web/` | React + Rsbuild | Standalone auth + account web app |
| `cloud-microfrontend/` | React + Vite | Cloud dashboard microfrontend |
| `quantum-ui/` | React + Tailwind v4 | Shared component library (`@vritti/quantum-ui`) |
| `web-nexus/` | React + Vite | Host/container app |
| `developer-docs/` | Mintlify | Developer documentation site |
| `infrastructure/` | Terraform/Docker | Infrastructure as code |
| `landing/` | Next.js | Marketing landing page |

## Scope and Precedence

- Apply these rules when working anywhere in this workspace.
- Treat `.claude/rules/*.md` as canonical pattern docs unless a task explicitly asks to deviate.
- If two rules conflict, prefer the more specific file-level rule over general guidance.

## Rule Index (Canonical Sources)

- API auth architecture facts: `.claude/rules/auth-architecture.md`
- Backend controller conventions: `.claude/rules/backend-controller.md`
- Backend DTO conventions: `.claude/rules/backend-dto.md`
- Backend module structure: `.claude/rules/backend-module-structure.md`
- Backend repositories: `.claude/rules/backend-repository.md`
- Backend repository query strategy: `.claude/rules/backend-repository-queries.md`
- Backend services: `.claude/rules/backend-service.md`
- RFC 9457 error handling: `.claude/rules/error-handling.md`
- Swagger docs pattern: `.claude/rules/swagger-docs.md`
- Export conventions: `.claude/rules/export-conventions.md`
- Frontend conventions: `.claude/rules/frontend-conventions.md`
- Frontend file structure: `.claude/rules/frontend-file-structure.md`
- Frontend hooks: `.claude/rules/frontend-hook.md`
- Frontend services: `.claude/rules/frontend-service.md`
- Comment style: `.claude/rules/comment-style.md`

## Backend Guardrails (Codex)

- Keep controllers thin: logging, request extraction, one service call, response wiring only.
- Put business rules, branching, orchestration, and exception decisions in services.
- Repositories do data access only. No business validation in repositories.
- Prefer `this.model` in repositories for simple equality CRUD. Use `this.db` only for complex predicates, aggregations, joins, or atomic SQL expressions.
- Use exceptions from `@vritti/api-sdk`, not `@nestjs/common`, for API errors.
- Keep module layout folder-based (`controllers/`, `services/`, `repositories/`, `dto/`, `docs/`).
- Use request/response/entity DTO split and explicit controller return types.
- Keep Swagger decorators in `docs/*.docs.ts` helpers, not inline on controller methods.

## Frontend Guardrails (Codex)

- Use `@vritti/quantum-ui` components, not ad hoc HTML alternatives for covered UI primitives.
- Use `<Button>` instead of raw `<button>` and `<Spinner>` instead of `Loader2`.
- Keep API flow `services -> hooks -> pages`; pages should not call service functions directly.
- Services are pure axios wrappers and return promise chains directly.
- Hooks are thin TanStack Query wrappers and typically use `AxiosError` as error type.
- Follow domain-based file organization and specific-path imports; avoid top-level barrel imports.
- Use design tokens and Tailwind utility scale rather than hard-coded color values and pixel literals.

## Comment and Export Style

- Use single-line `//` comments for method-level intent; avoid multi-line JSDoc blocks except `@deprecated`.
- Add one concise method comment where required by the style rule; skip trivial constructors/getters.
- Use `export function` for functions/hooks/services/utilities.
- Use `export const` for React components and constant values.

## Auth and Security Constraints

- Keep established auth facts unchanged unless explicitly requested: token recovery route, session expiry behavior, refresh-token cookie policy, JWT payload shape, OAuth state storage, MFA challenge storage, and cookie env configuration helpers.
- When touching auth code in `api-nexus`, re-check `.claude/rules/auth-architecture.md` before editing.

## Practical Workflow for Codex

- Before implementing in backend/frontend files, open the matching rule doc from `.claude/rules/` and apply its pattern exactly.
- Prefer minimal diffs that preserve existing architecture.
- If a task requires breaking a rule, call it out explicitly in the PR/summary with rationale.
