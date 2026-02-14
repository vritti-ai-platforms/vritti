---
description: Frontend patterns for Vritti microfrontends
globs: auth-microfrontend/src/**/*.{ts,tsx}, cloud-microfrontend/src/**/*.{ts,tsx}, web-nexus/src/**/*.{ts,tsx}
---

# Frontend Conventions

## Component Library
- Use components from `@vritti/quantum-ui` (shadcn-based, Tailwind v4)
- Do NOT install shadcn directly in microfrontends — use quantum-ui exports
- Import from specific paths: `import { Button } from '@vritti/quantum-ui/Button'`

## Icons
- Use lucide-react icons, NOT custom inline SVG icons
- Common status icons: `CheckCircle2` (success), `AlertCircle` (error), `Info` (info), `TriangleAlert` (warning)
- Status page icon container pattern:
  ```tsx
  <div className="flex justify-center">
    <div className="flex items-center justify-center w-16 h-16 rounded-full bg-{variant}/15">
      <IconComponent className="h-8 w-8 text-{variant}" />
    </div>
  </div>
  ```

## Alert Component
- Use quantum-ui Alert with `title` and `description` props
- NOT AlertTitle/AlertDescription as separate components
- Available variants: `default`, `destructive`, `warning`, `success`, `info`
- Pattern:
  ```tsx
  import { Alert } from '@vritti/quantum-ui/Alert';

  <Alert
    variant="destructive"
    title="Error"
    description="Error message here"
  />
  ```

## quantum-ui Initialization
Every microfrontend that uses quantum-ui axios must call `configureQuantumUI` before rendering:
```typescript
import { configureQuantumUI } from '@vritti/quantum-ui';
import quantumUIConfig from '../quantum-ui.config';

configureQuantumUI(quantumUIConfig);
```
Without this, quantum-ui falls back to defaults (wrong endpoints, wrong baseURL).

## Forms
- React Hook Form + Zod for validation
- Use `<Form>` component from quantum-ui (auto error mapping, auto loading states)
- Use `showRootError` for forms that receive general API errors
- Use `transformSubmit` when form shape differs from API DTO
- Use `rootErrorAction` for contextual actions inside the error Alert (e.g. "Login" button on 409)

## API Calls — Service → Hook → Page
- Services: pure functions wrapping axios (see `frontend-service.md`)
- Hooks: TanStack Query wrappers (see `frontend-hook.md`)
- Pages call hooks, never services directly

## Styling
- NEVER hardcode colors — use design tokens: `text-primary`, `bg-destructive/15`
- NEVER use pixels — use Tailwind classes: `p-4`, `gap-8`, `pt-16`
- Available tokens: primary, secondary, muted, accent, destructive, warning, success, foreground, background, card, border

## State Management
- AuthProvider in web-nexus for authentication state
- React Context for shared state within microfrontends
- No Redux

## Module Federation
- web-nexus is the host, loads remotes dynamically
- Microfrontends export route arrays (`authRoutes`, `accountRoutes`, `cloudRoutes`)
- Protocol auto-detected from `window.location.protocol`
- Environment variables use `PUBLIC_` prefix via `import.meta.env`
