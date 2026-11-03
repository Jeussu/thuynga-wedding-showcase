# Selected decisions and trade-offs

Each entry gives the decision, why it was made, and what it cost.

## 1. A modular monolith, not microservices

**Decision.** One ASP.NET Core API with Api, Core and Infrastructure projects and a strict dependency direction.
**Why.** One small business, one team, strongly related data and workflows that need database transactions across modules.
**Trade-off.** No independent scaling or deployment per module — not needed at this scale, and module boundaries keep a later split possible.

## 2. Same-origin cookie authentication

**Decision.** ASP.NET Core Identity with an HttpOnly cookie, antiforgery tokens on unsafe requests, and the API served under the same origin as the web app.
**Why.** No tokens readable by JavaScript and no cross-origin access to configure.
**Trade-off.** Clients must fetch and send an antiforgery token, and the reverse proxy must route `/api` correctly.

## 3. Mandatory two-factor authentication for the system administrator

**Decision.** Authenticator-app two-factor with recovery codes, enforced for the system administrator through authorization policies; optional for business administrators.
**Why.** The system administrator manages accounts and site-wide settings.
**Trade-off.** More onboarding steps for the most privileged account.

## 4. Money computed by the server; balances derived

**Decision.** All amounts are whole đồng computed on the server; the remaining balance is calculated on read and never stored.
**Why.** Removes rounding and drift between stored totals and their inputs.
**Trade-off.** A small amount of computation on every read.

## 5. Append-only payments

**Decision.** Payments are cancelled and re-recorded, never edited or deleted.
**Why.** A complete, trustworthy payment history.
**Trade-off.** Correcting a mistake takes two steps.

## 6. Quotation PDFs rendered on demand

**Decision.** Generate the PDF from current data each time; store no files.
**Why.** The PDF always matches the record, and no document files containing personal data accumulate.
**Trade-off.** Rendering cost per download, and no frozen copy of an earlier version.

## 7. Transactional outbox instead of a message broker

**Decision.** Stage e-mails in the same transaction and deliver them with an in-process background dispatcher.
**Why.** Durable delivery without operating a broker.
**Trade-off.** At-least-once delivery means a duplicate is possible; a stable message identifier mitigates it.

## 8. Fail-closed configuration

**Decision.** Outside development the API refuses to start when a development-only switch is on or a required setting is missing, and the web app refuses to build or start without its required settings.
**Why.** Misconfiguration is caught before the application serves traffic.
**Trade-off.** Stricter, more explicit deployments.

## 9. Provider-neutral hosting and e-mail

**Decision.** Hosting was chosen only when staging began and the SMTP relay during production readiness; until then the code stayed provider-neutral.
**Why.** Kept the choice open while requirements settled.
**Trade-off.** Some extra abstraction work up front.

## 10. Privacy before data volume

**Decision.** Analytics load only after consent and send a minimal event set; videos load on click; testimonials require recorded consent.
**Why.** Respect visitors and the business's legal obligations.
**Trade-off.** Less analytics data.
