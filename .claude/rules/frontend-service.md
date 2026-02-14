---
description: Frontend service file conventions
globs: auth-microfrontend/src/services/**/*.ts, cloud-microfrontend/src/services/**/*.ts, web-nexus/src/services/**/*.ts
---

# Frontend Service Files

Services are pure functions that wrap axios calls. They live in `src/services/<domain>.service.ts` and are consumed by hooks (`useMutation`/`useQuery`).

## No async/await

Services wrap a single axios call — no need for async/await. Return the promise chain directly with `.then((r) => r.data)`. Use the axios generic type parameter for response typing.

```typescript
// WRONG
export async function login(data: LoginDto): Promise<LoginResponse> {
  const response: AxiosResponse<LoginResponse> = await axios.post(
    "cloud-api/auth/login", data, { public: true },
  );
  return response.data;
}

// CORRECT
export function login(data: LoginDto): Promise<LoginResponse> {
  return axios
    .post<LoginResponse>("cloud-api/auth/login", data, { public: true })
    .then((r) => r.data);
}
```

For void returns, use `.then(() => undefined)`.

## No AxiosResponse import

The generic type on `axios.get<T>()` / `axios.post<T>()` handles typing. No need to import `AxiosResponse`.

## `export function` for services

```typescript
// WRONG
export const login = (data: LoginDto): Promise<LoginResponse> => { ... }

// CORRECT
export function login(data: LoginDto): Promise<LoginResponse> { ... }
```
