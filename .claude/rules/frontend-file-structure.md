---
description: Frontend file organization — modular directories, no flat duplicates
globs: auth-microfrontend/src/**/*.ts, cloud-microfrontend/src/**/*.ts, web-nexus/src/**/*.ts
---

# Frontend File Structure

## Modular directories for hooks

One hook per file, organized in domain directories with barrel `index.ts`.

```
// CORRECT
hooks/
├── auth/
│   ├── useLogin.ts
│   ├── useSignup.ts
│   └── index.ts
├── onboarding/
│   ├── useVerifyEmail.ts
│   └── index.ts
└── index.ts          ← re-exports from subdirectories

// WRONG — flat files alongside directories
hooks/
├── auth.ts           ← duplicates auth/useLogin + auth/useSignup
├── auth/
│   ├── useLogin.ts
│   └── useSignup.ts
```

Never have both `hooks/auth.ts` and `hooks/auth/index.ts` — TypeScript resolves the file over the directory.

## Services — one file per domain

```
services/
├── auth.service.ts
├── onboarding.service.ts
├── settings.service.ts
└── index.ts
```

## Barrel exports — centralize in index.ts

```typescript
// hooks/index.ts
export { useLogin, useSignup } from './auth';
export { useVerifyEmail, useResendEmailOtp } from './onboarding';
export { useProfile, useUpdateProfile } from './useProfile';
```

Types first in re-exports:
```typescript
export { type PasswordResetFlow, type Step, useForgotPassword } from './password-reset';
```
