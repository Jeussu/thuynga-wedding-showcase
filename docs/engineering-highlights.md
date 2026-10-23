# Backend engineering highlights

## Correct workflows under concurrency

- **Exactly-once lead conversion.** Converting a lead into a customer and an event happens once per lead. Possible duplicate customers (matched by phone) are shown as suggestions that are never preselected, and the duplicate acknowledgement is race-safe.
- **Idempotent public submissions.** Quote and consultation requests carry a submission key, so a retried or double-clicked submission does not create a second lead.
- **Optimistic concurrency.** Quotations and payments carry a version check, and status changes on leads, events and appointments are concurrency-checked, so two administrators cannot silently overwrite each other.
- **Money invariants enforced by the server.** Totals, discounts and deposits are computed on the server in whole đồng. A payment is accepted only against an accepted quotation and never beyond its total; an excess is refused rather than reduced.
- **Derived, not stored, balances.** The remaining balance is computed on every read from the current accepted quotation and the standing payments, so it cannot drift.

## Append-only history

- Lead activity is an append-only timeline.
- Payments are never edited or deleted: a mistaken payment is cancelled, kept in the history and recorded again.
- Administrative actions are written to an audit log whose metadata is sanitized before it is stored.

## Documents and media

- **Quotation PDFs** are generated with QuestPDF, embedding open-licensed fonts so Vietnamese diacritics render correctly, and are produced on demand instead of being stored.
- **Media pipeline** with whole-batch gallery uploads and cover selection. Public media is stored without embedded metadata; private images are streamed only through the authenticated API.

## Reliable notifications

- Notification e-mails are staged in the same transaction as the request that triggers them and delivered by an in-process background dispatcher, at least once, with a stable message identifier.
- E-mails to the business carry no customer data; the details stay behind the admin sign-in.
- Password-reset e-mails are handed off in the background, so the anonymous request never waits for the mail transport.

## A consistent API contract

- Errors use RFC 7807 Problem Details; the API answers 401 or 403 and never redirects.
- Every endpoint requires an authenticated user unless explicitly marked anonymous, and a test sweeps every endpoint to confirm its authorization.

## Build hygiene

- Central package management with every NuGet version pinned, and locked-mode restores in CI.
- Compiler warnings treated as errors.
- A pinned .NET SDK and Node.js toolchain.

## Safe test infrastructure

Integration and end-to-end runs use a freshly generated test database per run, behind a guard tool that fails closed before any test can touch a database it did not create.
