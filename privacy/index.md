---
layout: default
title: Stockd Privacy Policy
permalink: /privacy/
---

# Stockd Privacy Policy

**Last updated:** 2026-05-16 *(Auth, diagnostics/Crashlytics, analytics, household, security, and rights sections are final. One narrow note remains in §1.4 — the exact subscription-data field set + RevenueCat client configuration — pending the Plan 4.0b purchase client; it updates in place with no URL change. Cross-checked against App Store Connect nutrition labels per `docs/privacy/data-inventory.md`.)*

**Effective:** Date of first app launch

Stockd is built and operated by **Kynera Studios LLC** ("Stockd," "we," "us," or "our"). This Privacy Policy explains what we collect, why, where it goes, and what you control.

If you have questions, email [info@kynerastudios.com](mailto:info@kynerastudios.com).

> **Plain-English summary** (also legally binding):
>
> - We collect what's needed to run a household pantry app: your account identifier, the food items you track, your grocery list, your meal plans, and (if you subscribe) your subscription status.
> - We do **not** sell your data, do **not** track you across other apps, do **not** use advertising, and do **not** access your photos library, contacts, or location.
> - We use Google Firebase to store your data, Google's Gemini and Imagen AI to generate recipe text and images, RevenueCat to manage subscriptions, and Open Food Facts to look up barcode product info.
> - You can delete your account at any time from **Settings → Delete account**. Deletion is a cascade — your data is removed from our systems. (Apple subscriptions, if any, must be cancelled separately in your Apple ID Settings.)

---

## 1. What we collect

### 1.1 Account identifier

When you sign in, we receive an account identifier from your chosen provider:

- **Sign in with Apple:** Apple gives us a stable identifier (the SIWA `sub` claim). If you choose **Hide My Email**, Apple gives us a private-relay email of the form `*.appleid.com` — we never see your real email, and email you send to that relay address is forwarded to you by Apple. We do not store the relay address beyond what Firebase Auth retains for your account, and we never use it for marketing. If you later revoke Stockd's access in **iOS Settings → Apple ID → Sign in with Apple**, the relay address stops forwarding and you'll be signed out (you can sign in again at any time). If you instead choose to share your real email, we receive and store it with your account.

- **Sign in with Google:** Google gives us your Google account email and Google ID, with your consent.

Firebase Auth assigns a stable internal user ID (UID) that we use to associate your data with your account. Your UID is visible in-app at **Settings → User ID**.

### 1.2 What you put into Stockd

- **Pantry items** you add: name, category, location, quantity, expiration date, optional notes, optional barcode.
- **Grocery list** entries you create.
- **Meal plans** (recipes scheduled on the calendar).
- **Household memberships** (who else has access to your pantry, if you share).
- **Optional display name** you set in Sign in with Apple or Google.

### 1.3 What gets generated for you

- **AI recipe suggestions:** when you ask Stockd to generate a recipe, your prompt text plus the names of pantry items you currently have are sent to Google's Gemini API. The prompt and pantry context never include your user ID or your account email — only the words you typed and item names from your pantry.
- **AI recipe images:** the recipe name is sent to Google's Imagen API to generate an image. The image is cached publicly under a hash of the recipe name so other Stockd users who generate the same recipe can reuse it. The image is not tied to your account.
- **Barcode product info:** when you scan a barcode, we send the barcode digits to Open Food Facts to look up the product name and category. Just the barcode — no identifier.
- **OCR (expiration-date scanning):** done entirely on your device using Apple's Vision framework. Camera frames never leave your device.

### 1.4 Subscription info

If you subscribe to Stockd Pro, Apple charges your Apple ID and notifies RevenueCat (our subscription middleware), which sends us a webhook event. We store the subscription product ID, transaction ID, entitlement status, and expiration timestamp — no payment details. We never see your credit card.

