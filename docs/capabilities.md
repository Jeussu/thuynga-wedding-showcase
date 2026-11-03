# Business capabilities

The platform has two faces: a public Vietnamese-language website for prospective customers, and an admin area used by the business to run its sales pipeline and events.

## Public website

- **Home page** managed from the CMS: hero section with an optional short intro video that is skipped under reduced-motion, data-saver or slow-connection conditions; featured services; latest projects; a gallery of real work.
- **Services** grouped by category, each with its own detail page.
- **Completed projects** with multi-image albums and a lightbox.
- **Guides** (articles) and **customer testimonials**; testimonials are published only with recorded consent.
- **Funeral services** section with its own restrained visual theme, kept out of the main header.
- **Contact** page, **privacy policy** page and click-to-load video embeds.
- **Quote request** and **consultation booking** forms, both protected against bots and duplicate submissions.
- **SEO**: canonical URLs, a sitemap that fails closed, structured data limited to verified facts, and editable SEO titles and descriptions for services and projects.
- **Analytics** built to load only after the visitor consents, with a minimal event set; switched off at launch until the business chooses to enable it.

## Lead CRM

- A single inbox for quote requests and consultation requests.
- A controlled status workflow and an append-only activity timeline per lead.
- Duplicate-phone warnings and narrowly scoped corrections of name and phone.
- Per-user in-app notifications, plus an e-mail notification to the business that carries no customer data.

## Customers, events and appointments

- An explicit **lead-to-customer conversion** that creates the customer and the event exactly once, with phone-number matches offered as suggestions and never preselected.
- **Customers** can be created and edited, never merged or deleted.
- **Events** follow their own status workflow and can be reopened.
- **Appointments** (consultations and site surveys) can be confirmed, declined, cancelled, completed or rescheduled.
- A **month calendar** of scheduled work.

## Quotations

- A quotation belongs to exactly one lead or one event and never changes owner.
- Free-text line items with quantity and unit price, one fixed discount, and an optional required deposit (none, a percentage or a fixed amount).
- All money is in whole Vietnamese đồng and computed on the server.
- Numbering per Vietnam calendar year and a status workflow (draft → sent → accepted or declined), with at most one accepted quotation per deal.
- Edited in place under an optimistic-concurrency check, never deleted.
- Rendered on demand as a PDF with Vietnamese typography; the PDF is not stored.

## Payment tracking

- Records money the customer has already paid (deposit, partial or final payment; bank transfer, cash or other).
- Allowed only against an accepted quotation and never beyond its total — an excess amount is refused, not reduced.
- Payments are never edited or deleted; a mistake is cancelled, kept in the history and recorded again.
- An optional private photo of the receipt, visible only to signed-in administrators.
- The remaining balance is derived from the accepted quotation and the standing payments on every read.

## Administration

- Two roles: a system administrator and business administrators.
- Two-factor authentication with an authenticator app and recovery codes; mandatory for the system administrator, optional for business administrators.
- Account management, password recovery, CMS screens for services, projects, guides, testimonials and site content, and an audit log.

## Deliberately out of scope

Online payments or a payment gateway, invoices and accounting, a customer portal, messages to customers, and quotation version history were not part of the delivered scope. Keeping these out was an explicit decision recorded per milestone.
