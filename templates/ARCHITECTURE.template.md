# [Your Project Name] — Architecture

> This file is the single source of truth for all Architect-Ops skills.
> Every rule here is enforced automatically. Keep it honest.
>
> Replace this template with your actual architecture decisions.

---

## §1. Layering Rules

Define how your codebase is structured vertically. Be explicit about what is allowed at each layer.

```
Example:
  HTTP Layer (Controllers/Routes)
    ↓ delegates to
  Service Layer (Business Logic)
    ↓ delegates to
  Repository Layer (Data Access)
    ↓ talks to
  Database / External APIs
```

**Rules:**
- Controllers receive HTTP requests and immediately delegate to services. No business logic in controllers.
- Services contain all business logic. No direct database access.
- Repositories own all data access. SQL/ORM calls live only here.
- Cross-layer imports are directional: upper layers import lower. Never the reverse.
- Cross-service communication: [events / direct import / HTTP — choose one and document it]

---

## §2. State Management

How is state managed in this system? Be explicit so agents don't introduce competing patterns.

**Client-side state:**
- Server state: [React Query / SWR / Apollo — one pattern only]
- UI/local state: [Zustand / Redux / useState — and when each is appropriate]
- Persistent state: [localStorage keys, cookie strategy]

**Server-side state:**
- Session management: [stateless JWT / server sessions]
- Cache layer: [Redis keys, TTL conventions]
- Background jobs: [queue system, job naming conventions]

---

## §3. API Conventions

**Internal API calls:**
- All HTTP requests go through `[your wrapper, e.g., src/lib/api-client.ts]`
- Never use fetch/axios directly in business code
- Retry logic lives in the API client wrapper, not in callers
- Error handling: all API errors thrown as `[your error class, e.g., AppError]`

**External API integrations:**
- Each third-party integration lives in `[directory, e.g., src/integrations/]`
- No integration SDK is used directly outside its integration module
- API keys: only read from environment, never hardcoded

---

## §4. Naming Conventions

**Files and directories:**
- Components: `PascalCase.tsx`
- Utilities/helpers: `camelCase.ts`
- Constants: `SCREAMING_SNAKE_CASE.ts`
- Test files: `*.test.ts` co-located with source

**Functions and variables:**
- Functions: `camelCase`, named for *what they do* not *how they do it*
- Boolean variables: prefix with `is`, `has`, `should` (e.g., `isLoading`, `hasPermission`)
- Event handlers: prefix with `handle` (e.g., `handleSubmit`, `handleDelete`)

**Modules:**
- Each module exports a single barrel `index.ts`
- No circular imports between modules
- Shared utilities: `src/utils/` — pure functions only, no side effects

---

## §5. Error Handling

- All errors are instances of `[your error class]` or its subclasses
- Errors are caught at the controller layer and converted to HTTP responses
- Services throw domain errors, never raw database errors
- Log errors with context: `{ error, context: { userId, operation } }`
- User-facing error messages live in `[constants file]`, not inline strings

---

## §6. Testing Strategy

- Unit tests: services and utilities — mock all dependencies
- Integration tests: repository layer — use a test database
- E2E tests: critical user flows only — run in CI on main branch
- Test files live next to source files (not in a separate `/test` top-level directory)
- Coverage threshold: [your number]% minimum on the service layer

---

## §7. Performance Rules

- Database queries: no N+1. Use joins or dataloaders.
- API responses: paginate anything that can return >100 records
- Client bundles: no library that adds >50KB gzipped without team approval
- Images: always use the image optimization pipeline, never raw `<img>` to external URLs

---

## §8. Security Constraints

- No secrets in code. All sensitive values via environment variables.
- Input validation: all user input validated at the controller layer before reaching services
- Authentication: checked in middleware, not duplicated in each route handler
- Authorization: fine-grained permissions checked in the service layer
- SQL injection: parameterized queries only, never string concatenation

---

## Change Log

| Date | Change | Author |
|---|---|---|
| YYYY-MM-DD | Initial architecture documented | @you |

---

> Last updated: [date]  
> Maintained by: [team/owner]  
> Enforced by: Architect-Ops `/reviewer` skill
