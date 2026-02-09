---
description: Frontend patterns for Vritti microfrontends
globs: auth-microfrontend/src/**/*.{ts,tsx}, cloud-microfrontend/src/**/*.{ts,tsx}, web-nexus/src/**/*.{ts,tsx}
---

# Frontend Conventions

## Component Library
- Use components from `@vritti/quantum-ui` (shadcn-based, Tailwind v4)
- Do NOT install shadcn directly in microfrontends — use quantum-ui exports

## Forms
- React Hook Form for form state management
- Zod for validation schemas
- `mapApiErrorsToForm` utility to map API errors to form fields

## API Calls
- Axios with interceptors from quantum-ui
- API error responses follow RFC 9457 Problem Details format

## State Management
- AuthProvider in web-nexus for authentication state
- React Context for shared state within microfrontends
- No Redux — keep it simple

## Documentation (developer-docs)
- Mintlify MDX files with `---` frontmatter (title, description, icon)
- Components: `<Info>`, `<Warning>`, `<Note>`, `<Steps>`, `<Accordion>`, `<CardGroup>`, `<Card>`
- Mermaid diagrams for flows, tables for schemas/endpoints
- `docs.json` uses tab indentation