*(This section is final for the data we store today — product ID, transaction ID, entitlement status, expiration timestamp, no payment details. One narrow item locks when the purchase client ships in **Plan 4.0b**: the exact field set written per transaction and written confirmation that RevenueCat's optional customer-attribution and audience features stay disabled for privacy minimalism. This section updates in place at that point — same URL, no payment data involved either way.)*

### 1.5 Diagnostics

- **Server logs:** Firebase Functions logs include request timestamps, error messages, and (for authenticated calls) your UID. Request bodies are not logged. Retained 30 days.
- **Crash reports:** Stockd uses Firebase Crashlytics to record crashes and serious errors. When a crash happens, Crashlytics sends the crash stack trace, your device model, your OS version, and your app version to Google's Crashlytics servers. We attach two custom labels: your Firebase user ID and (if applicable) the error code from the in-app diagnostic display, so we can correlate the crash to your account when you write to support. We do NOT attach pantry items, grocery list contents, meal plans, email addresses, names, locations, or payment data to crash reports. Retained per Firebase Crashlytics' defaults (~90 days for analytics).
- **Account-deletion audit:** server-only record of when an account was deleted. Retained indefinitely for forensic / refund purposes.

### 1.6 Analytics

Stockd v1.0 collects **two specific events** to validate critical design assumptions:

- `aha_fired` — once per account, when you first generate a recipe using ingredients you already have.
- `meal_plan_grocery_prompt_skipped` — when you tap Skip on the "Add missing ingredients to your grocery list" prompt.

Both events are sent to Firebase Analytics and are linked to your UID. We do **not** use this data for advertising or sell it. The full event taxonomy stays minimal in v1.0; future versions may add events, and this section will be updated accordingly.

### 1.7 What we do NOT collect

We do not collect: your location, your photos library, your contacts, health data, financial information (Apple handles payment), browsing history, search history across sessions, or sensitive personal data as defined by Apple. We do not use the AppTrackingTransparency framework — Stockd does not track you across other apps or websites.

---

## 2. Why we collect it (purposes)

- **Provide the service:** we cannot show your pantry, generate recipes, or process subscriptions without storing the underlying data.
- **Maintain account security and integrity:** Firebase Auth identifiers prevent unauthorized access; audit logs help us detect and respond to issues.
- **Improve the product:** two specific events (above) let us validate whether design assumptions hold; we use this to make better decisions in future versions.
- **Support you:** when you write to `info@kynerastudios.com`, we look up your account by the User ID you provide.

We do not use any of this data for advertising, profiling, or sale.

---

## 3. Where it goes (third-party processors)

| Provider | Purpose | What they see |
|---|---|---|
| Google Firebase (Auth, Firestore, Cloud Functions, Storage, Analytics) | Storage, auth, server compute, analytics | Everything in §1 except payment details |
| Google Gemini API | AI text generation | Your prompt + pantry item names (no UID, no email) |
| Google Imagen API | AI image generation | Recipe name strings only |
| Apple StoreKit | Payment processing | Your Apple ID payment info (we never see card data) |
| RevenueCat | Subscription middleware | Apple StoreKit transaction events; our UID-to-subscription mapping |
| Open Food Facts | Barcode lookup | Barcode digits only |
| Apple SIWA relay | Private email forwarding (only if you chose Hide My Email) | Your real email (we never see it) |

We have agreements with these providers consistent with our handling of your data. We do not sell or share your data outside this list.

---

## 4. Sharing within Stockd (household members)

If you invite other people to share a pantry, those people can view and modify the shared pantry's items, grocery list, and meal plan entries. Their visibility is to the shared pantry, not to your other accounts or unrelated data.

Removing a household member revokes their access immediately for future reads.

---

## 5. Your rights and choices

- **Access:** see your data live in the app at any time.
- **Correction:** edit or delete items, list entries, and meal plan entries directly in the app.
- **Deletion:** **Settings → Delete account** removes your data via a cascade. Note that this does *not* cancel an active Apple subscription — see §1.4 of our Terms.
- **Export:** if you need a copy of your data, email [info@kynerastudios.com](mailto:info@kynerastudios.com) and we'll generate one within a reasonable period.
- **Subscription:** managed by Apple in **Settings → [your name] → Subscriptions**.
- **Sign in with Apple Hide My Email:** controlled in **Settings → Apple ID → Sign in with Apple**. If you revoke Stockd's access, you'll need to sign in again.

### 5.1 EU / UK residents (GDPR)

If you are a resident of the European Union or the United Kingdom, you have additional rights under GDPR / UK GDPR: access, rectification, erasure, restriction of processing, data portability, and objection. To exercise these rights, email [info@kynerastudios.com](mailto:info@kynerastudios.com). We respond within 30 days.

The legal basis for our processing is performance of a contract (running the service you signed up for) under Article 6(1)(b) GDPR. For diagnostic and analytics purposes, our legal basis is legitimate interest (Article 6(1)(f) GDPR) — improving the service we provide you.

### 5.2 California residents (CCPA / CPRA)

If you are a California resident, you have the right to know what personal information we collect, to delete it, to correct it, and to opt out of any "sale" or "sharing" — Stockd does not sell or share your data, so the opt-out is automatic and there is no link to provide. Contact [info@kynerastudios.com](mailto:info@kynerastudios.com) to exercise other rights.

---

## 6. Children

Stockd is not directed to children under 13. We do not knowingly collect personal information from children under 13. If you believe a child has provided us with personal information, please contact [info@kynerastudios.com](mailto:info@kynerastudios.com) and we will delete it.

---

## 7. Security

We use industry-standard security practices: encrypted connections (HTTPS / TLS), Firebase security rules enforcing per-user access controls, no plaintext password storage (we use OAuth — there are no passwords for us to store), webhook signature verification for subscription events, and an audit log for account deletions and entitlement changes.

No system is perfectly secure. If we discover a breach affecting your data, we will notify you in accordance with applicable law.

---

## 8. Changes to this policy

We may update this Privacy Policy. Material changes will be announced in-app or by email before they take effect. The "Last updated" date at the top reflects the most recent revision.

---

## 9. Contact

Privacy questions, data requests, or concerns: [info@kynerastudios.com](mailto:info@kynerastudios.com)

Stockd is operated by **Kynera Studios LLC**.

---

*Kynera Studios LLC · © 2026*

---

**Document status note (for the spec / pass-two reviewers):**
This Privacy Policy was a DRAFT through CW pass-two. Status as of 2026-05-16: (a) OD-b4 SIWA relay-address handling — **FINALIZED** in §1.1 (relay non-storage + marketing exclusion + revocation-stops-forwarding now stated explicitly, per `data-inventory.md` pending-section A); (b) Plan 4.x RevenueCat data set — **NARROWLY STILL PENDING Plan 4.0b** (the data we store today is described and final, but the exact per-transaction field set + the customer-attribution/audience-off confirmation lock with the 4.0b purchase client — `data-inventory.md` pending-section B remains open by design; the earlier round-11 note that called this "resolved" was optimistic and is corrected here); (c) Crashlytics — **enabled** per OD-b5, finalized text at §1.5. Factual basis at `docs/privacy/data-inventory.md`. The page is publicly live at `https://kynera-studios.github.io/stockd-pages/privacy/`; the §1.4 update lands in place when 4.0b ships (no URL change). **Standard caveat: this is a legal document; neither Cece nor Sarah is a lawyer; a second set of eyes is strongly recommended before submission for a product taking payments — see the v1-launch-checklist §6 "second-set-of-eyes legal review" row.**
