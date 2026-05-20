# YCS Onboarding Email Templates
## Technical Handoff Document — Version 3.0
**Prepared by:** Anis Rahaman, AVP Operations, Yanolja Cloud Solution
**Date:** May 2026
**For:** Tech / Product Team

---

## PURPOSE

This document hands off 6 redesigned transactional email templates in the YCS customer onboarding flow. Each template replaces an existing email currently served from your internal product server. Every section includes: trigger logic, business goal, required dynamic tokens, subject line, preheader, signature owner, and HTML file reference.

Read this fully before updating any template in the product.

---

## BRAND RULES — NON-NEGOTIABLE

These apply to every email, no exceptions:

| Rule | Detail |
|------|--------|
| **Product naming** | Never use: eZee Absolute, eZee Centrix, eZee BurrP!, eZee Mint, eZee Reservation. Use: Hotel PMS, Channel Manager, Booking Engine, Restaurant POS, Revenue Management, Payment Solution, Reputation Management |
| **Primary color** | `#F54B1E` (YCS orange) |
| **Secondary background** | `#F5EBE1` (warm beige) |
| **Email background** | `#EDE4D8` |
| **Typography** | Ledger (headings) + Archivo (body) — Google Fonts CDN |
| **Tone** | Warm, direct, human. Never corporate-sounding. |
| **Em dashes** | Never use `—`. Use commas, colons, or periods. |
| **Signatures** | Never add manually — assigned per email as documented below. System does not auto-apply signatures on transactional emails. |

---

## VARIABLE / TOKEN FORMAT

All dynamic tokens use **single curly braces**: `{Token Name}`

This matches your current product template syntax (confirmed from existing templates at 192.168.10.10). Do not convert to HubSpot double-brace format `{{Token}}` — these are product emails, not HubSpot marketing emails.

---

## EMAIL 2 — Online Data Gathering Form

**HTML File:** `v3-email-02-dgf-form.html`

### Trigger Point
Fired automatically **after client payment is confirmed** in the system.
The DGF link is embedded in this email dynamically at send time.

**Exception:** For Demo properties and Partner properties, this email fires immediately upon account creation — no payment required.

### Business Goal
Get the hotelier to open and submit the DGF within 48 hours. This is the single most important conversion in the onboarding sequence. Every day the form is not submitted is a day added to the hotel's go-live date. The email uses a progress indicator (Step 2 of 3 framing) and a single CTA to create forward momentum without pressure.

**Metric to track:** DGF submission rate within 48 hours of this email.

### Subject Line
```
{Hotel Name}, your account setup is waiting on one form.
```

### Preheader
```
5 minutes. That is all it takes to get {Hotel Name} live on One Intelligent Platform.
```

### Signature
**{Sales Person}** — the assigned sales consultant for this property.
Role shown: "Your Sales Consultant, Yanolja Cloud Solution"

### Dynamic Tokens

| Token | Description | Source |
|-------|-------------|--------|
| `{Contact Person}` | Primary contact first name | CRM |
| `{Hotel Name}` | Property name | CRM |
| `{DGF_LINK}` | Full URL to the property's online data gathering form | System generated |
| `{Sales Person}` | Name of the assigned sales consultant | CRM |
| `{UNSUBSCRIBE}` | Unsubscribe link | System generated |

### Design Notes
- Orange hero band — warm and inviting, momentum-building
- 3-step "what happens" section (You submit → We configure → You go live)
- Single black pill CTA button
- P.S. addresses the "I don't have all the info" objection

---

## EMAIL 3 — DGF Reminder

**HTML File:** `v3-email-03-dgf-reminder.html`

### Trigger Point
Sent when the onboarding/sales team **manually re-shares the DGF link** because no submission has been received.

This is not an automated time-based trigger — it requires a manual action by the sales person. If you want to automate this, the recommendation is: fire automatically 72 hours after Email 2 if no DGF submission event has occurred.

### Business Goal
Re-engage a hotelier who has gone cold after receiving Email 2. The email is intentionally darker in design (black header vs orange in Email 2) so it reads as a new, distinct message — not a duplicate. It kills the three most common blockers: not having all the information, not knowing the consequence of delay, and not knowing who to ask. It also gives the hotelier a graceful exit if their situation has changed.

**Metric to track:** DGF submission rate within 24 hours of this email.

### Subject Line
```
{Hotel Name} is not live yet. Here is what is paused.
```

### Preheader
```
Your configuration has not started. The clock moves the moment you submit.
```

### Signature
**{Sales Person}** — same sales consultant as Email 2.
Role shown: "Your Sales Consultant, Yanolja Cloud Solution"

