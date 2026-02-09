# Vritti Workspace

This is a multi-project workspace. Each subfolder is an independent git repo.

## Project Map

| Folder | Stack | Purpose |
|--------|-------|---------|
| `api-nexus/` | NestJS + Fastify + Prisma | Backend API |
| `api-sdk/` | NestJS library | Shared server SDK (@vritti/api-sdk) |
| `auth-microfrontend/` | React + Vite | Auth UI microfrontend |
| `cloud-microfrontend/` | React + Vite | Cloud dashboard microfrontend |
| `quantum-ui/` | React + Tailwind v4 | Shared component library (@vritti/quantum-ui) |
| `web-nexus/` | React + Vite | Host/container app |
| `developer-docs/` | Mintlify | Developer documentation site |
| `infrastructure/` | Terraform/Docker | Infrastructure as code |
| `landing/` | Next.js | Marketing landing page |

## Subagent Routing

When working across projects, use the appropriate specialized agent:
- **api-nexus work** → `vritti-api-nexus-agent`
- **quantum-ui work** → `quantum-ui-architect`
- **@vritti/api-sdk work** → `api-sdk-maintainer`
- **Frontend UI work** (auth-microfrontend, cloud-microfrontend, web-nexus) → `vritti-ui-builder`
- **Postman collection sync** → `postman-collection-syncer`

## Cross-Project Conventions

See `.claude/rules/` for detailed pattern documentation:
- `swagger-docs.md` — API controller Swagger docs pattern
- `error-handling.md` — RFC 9457 exception patterns
- `auth-architecture.md` — Auth system facts and constraints
- `frontend-conventions.md` — Frontend patterns and component usage
