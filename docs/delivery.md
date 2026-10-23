# Delivery, milestones and lessons learned

## Milestones

The project was delivered as milestones M0–M11 — a foundation milestone followed by eleven delivery milestones — between September and October 2026. Each one had written scope with an explicit out-of-scope list, was delivered through pull requests with passing CI, and was formally accepted.

| Milestone | Scope |
|---|---|
| M0 | Platform foundation: solution layout, toolchain pinning, CI, authentication topology, technical spikes (media, bot protection, PDF) |
| M1 | Authentication and admin shell: sign-in, password recovery, two-factor authentication, user management, audit log |
| M2 | Services and basic CMS: service categories, services, site-wide content |
| M3 | Portfolio and media: project albums, batch uploads, public project pages |
| M4 | Public website: home page, guides, testimonials, contact, SEO foundations |
| M5 | Quote requests and lead CRM |
| M6 | Customers, events, appointments, consultation booking and calendar |
| M7 | Quotations, pricing and PDF output |
| M8 | Payment tracking and remaining balance |
| M9 | Notifications, e-mail delivery and SEO completion, consent-based analytics |
| M10 | Security hardening, then staging with user acceptance testing |
| M11 | Production readiness, public go-live and stabilization |

## Path to production

1. **Hardening** — security headers and policy, fail-closed configuration, rehearsed migration and backup/restore procedures.
2. **Staging** — production-like environment, acceptance checks and user acceptance testing.
3. **Production readiness** — production profile, notification-delivery checks and a protected preview of the production site.
4. **Acceptance** — business acceptance by the client and final acceptance before launch, together with a privacy policy and adopted data-protection procedures.
5. **Go-live** — public launch on 22 October 2026, followed by a stabilization check.
6. **Post-launch** — a Next.js security update shipped to production on 23 October 2026.

## Lessons learned

- **Out-of-scope lists matter as much as scope.** Writing down what each milestone would *not* do (for example, no payment gateway) kept the delivery focused.
- **Make misconfiguration impossible to ignore.** An application that refuses to start without its required settings turns a checklist into an automated gate.
- **Use real databases for transactional rules.** Concurrency, idempotency and money invariants were verified against SQL Server, not mocks.
- **Privacy work needs lead time.** Consent, data-protection procedures and the privacy policy had to be ready before launch, not after.
- **Deferring provider choices has a cost and a benefit.** Staying provider-neutral kept options open, at the price of some abstraction work.
- **AI-assisted development.** Development was supported by Claude Code, with independent AI-assisted technical reviews. Project decisions, manual QA, release approvals, and final acceptance remained under human ownership.