### Dynamic Tokens

| Token | Description | Source |
|-------|-------------|--------|
| `{Contact Person}` | Primary contact first name | CRM |
| `{Hotel Name}` | Property name | CRM |
| `{DGF_LINK}` | Full URL to the DGF | System generated |
| `{Sales Person}` | Assigned sales consultant name | CRM |
| `{UNSUBSCRIBE}` | Unsubscribe link | System generated |

### Design Notes
- Dark (`#111111`) hero with animated status pill — visually distinct from Email 2
- Orange left-border "on hold" callout box
- Three fact rows: timeline consequence, progress-save, ask-for-help
- P.S. addresses broken link scenarios — takes responsibility on YCS's behalf

---

## EMAIL 4 — DGF Submission Acknowledgement

**HTML File:** `v3-email-04-submission-ack.html`

### Trigger Point
Fired **automatically the moment the customer submits the DGF**. This should be a system webhook — zero delay. The hotelier should receive this email within seconds of submission.

### Business Goal
Two jobs: (1) Eliminate submission anxiety — the hotelier needs instant confirmation that their data was received. (2) Prevent "what happens next" support tickets — the email sets exact expectations for the 2–3 business day configuration window. The receipt table at the bottom doubles as a legal confirmation of what was submitted and when.

**Metric to track:** Reduction in inbound "did you get my form?" support contacts.

### Subject Line
```
{Hotel Name} — we have everything. Configuration starts now.
```

### Preheader
```
DGF received. Here is exactly what happens in the next 2 to 3 business days.
```

### Signature
**The Onboarding Team** — this email is from the team, not an individual.
No individual name. Role shown: "Yanolja Cloud Solution"

### Dynamic Tokens

| Token | Description | Source |
|-------|-------------|--------|
| `{Contact Person}` | Primary contact name | CRM / DGF |
| `{Hotel Name}` | Property name | CRM / DGF |
| `{Property Address}` | Full address submitted in DGF | DGF submission data |
| `{Product Names}` | Comma-separated list of purchased and activated modules | DGF / CRM |
| `{Authorized Signatory}` | Name of the person who submitted/signed | DGF submission data |
| `{Submission Timestamp}` | UTC date and time of form submission | System auto-generated |
| `{IP Address}` | IP address from which DGF was submitted | System auto-generated |

### Design Notes
- Full orange hero with circular checkmark — celebratory moment
- 3-step vertical timeline (Config → Credentials → Training)
- Dark receipt table with black header, orange property name
- P.S. tells them their Account Manager will introduce themselves proactively

---

## EMAIL 5A — Hotel PMS Login Credentials

**HTML File:** `v3-email-05a-pms-login.html`

### Trigger Point
Fired automatically **after DGF submission when the Hotel PMS account creation completes** in the backend system. This is a system-generated event — no human action required. Timing depends on account creation speed (target: within 24 hours of DGF submission, ideally same business day).

**Send only to properties that have purchased the Hotel PMS module.**

### Business Goal
Deliver login credentials clearly and completely so the hotelier can access their platform on first attempt — no back-and-forth with support. Secondary goal: establish security hygiene from day one. The email separates credentials by access level (Admin, Front Desk, Mobile, Booking Engine) so there is no confusion about which link to use for which staff role.

**Metric to track:** % of properties that log in within 24 hours of receiving this email.

### Subject Line
```
{Hotel Name} — your Hotel PMS is live. Your login is inside.
```

### Preheader
```
Admin, Front Desk, and mobile access — all configured. Bookmark this email.
```

### Signature
**Team Hotel PMS** — product team sign-off.
Role shown: "Yanolja Cloud Solution"

### Dynamic Tokens

| Token | Description | Source |
|-------|-------------|--------|
| `{Contact Person}` | Primary contact name | CRM |
| `{Hotel Name}` | Property name | CRM |
| `{Hotel ID}` | Unique hotel/property code | System generated |
| `{Username}` | Login username (typically "admin" for initial setup) | System generated |
| `{Password}` | Auto-generated temporary password | System generated |
| `{ADMIN_URL}` | Full URL to the Admin Panel login page | System / product config |
| `{FRONTDESK_URL}` | Full URL to the Front Desk login page | System / product config |
| `{MOBILE_URL}` | Full URL or app deep-link for mobile login | System / product config |
| `{BOOKING_ENGINE_URL}` | Full URL to Booking Engine dashboard | System / product config |
| `{CHAT_URL}` | URL to live chat support | Static — support config |
| `{UNSUBSCRIBE}` | Unsubscribe link | System generated |

