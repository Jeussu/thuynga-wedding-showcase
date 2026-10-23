# Security, privacy and reliability

This page describes practices, not configuration. Thresholds, allow-lists, network details and operational procedures are intentionally omitted.

## Identity and access

- ASP.NET Core Identity with HttpOnly, Secure cookies.
- Authenticator-app two-factor authentication with recovery codes: mandatory for the system administrator, optional for business administrators.
- Deny by default: every endpoint requires an authenticated user unless explicitly marked anonymous, and an automated sweep checks the authorization of every endpoint.
- Sessions are re-validated on every request, so deactivating an account, resetting a password or changing two-factor settings ends existing sessions at once.

## Request protection

- Antiforgery tokens on every state-changing request.
- Bot protection on public forms, verified before the request body is processed.
- Rate limiting on public submissions and account flows.
- Forwarded headers accepted only from the trusted reverse proxy.

## Browser hardening

- An enforcing Content-Security-Policy with a fresh nonce per document, with a stricter policy for the admin area.
- HSTS, `X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy` and `Permissions-Policy`. These response headers can be observed on the live website.
- Security headers on every API response, including errors.

## Privacy and data protection

- Notification e-mails carry no customer data.
- Private images are never public and are streamed only to signed-in administrators.
- Public media is stored without embedded metadata.
- Analytics built to load only after consent (switched off at launch); click-to-load video embeds; a privacy policy page.
- Documented data-protection procedures were adopted before go-live.

## Configuration and secrets

- No secrets in the repository; configuration comes from the environment.
- Fail-closed startup: the API refuses to run with development-only switches or missing production settings, and the web app refuses to build or start without its required settings.
- Data Protection keys persisted separately per environment.
- CI uses throwaway database credentials that are generated per run and masked in logs.

## Supply chain

- Pinned SDK and toolchain versions, central package management and locked restores.
- NuGet and npm vulnerability audits in CI.
- GitHub Actions pinned to full commit SHAs, with a least-privilege workflow token.
- A framework security update (Next.js 16.4.2) was shipped to production the day after go-live.

## Reliability and operations

- A health-check endpoint.
- CI verifies that database migrations match the model.
- Database migration and backup/restore procedures were rehearsed before go-live.
- A staging environment with production-like configuration hosted user acceptance testing.
- The production origin stayed behind a protected preview until launch.
- Each release is identified by its exact commit, and the previous release is kept available for rollback.
