---
description: Backend controller conventions — thin HTTP layer, no business logic
globs: api-nexus/src/**/*.controller.ts
---

# Backend Controller Files

Controllers are a thin HTTP layer. They handle request/response concerns ONLY.

## Allowed in controllers

- Logging (`this.logger.log(...)`)
- Calling ONE service method and returning the result
- Fastify response operations (`reply.setCookie`, `reply.clearCookie`)
- Extracting values from request (headers, cookies) to pass to service

## NOT allowed in controllers

- Conditional logic / if-else branching on business rules
- Multiple service calls orchestrated together
- Data transformation / mapping (use DTOs in service layer)
- Throwing exceptions based on business logic (move to service)
- Array operations (`.find()`, `.filter()`, `.map()`)
- Direct database access

## Pattern

```typescript
// WRONG — business logic in controller
@Delete('sessions/:id')
async revokeSession(@UserId() userId: string, @Param('id') sessionId: string, @Req() request: FastifyRequest) {
  const currentSession = await this.sessionService.validateAccessToken(accessToken);
  if (currentSession.id === sessionId) {
    throw new BadRequestException('Cannot revoke current session');
  }
  const sessions = await this.sessionService.getUserActiveSessions(userId);
  const targetSession = sessions.find((s) => s.id === sessionId);
  if (!targetSession) throw new NotFoundException('Session not found');
  await this.sessionService.invalidateSession(targetSession.accessToken);
  return { message: 'Session revoked' };
}

// CORRECT — all logic in service
@Delete('sessions/:id')
async revokeSession(@UserId() userId: string, @Param('id') sessionId: string, @Req() request: FastifyRequest) {
  this.logger.log(`DELETE /auth/sessions/${sessionId}`);
  const accessToken = request.headers.authorization?.replace('Bearer ', '') || '';
  return this.sessionService.revokeSessionById(userId, sessionId, accessToken);
}
```

## Use decorators for request data — no direct header/cookie access

```typescript
// WRONG — accessing request directly
async logout(@Req() request: FastifyRequest) {
  const accessToken = request.headers.authorization?.replace('Bearer ', '') || '';
}

// CORRECT — use @AccessToken() decorator from @vritti/api-sdk
async logout(@AccessToken() accessToken: string) { ... }
```

Available decorators from `@vritti/api-sdk`:
- `@UserId()` — extracts user ID from JWT (set by auth guard)
- `@AccessToken()` — extracts bearer token from Authorization header
- `@RefreshTokenCookie()` — extracts refresh token from httpOnly cookie
- `@Public()` — skips auth guard
- `@Onboarding()` — allows onboarding session tokens

Use `@Req()` only when no decorator exists (e.g., reading cookies by name).

## Import exceptions from `@vritti/api-sdk`

```typescript
// WRONG
import { BadRequestException } from '@nestjs/common';

// CORRECT
import { BadRequestException } from '@vritti/api-sdk';
```