### Design Notes
- Dark (`#111111`) hero with subtle grid texture — serious, credential-delivery tone
- "Account Ready" orange pill badge + Hotel ID shown in header
- Each credential block is a separate panel with its own header (Admin / Front Desk / Mobile / Booking Engine)
- All credential values rendered in monospace font (`Courier New`) for clarity
- Security section on dark background — deliberately weighted to ensure it is read
- 3-column support bar: India / USA / Chat

### Critical Tech Note
Passwords and usernames **must render in monospace font** (`font-family: 'Courier New', Courier, monospace`). Test this in Outlook — Outlook does not honor web fonts and will fall back to the specified stack. Courier New is a safe monospace fallback that works in Outlook natively.

---

## EMAIL 5B — Restaurant POS Login Credentials

**HTML File:** `v3-email-05b-pos-login.html`

### Trigger Point
Same trigger as Email 5A — fires after DGF submission when the **Restaurant POS account creation completes**.

**Send only to properties that have purchased the Restaurant POS module.**

If a property purchased both Hotel PMS and Restaurant POS, send **both Email 5A and 5B separately** — do not combine. Each product has a distinct login, distinct team of users, and distinct use case.

### Business Goal
Same as Email 5A but scoped to F&B operations. The design uses an orange gradient hero (vs the dark hero in 5A) so a hotelier receiving both emails immediately knows which is which.

**Metric to track:** % of properties that log in to POS within 24 hours.

### Subject Line
```
{Hotel Name} — your Restaurant POS is live. Your login is inside.
```

### Preheader
```
Your F&B command centre is configured and ready. Bookmark this email.
```

### Signature
**Team Restaurant POS** — product team sign-off.
Role shown: "Yanolja Cloud Solution"

### Dynamic Tokens

| Token | Description | Source |
|-------|-------------|--------|
| `{Contact Person}` | Primary contact name | CRM |
| `{Hotel Name}` | Property name | CRM |
| `{Hotel ID}` | Unique hotel/property code | System generated |
| `{Username}` | Login username | System generated |
| `{Password}` | Auto-generated temporary password | System generated |
| `{ADMIN_URL}` | Full URL to POS Admin login page | System / product config |
| `{UNSUBSCRIBE}` | Unsubscribe link | System generated |

---

## EMAIL 6 — Recovery Email Verification

**HTML File:** `v3-email-06-recovery.html`

### Trigger Point
**THIS TRIGGER IS CURRENTLY UNCONFIRMED.** Per the original documentation, the exact trigger point is unknown. Based on system logic, this likely fires when a recovery email address is populated in the account record — either during account creation or when the hotelier adds one manually via Settings.

**ACTION REQUIRED FROM DEV TEAM:** Identify and document the exact system event that fires this email. Is it triggered by account creation? By the hotelier adding a recovery email in Settings? By the AM setting it up during configuration? This must be resolved before this template is deployed.

### Business Goal
Get the hotelier to confirm their recovery email address within 60 minutes of receiving it. This is a pure security/transactional email — no marketing, no upsell, no noise. The design is intentionally the most minimal in the entire sequence. The goal is trust: every element that does not serve confirmation is removed.

**Metric to track:** Recovery email confirmation rate within 60 minutes.

### Subject Line
```
Action required: confirm your YCS recovery email (link expires in 60 min)
```

### Preheader
```
One click to protect your account and your guests' data.
```

### Signature
**YCS Security Team** — not an individual, not a product team.
Role shown: "Yanolja Cloud Solution"
No P.S. on this email — security emails must have zero distraction.

### Dynamic Tokens

| Token | Description | Source |
|-------|-------------|--------|
| `{CONFIRM_URL}` | Unique one-time confirmation link | System generated |

### Critical Tech Notes
1. The confirmation link **must expire exactly 60 minutes** after the email is sent — not 60 minutes after the email is opened.
2. If the link is clicked after expiry, show a clear error page explaining the link has expired and provide a way to request a new one.
3. If the hotelier does not click within 60 minutes, the system should either re-send or notify them via another channel (SMS or in-app notification).
4. Do **not** include any login credentials or sensitive data in this email.
5. The max-width on this email is **520px** (narrower than the others at 620px) — intentional. Recovery/security emails should feel focused and contained.

---

## GLOBAL TECHNICAL SPECIFICATIONS

### Fonts
```html
<link href="https://fonts.googleapis.com/css2?family=Ledger&family=Archivo:ital,wght@0,400;0,500;0,600;0,700;1,400&display=swap" rel="stylesheet">
```
**Fallback stack:**
- Ledger → `Georgia, 'Times New Roman', serif`
- Archivo → `Helvetica Neue, Helvetica, Arial, sans-serif`

