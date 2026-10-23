# Testing strategy and CI/CD

## Test layers

| Layer | Tooling | What it covers |
|---|---|---|
| Backend unit | xUnit | Domain rules, validators, formatting, PDF models, security helpers |
| Backend integration | xUnit, real API host, real SQL Server | Endpoints end to end: authorization, transactions, concurrency, idempotency, audit |
| Frontend unit | Node.js test runner | Form logic, formatting, analytics, security policy, routing helpers |
| End to end | Playwright (Chromium) | User journeys against the real API and a production build of the web app, at desktop (1440 px) and phone (375 px and 360 px) widths |

**Test inventory at the production release** (counts of test files and test methods, not executions):

| | |
|---|---|
| Backend test-project files | 265 C# files (96 unit, 169 integration), including shared test infrastructure |
| xUnit test methods (`[Fact]` / `[Theory]`) | about 1,950 |
| Frontend unit-test files | 57 |
| Playwright spec files | 60 |

## Principles

- **Real database over mocks** for anything transactional. Integration tests run against SQL Server, using a database generated for each run.
- **Fail-closed test infrastructure.** A guard tool refuses to run destructive tests against any database the run did not create.
- **Prove the security policy.** End-to-end runs observe every page and fail if the Content-Security-Policy is violated.
- **Test configuration refusals.** CI checks that production builds refuse missing or malformed production settings and test keys.

## CI pipeline (GitHub Actions)

Runs on every pull request and on pushes to the main and feature branches.

1. **Backend:** pinned SDK, locked restore, Release build, a check that the migrations match the model, NuGet vulnerability audit, unit tests, a throwaway SQL Server container, integration tests.
2. **Frontend:** `npm ci`, `npm audit`, lint, typecheck, unit tests, production build, configuration-refusal checks.
3. **End to end:** builds of the API and the web app, a throwaway SQL Server, the database-guard checks, then the desktop and phone Playwright journeys.

Actions are pinned to full commit SHAs and run with a read-only token. CI stores no secrets and performs no deployment.

## Delivery method

- Each milestone is delivered through a pull request and merged only after CI passes on the exact commit.
- Deployment is a separate, deliberately approved step; a release is identified by its exact commit SHA.
- A staging environment, user acceptance testing and business acceptance came before the public go-live.