**Outlook note:** Outlook (Windows) does not support Google Fonts via `@import`. All headings will fall back to Georgia. All body text will fall back to Helvetica/Arial. This is acceptable — test and confirm with the team.

### Color Palette
| Name | Hex | Usage |
|------|-----|-------|
| YCS Orange | `#F54B1E` | CTAs, accents, hero backgrounds, highlights |
| YCS Beige | `#F5EBE1` | P.S. boxes, secondary surfaces |
| Email Background | `#EDE4D8` | Body wrapper background |
| Light Surface | `#F7F3EE` | Secondary cards, security boxes |
| Near Black | `#1A1A1A` | Body copy primary |
| Dark Hero | `#111111` | Dark email headers |
| Mid Gray | `#6B6560` | Secondary text, subheadings |
| Border | `#EEEAE4` | Dividers, card borders |

### Email Widths
- Emails 2, 3, 4, 5A, 5B: **620px** max-width
- Email 6 (Recovery): **520px** max-width — intentionally narrower

### Mobile
All templates are responsive down to 375px. Credential rows in 5A and 5B stack vertically on mobile via the flex column layout. Test on both iOS Mail and Gmail Android.

### Plain Text Fallback
Generate a plain text version for each email. Strip all HTML. Retain: greeting, core message, CTA URL as plain text, signature, support contacts.

### Token Validation
Before sending any email, validate that all required tokens are populated. If `{Hotel Name}` or `{Contact Person}` is empty, do not send — hold the email and alert the onboarding team. A blank token showing up in a customer email is a critical trust failure.

---

## OPEN ITEMS — MUST RESOLVE BEFORE DEPLOYMENT

| # | Issue | Owner | Priority |
|---|-------|--------|----------|
| 1 | **Confirm exact trigger event for Email 6** (recovery email verification). Is it account creation? Manual Settings action? AM-initiated? | Dev + Ops | CRITICAL |
| 2 | **Automate Email 3 trigger** — currently manual. Recommend: auto-fire 72 hours after Email 2 if no DGF submission event. Confirm with Ops. | Dev + Ops | High |
| 3 | **Token: `{DGF_LINK}`** — confirm this is dynamically generated per property at send time. Not a static URL. | Dev | High |
| 4 | **Token: `{BOOKING_ENGINE_URL}`** — confirm this URL is system-generated per property, not a generic link. | Dev | High |
| 5 | **Monospace font test in Outlook** — confirm `{Password}` and `{Username}` render in Courier New in Outlook 2016, 2019, and 365. | Dev / QA | High |
| 6 | **Google Fonts fallback test** — confirm heading fallback to Georgia renders acceptably in Outlook. | Dev / QA | Medium |
| 7 | **Email 6 expiry page** — build a clear error page for expired confirmation links. Include a "request new link" option. | Dev | High |
| 8 | **Token validation logic** — implement a check before every send: if any required token is null/empty, hold the email and alert the team. | Dev | High |
| 9 | **Separate send logic for 5A and 5B** — confirm the system can conditionally send 5A (PMS), 5B (POS), or both based on purchased modules from the DGF record. | Dev | High |
| 10 | **`{CHAT_URL}` in Email 5A** — confirm the correct live chat URL and whether it requires authentication or is open-access. | Ops | Medium |

---

## EMAIL SEQUENCE FLOW

```
PAYMENT CONFIRMED (or Account Created for Demo/Partner)
│
└──► EMAIL 2: Online Data Gathering Form
         │
         │ [No submission after 72 hours — auto-trigger recommended]
         │
         └──► EMAIL 3: DGF Reminder (currently manual trigger)
                   │
                   │ [DGF Submitted — webhook, zero delay]
                   │
                   └──► EMAIL 4: Submission Acknowledgement
                             │
                             │ [Account creation completes — system event]
                             │
                             ├──► EMAIL 5A: Hotel PMS Login  (if PMS purchased)
                             │
                             └──► EMAIL 5B: Restaurant POS Login  (if POS purchased)
                                       │
                                       │ [Recovery email registered — trigger TBC]
                                       │
                                       └──► EMAIL 6: Recovery Email Verification
```

---

*Document Version: 3.0*
*Prepared by: Anis Rahaman, AVP Operations*
*Contact: anisur@yanoljacloudsolution.com*
*© Yanolja Cloud Solution Pvt. Ltd. — Transforming Hospitality Worldwide*