# Orion Release 1.0.3.29

**Release date:** November 21, 2026

## Overview

This release captures the work completed across the Orion ecosystem after the previous 1.0.3.9 changelog (September 26, 2026), up to November 21, 2026. It spans every repository: Orion Intelligence (backend and web client), Orion Dark Nexus, Orion Mail, Orion Micros, Orion Social, Orion Crawler (crawler, collector console and Orion Social extension), Orion Storage, Orion Tor2Web, Orion Sandbox, the new Orion Control panel, and the landing and documentation sites.

**How to read this document**

- Each product has its own part under a banner. New and changed capabilities come first, followed by a "Removed (for reference)" list.
- Bug fixes, security fixes and hardening are in the last part (SECURITY FIXES, HARDENING & IMPROVEMENTS), which ends with the open items: what is still left to do.
- Items that exist only in a working tree on November 21 are tagged [uncommitted]. Everything else is committed.
- 1.0.3.29 is the database version set by the newest migration.

## Contents

1. [Orion Intelligence (backend + web client)](#1-orion-intelligence-backend--web-client)
2. [Orion Dark Nexus](#2-orion-dark-nexus)
3. [Orion Mail](#3-orion-mail)
4. [Orion Micros](#4-orion-micros)
5. [Orion Social](#5-orion-social)
6. [Orion Storage](#6-orion-storage)
7. [Orion Tor2Web](#7-orion-tor2web)
8. [Orion Sandbox](#8-orion-sandbox)
9. [Orion Control (new)](#9-orion-control-new)
10. [Landing & documentation sites](#10-landing--documentation-sites)
11. [Orion Crawler (crawler, collector console & Orion Social extension)](#11-orion-crawler-crawler-collector-console--orion-social-extension)
12. [New collector & extension scripts since 1.0.3.9](#12-new-collector--extension-scripts-since-1039)
13. [Security fixes, hardening & improvements (audit items, fixes after the audit, open items)](#13-security-fixes-hardening--improvements-audit-items-fixes-after-the-audit-open-items)

## Added since the 1.0.3.28 notes (November 19-21)

- Tenant subscription billing through Stripe, with per-tenant Stripe keys, pricing, discounts, a Billing page and migration 1.0.3.29 (new section "Billing & Subscriptions").
- **LinkedIn as a full social automation platform:** session capture and verification, posting, ad monitoring, hate-speech monitoring and impersonation takedown, rewritten for LinkedIn's new page design and tested on a live account on November 21.
- APK security assessment in Micros (52 native rules merged with MobSF) and its Security Assessment view in the web client.
- **Theme palettes now recolour the whole web client:** 3,745 hardcoded colours in 213 client files follow the tenant palette.
- Identity Resolution keyword search for candidate profiles, and Orion Vault autofill on any website.
- Orion Social extension 0.6.12 to 0.6.21, and six social collector platforms restored (Instagram, TikTok, LinkedIn, Quora, Imgur, Douban) with 20 new leak-site scripts.
- Corrections to earlier counts and platform lists, 15 stale [uncommitted] tags removed, and three new closing sections: fixes made after the audit, audit entries whose behaviour changed, and open items.

## Featured

- **Kill Chain:** a new agentic offensive-security engine in Dark Nexus with a full design/run/report workspace in the AI client, 22 prebuilt scenarios, an autonomous hypothesis engine, and a conversational controller.
- Penetration-testing arsenal rebuilt from \~50 hand-coded modules into 286 declarative YAML specs (Active Directory/Kerberos, NetExec, brute-force, web exploit-proofs, IoT/OT, cloud and identity-federation attacks).
- **Marketplace intelligence:** a new underground-marketplace data source, index, search, consolidated results and heatmap category, fed by a new 412-script market collector family.
- **Tenant subscription billing:** subscription-managed tenants buy user seats and licence modules by card through Stripe, monthly or yearly, with immediate prorated upgrades, end-of-period downgrades, cancellation and receipts in a new Billing page; administrators and primary tenants set their own Stripe keys, prices and per-tenant discounts.
- **Identity Resolution:** two-profile impersonation analysis across 14 social platforms with scoring, an evidence matrix, keyword search for candidate profiles and automated takedown on 10 platforms.
- **Social automation on 15 platforms:** LinkedIn joins with session capture and verification, posting, ad monitoring, hate-speech monitoring and takedown.
- **Orion Vault:** a personal, browser-encrypted credential vault with recovery key, generator and extension autofill for case communications and, when switched on, any website.
- **Orion Mail:** self-destructing and password-protected mail, schedule/undo send, rich-text compose, an encrypted Storage drive, vacation responder, WKD key publishing, and full takedown-mail integration with Orion Intelligence.
- Authenticated vulnerability scanning in Micros with CVSS scoring, highlighted request/response evidence, custom Orion ZAP rules, TLS checks and a findings explorer with PDF export.
- **APK security assessment:** Android packages get a scored report from 52 native rules merged with MobSF results, with CWE, MASVS and OWASP Mobile references per finding.
- **Tenant theme palettes:** 20 colour palettes that now recolour every screen of the web client in dark and light mode.
- New standalone Orion Control panel for operating k3s crawler servers (live logs, pod health, env editor, terminal, build/deploy, stop/start).
- **Orion Storage:** a new encrypted, malware-scanned file service backing Mail, Kill Chain and Case Management.
- **Collector and extension expansion:** 643 new collector scripts (412 market, 105 social/forum, 80 news/tracking, 46 leak) and 1,017 new per-site extension resource modules, with HTML evidence snapshots for leak and forum records; the Orion Social extension moves from 0.6.5 to 0.6.21.
- **New and rebuilt public sites:** the Orion-Intelligence-Solution public repo and documentation, the DeepIntel.si landing site, and rebuilt Genesis and Orion Intelligence landing sites with full policy pages.

---

## 1. Orion Intelligence (backend + web client)

### Search, Data Sources & Visualisation

- **Marketplace module:** a new index of individual underground-market listings (market, vendor, category, price, currency, status, first/last seen, screenshots, links); crawlers submit via POST /api/index/market, Data Breach-licensed users search via POST /api/search/marketplace and open via GET /api/search/marketplace/{doc_id}. A Marketplace sidebar section with Leak-style filters and full report pages, plus marketplace results in consolidated search, result insights, the unified-search Platform filter and the Threat Lens globe.
- **Page snapshots:** crawlers upload a cleaned HTML copy of the source page (POST /api/snapshot, up to 2 MB); breach and forum-thread records show it as a sandboxed, non-clickable snapshot scrolled to the record, in place of the static screenshot; chat reports gain a collapsible Thread Screenshot card.
- **Search autocomplete:** GET /api/search/autocomplete returns up to 20 (default 8) matching record titles from licensed modules only; typing two or more characters in a category box shows completions under the box and in a new Suggestions list in the filter panel.
- **"Did you mean" suggestions:** GET /api/search/suggest returns a spelling correction when a query word is rare across 8 indices; results show a one-click re-run.
- **Exclusions and broader matching:** ranked searches accept -term and -"phrase" exclusions, match English word forms via stemmed subfields, tolerate typos in alphabetic words, treat emails/domains/hyphenated tokens as phrases with a domain wildcard, and apply every filter and exclusion to semantic (vector) search.
- **Diversified ranking:** Strategic, Breach, Marketplace and Social results are re-ranked over up to 1,000 results so repeated hosts, channels, URLs, titles or contents no longer fill a page; every ranked search gets a seeded tie-break; "sort by latest" alternates sources within each day; a Breach subcategory opened with no query lists newest first.
- **Absolute dates for relative times:** phrases such as "3 hours ago" or "yesterday at 10:00" in breach, marketplace, social and Strategic results and reports are rewritten as dates computed from the record's capture time.
- **Safe search by default:** the search API applies safe search unless a request turns it off, and on Strategic (General Intelligence) data it also excludes child-abuse material via explicit-phrase and minor/sexual-term proximity rules.
- **Exploit identifier search:** CVE, CWE and ZDI identifiers written with other separators are normalised (e.g. "cve 2024 1234" to CVE-2024-1234); bare CVE/CWE numbers are accepted in filters; product and tag filters match whole words; a company (vendor) filter is added; the Tools tab shows tool records only; exploit report content now renders as Markdown.
- **Enterprise-only credential visibility:** non-Enterprise users no longer see breach dump links, see archive passwords masked, and see stealer-log passwords, tokens, cookies and card data replaced with "Not authorized"; Enterprise users still see raw values. The stealer-log password-strength filter applies only to Enterprise users.
- **Public exposure check:** GET /api/search/stealerlogs returns matching exposure records with passwords, tokens, cookies, card and identity data removed, and the per-IP limit rises from 5 to 35 searches a day.
- **IOC extraction from a URL:** POST /api/ioc/extract/url extracts indicators from a public page via Micros and flags the page as a scam using the Dark Nexus scam classifier.
- **Defacement paging:** defacement search returns newest-first pages of 200 records (up to 50 pages) instead of one 1,000-record list; the category is renamed Compromise Monitoring across filters, alert and case types, CTI Graph and exports, and each record is shown as its own card (threat type, date, actor, key facts) that opens the report directly, with a Threat Types count in the summary.
- **Crypto watchlist status:** crypto address lookups show a Watchlist status naming the sanctions lists the address appears on, in place of the old risk level.
- **World heatmap:** the homepage heatmap now cycles 11 categories (Strategic, Leaks, News, Tracking, Exploits, Actors, Malware, Compromised, Compromise Monitoring, Social, Chat), shows the date range each covers, reports when there is no data in the window, and uses the tenant's default range; the Threat Lens globe loads its ArcGIS 4.34 library from the Orion server instead of the ArcGIS CDN.

### Homepage Insights

- **Snapshot-based trends:** insight trends are recalculated hourly against snapshots kept for 8 days; daily and weekly changes are signed percentages against the newest snapshot at least 24 hours or 7 days old (rebuilt from record creation dates when no snapshot exists) and shown only for count metrics; Leak metrics exclude news and tracking; Languages, Organizations and Countries Tagged counts are added.
- **Heatmap window:** the country heatmap is limited to a window set by the tenant's heatmap setting or COUNTRY_INSIGHT_DAYS (default 30); the tenant setting is limited to 7, 15, 30 or 180 days and is the only source of the window.

### AI Workspace (Nexus) & Kill Chain

- **Kill Chain view:** a new AI Workspace view (from the switcher or a reload-safe link) to design, run and review an attack chain saved with its own Nexus chat; the engagement target is one domain, IPv4, URL or CIDR, which becomes the root node, and invalid scopes are rejected with a reason.
- **Chain canvas:** steps appear on a drag/pan/zoom graph coloured by phase (Recon, Scanning & Enumeration, Access & Vuln ID, Exploitation, Actions on Objectives, Reporting) with live status (running, done with run count, failed, blocked out of scope, skipped).
- **Step types and inspector:** a chain mixes pentest-tool, data-API and "AI chooses" steps with Router (if/else, first match), Merge (union/dedupe/append; wait all or any), Transform and Report steps; a side inspector edits title, prompt, primary input, source and wiring and shows output and errors, with Re-run and Delete.
- **Run, stop, resume:** "Run chain" runs only new or reset steps server-side with live progress and auto-adds a Report step; users can stop, re-run one step or re-arm all; a run continues across view switches and reloads, with stalled runs restarted automatically.
- **Scenario library:** 22 prebuilt pentest chains load in one click (e.g. Passive Subdomain Discovery, TLS/SSL Configuration Audit, WordPress Assessment, SQL Injection and XSS Assessment, Credential & Breach Exposure, Full External Pentest).
- **Deep Intel assistant:** a chat panel over the canvas turns a written objective into a target and hypothesis paths and takes instructions ("form more hypotheses", "run", "stop", "re-arm", "write the report"), confirming before it changes the chain.
- **AI hypotheses:** a panel and dashed canvas areas show each hypothesis with goal type, confidence, rationale, planned tools, status (open, testing, confirmed, refuted, inconclusive) and verdict; users can try, extend, dismiss or delete each, and the chain re-plans after every run until the objective is met.
- **Chain reports and exports:** the Report step compiles a downloadable PDF (objective achieved, or attack surface found, untested surface and next steps); an Exports layout lists the chain's PDFs for preview, download and deletion; whole chains export and import as .kc files; the canvas can be clustered by hypothesis and reset after confirmation.
- **Storage and ownership:** each AI chat holds one kill-chain container in Dark Nexus (target, goal, completion, nodes, edges, hypotheses, panel preferences) managed via /api/kill-chain/containers (create, current, by chat, read, save, delete); chat requests naming a container are ownership-checked; reports are listed, downloaded and deleted per chat via /api/nexus/exports/kill-chain/{chat_id}; a kill_chain chat tool runs chain changes only after the user confirms; every change is auto-saved within about a second and restored for the active chat.
- **Unified AI search:** POST /api/ranked/ai returns one merged, de-duplicated list across up to 14 sources (breach, news, Strategic, exploits, APT, malware, chats, social, defacement, stealer logs up to 1,000 rows, the wanted list, plus crypto, phone and DKIM lookups when named), with match groups, exclusions and a date range, limited to licensed sources (listed by GET /api/ranked/ai/permitted), and used by the Dark Nexus assistant.
- **Nexus chat UX:** session-export PDFs appear in the Directory and Split views with preview and download; replies can include clarification/follow-up chips and section links that open the matching Orion page; replies render records as cards (title, source link, field grid, notes) with entity chips, formatted dates, "shown / total" tables and click-to-load image thumbnails; chats appear in the Notifications panel with live step and progress, ending as "Reply ready", "Stopped" or "Reply failed"; shared chat pages save as PDF and a "Revoke Shared Links" item cancels all shared chat links (DELETE /api/profile/chat-shares); a progress timer and a "Reconnecting to Nexus…" state are shown; on screens under 900 px the sidebar collapses and the workspace stacks; AI chat messages can be up to 20,000 characters (up from 500).

### Social Intelligence & Automation

- **Identity Resolution:** POST /api/social/identity-resolution/investigate (and the Identity Resolution page under Social Intel, linked from Social Mapper) compares a reference profile with a suspected impersonator on the same or a different platform across 14 platforms (Instagram, Facebook, X, Threads, TikTok, YouTube, Reddit, Pinterest, Quora, Patreon, MeWe, OK.ru, Hashnode, Behance). URLs are checked as typed, the handle is detected, each field shows a tick or cross with a tooltip, and matching accounts warn. It returns impersonation-risk and same-person scores, data coverage and confidence, key findings, side-by-side profiles (counts, bio, links, resolved link-in-bio targets, contacts), threat scoring (type, level, topic, scam categories, lure terms, severity, takedown eligibility, concerning vs protective signals), and a structured evidence matrix. A Dark Nexus scam flag on the candidate's text raises its threat score and recommends takedown at 65 or above.
- **Social impersonation takedown:** POST /api/manage-profiles/trigger-takedown files impersonation reports through each platform's official form by browser automation on 10 platforms (Facebook, Instagram, Threads, TikTok, YouTube, Reddit, Quora, Pinterest, Patreon, LinkedIn), using a captured session (required for Reddit, YouTube and LinkedIn) and an ID photo up to 10 MB, returning the platform ticket ID.
- **Add a Monitor:** a single "Add a Monitor" drawer replaces the separate account, persona and monitoring-profile forms; users pick Persona or External Account, an activity (Posting, Ad Monitoring, Hate Speech Monitoring, Keywords Monitoring), platform and session, a profile URL checked against the platform's rules, a label and persona details.
- **Keyword monitoring:** a new activity scans a feed for posts whose text or author contains any of the monitor's keywords, records which matched, can run on demand, shows matched-keyword chips with a toggle for the other posts, and combines with hate-speech labelling when both are selected.
- **Own-account and target monitors:** monitors can watch the persona's own account or an external profile URL (checked against the platform's domains); a monitor runs in the daily schedule only when its toggle is on (shown as "Cron: On"), needs a persona only for posting, and can share one captured session with monitors doing different activities (with an "already in use" warning).
- **Classification via Dark Nexus:** on storing automation results, each detected ad gets a topic, landing-page IOCs and a Scam/Not Scam verdict, shown with likes, shares and views and IOCs grouped by type with copy buttons; each post gets a hate-speech label.
- **Automation platform list:** Manage Profiles is limited to the 15 platforms that support automation (Facebook, Instagram, X, Threads, Reddit, Pinterest, YouTube, TikTok, Quora, Patreon, LinkedIn, OK.ru, Hashnode, Behance, MeWe).
- **Keyword-filtered profile crawls:** live Social Intelligence profile crawls through the extension accept a keyword, so a profile section can be fetched filtered by that keyword (results cached per keyword); Profile Intelligence lets users add keyword chips to filter a profile's posts and content tabs (not connections, followers or friends), re-running the live fetch limited to those keywords.
- **Identity Resolution keyword search:** a Keywords mode takes up to 10 keywords and asks the extension to run up to two Google queries limited to the reference profile's platform; candidate profile links open in the record sidebar, and "Compare with reference" runs the full two-profile analysis. The extension must acknowledge within 6 seconds (otherwise "extension needs updating") and the search times out after 240 seconds.
- **LinkedIn in Manage Profiles:** LinkedIn sessions can be fetched, uploaded, edited and verified like the other platforms (verification signs in with the stored session and reads the account handle), and LinkedIn profiles can be used for posting, ad monitoring, hate-speech monitoring and keyword monitoring.
- **Browser-targeted sessions:** Fetch, Edit and Verify send the requesting browser's extension instance (POST /api/manage-profiles/session and /session/verify accept instance), so the capture window opens only in the browser the user clicked in when the extension is installed in more than one.
- **Feeder target inventory:** social_threat_intel_targets.md lists the 9,185 production Feeder links on 30 social platforms (6,724 threat-intelligence accounts, pages, channels and groups added on November 21 and 2,461 already present, 1,585 of them search, tag or topic pages).
- **Social URL rules:** url_rules.json adds Douban and Imgur rules, eight more blog hosts under Blogger and old.reddit.com/user/&lt;name&gt; under Reddit.

### Takedown

- **Social Takedown tab:** the takedown dialog adds a Social Takedown tab for impersonating accounts, opening on it with platform and URL filled in when the target is on a supported platform; users choose from a searchable list of 260 platforms (All, Automated, Manual).
- **Automated impersonation reports:** for 10 platforms (Facebook, Instagram, Threads, TikTok, YouTube, Reddit, Quora, Pinterest, Patreon, LinkedIn) users fill a short form and Orion files the platform's own report automatically, showing step-by-step progress and the ticket number; the impostor URL is checked, reporter details come from the tenant and its Orion Mail mailbox, a photo ID is required where asked, and Reddit, YouTube and LinkedIn are filed from a captured session. For the other 250 platforms the dialog shows the official report-form link, short instructions and (for 125) an abuse email with a copy button.
- **Takedown mail:** accepting a request emails the abuse contact from the tenant's Orion Mail report mailbox (subject "Urgent: Takedown Request for &lt;domain&gt; [TD-&lt;id&gt;]", HTML and plain-text bodies, analyst note, screenshot and HTML evidence), requiring an activated tenant mail account whose status is reported by GET /api/takedowns/mail-account-status. A Takedown Mail page lists abuse-contact reply threads, shows an unread count, marks replies read, and replies with up to 10 attachments (25 MB total) via /api/takedowns/thread, /unread-mails-count, /mark-read and /reply; accepting is disabled with a "Mailbox Needs Activation" notice until the mail account is active.
- **Background evidence capture and retry:** new requests start as "Capturing evidence" and finish in the background even if the dialog is closed; captures that fail or time out after 10 minutes are kept as "Takedown failed" with the reason and can be retried (POST /api/takedowns/{id}/retry); a View dialog opens a request's target, status, abuse contact, channel, requester, timestamps, failure/rejection reason, analyst note and captured screenshot (GET /api/takedowns/{id}/report).

### Orion Vault

- **Orion Vault page:** a personal credential vault for admins, members and analysts (not demo) of up to 500 entries (name, URL, username, password) in a searchable list with edit and delete, served by /api/vault/\* and /api/e2e/\*.
- **Setup, unlock, lock:** a vault passphrase of at least 8 characters with mixed case, a number and a symbol (with a strength meter); a one-time 24-character recovery key shown once; the vault stays unlocked only for the current browser tab and can be locked at any time; 5 failed unlocks block it for 15 minutes.
- **Browser-side encryption:** each password is encrypted in the browser with AES-256-GCM before upload, under a vault key protected by keys derived from the passphrase or recovery key (PBKDF2-SHA-256, 600,000 iterations, ECDH P-256 key wrapping), so the server stores only encrypted passwords.
- **Generator and confirmation:** "Suggest strong password" fills a random 18-character password; showing, changing or deleting a stored password first requires a single-use, 5-minute Orion login-password confirmation, and a changed password is re-encrypted in the browser.
- **Vault security settings:** change the passphrase, regenerate the recovery key (invalidating the old one), recover a forgotten passphrase with the recovery key without losing entries, and delete the whole vault.
- **Case-communication autofill:** when a case communication page opened through the Orion extension asks for a login, the open Orion tab fills the matching vault login (or offers a choice), can generate and save a new login for the site, opens the vault for unlocking if locked, and encrypts each reply it sends to the extension.
- **Autofill on any website:** with the extension's Passwords switch on (off by default), the open Orion tab answers autofill requests from ordinary websites only when the request's user and tenant match the signed-in session, matches logins by exact origin, fills only after a click, and can create and save a new login from a sign-up form. Autofill never runs on Orion's own pages.

### Case Management

- **Multiple analysts per case:** several analysts can be assigned at once; removing an analyst clears their task assignments; responses include display names for creators, assignees and closers.
- **Analyst contributions:** analysts on an open case can add tasks, artifacts and related entities and edit or delete the ones they created (previously only task status).
- **Communication evidence:** PNG screenshots and WebM screen recordings taken in the extension while a case contact channel is open are saved to that channel automatically; a Captures viewer previews, downloads and deletes them after an integrity check (/communications/{communication_id}/captures).
- **Resume communication sessions:** opening a case communication returns to the saved page and scroll position on the same site and can target one specific browser when the extension is installed in more than one.
- **Orion Storage for case files:** case artifact files and captures are stored encrypted, per tenant, in Orion Storage (STORAGE_BASE_URL, STORAGE_TOKEN, STORAGE_CERT_PATH) with a 20 MB per-file limit (down from 100 MB); backend/copy_case_artifacts_to_storage.py migrates existing local files.
- **Detail refinements:** case sections collapse and carry short descriptions; related entities can be removed; adding a contact channel needs a connected extension; case PDFs carry the tenant logo.

### Accounts & Personal Security

- **2FA enrolment in account settings:** two-factor is turned on in account settings (confirm current password, scan QR or secret, enter a valid code), with each code single-use and 5 attempts per 15 minutes; enrolment no longer starts at login.
- **Session lifetime and sign-out:** web sessions end 12 hours after sign-in regardless of activity, and changing or resetting a password signs the user out of all web and extension sessions.
- **Self-service account deletion:** members and analysts without the maintainer licence can permanently delete their own account after entering their password (POST /api/account/delete), which also removes their Orion Mail mailbox, stored mail and keys.
- **Audit events:** the audit log also records failed logins, lockouts, logins completed with 2FA, recovery-key creation, self-service account deletion and tenant deletion (with the number of sub-tenants removed), and returns a total count for paging.
- **Orion Mail unread badge:** the Orion Mail menu item shows the unread count up to 99+.

### Tenants, Domains & Branding

- **Custom domains:** primary tenants can serve the workspace on their own hostname once POST /api/tenant/custom-domain/verify confirms an A or CNAME record to CUSTOM_DOMAIN_TARGET, a wildcard record for sub-tenants, and an \_orion-verify TXT record; sub-tenants become reachable at &lt;slug&gt;.&lt;custom domain&gt;, with TLS issued on demand; the UI shows the DNS records and a Verified / Unverified / Verification failed status.
- **Custom webmail domain:** tenants with a verified custom domain can move mailboxes to mail.&lt;root domain&gt; (sub-tenants on &lt;slug&gt;mail.&lt;root domain&gt;); the MX, SPF and webmail records are listed and sub-tenants see the primary's choice as read-only.
- **Tenant onion addresses:** the root administrator and primary-tenant maintainers can assign a tenant a Tor v3 onion address in one click (POST /api/tenant/onion/assign), sub-tenants reachable at &lt;slug&gt;.&lt;onion address&gt;, keys stored encrypted, republished every minute by a new Tor service.
- **Theme palette:** tenants can apply one of 20 colour palettes to every user in both dark and light mode via the theme_palette system setting; tenants without one use the root tenant's.
- **Palette applied everywhere:** 3,745 hardcoded colours in 213 client files now read palette-driven variables (392 tone variables shifted by the palette's hue, saturation and depth, 12 glass tokens for the Threat Lens and Satellite panels, seven brand-blue tokens, and Tailwind's slate scale), so every screen follows the tenant palette; the default Orion palette is unchanged. Platform brand colours, severity and status colours, and chart and map data colours keep their own colours.
- **Sub-tenant IOCs:** primary maintainers whose tenant has privileged IOC access can add IOCs to their sub-tenants from Tenant management.
- **World heatmap range:** tenant settings set the homepage heatmap's default window for all users (system default or last 7, 15, 30 or 180 days).
- **Data Collection Overview:** the Statistics page (now in the admin profile menu) is rebuilt as a plain-language overview with donut charts (source health, data freshness, collection methods, monitored link status, each area's share), expandable source groups (each source Active or Paused, a "Needs attention" flag, last-updated time, watched links) and record counts per area; the statistics endpoint reports feeder freshness, source types, link status and page coverage and is limited to maintainers and administrators.

### Billing & Subscriptions (new)

- **Subscription-managed tenants:** Stripe subscriptions apply to tenants flagged subscription_required; only the tenant maintainer can manage billing, and platform administrators, default-tenant and primary-tenant users are refused (403). A sub-tenant that signs up under a primary tenant is created subscription-managed when that primary or the platform has Stripe keys configured.
- **Plans and prices:** a plan is 1 to 50 user seats, at least one licence module and a monthly or yearly interval; default monthly prices in USD are seat 10, OSINT Basic 49, OSINT Advanced 149, Social Mapper 79, Pentester 99 and Enterprise 499, with yearly at 12 times monthly. Maintainer, feeder and free licences cannot be bought.
- **Checkout:** the subscription is created incomplete and paid by card in an embedded Stripe Payment Element; it activates on payment confirmation or the invoice.paid webhook, and a second subscription while one is active is refused.
- **Upgrades and downgrades:** more seats, month to year, a higher OSINT tier, Enterprise or added modules apply immediately with a prorated invoice; downgrades are scheduled for the end of the current period with no proration and can be cancelled before then; a mixed upgrade and downgrade is refused.
- **Cancel and resume:** cancelling ends the subscription at the period end and releases any scheduled downgrade; resuming clears the cancellation.
- **Licences and quotas:** on activation the tenant receives the plan's licences and user quota; users holding licences outside the plan are disabled and reset to free. When Stripe reports the subscription unpaid, expired or cancelled, the tenant drops to no licences and a quota of 1. A failed payment marks the subscription past due, which blocks gated features at once (there is no grace period).
- **Enforcement:** licence, permission, case-management, user-management and IOC alert-scan checks return 402 with an X-Subscription-Error header (SUBSCRIPTION_REQUIRED or SUBSCRIPTION_EXPIRED); unsubscribed tenants see a paywall on everything except Homepage and Billing; the 30-day trial check is skipped for these tenants; expired subscriptions are re-checked against Stripe in the alert loop and on billing reads.
- **Billing page:** Profile &gt; Billing (/dashboard/profile/billing) shows the plan, a change preview, payment method, cancel/resume and the last 50 invoices with receipt links, served by /api/billing/status, subscribe, confirm-payment, setup-intent, payment-method, preview-change, change-plan, cancel-downgrade, cancel and resume.
- **Per-tenant Stripe settings:** the platform administrator stores Stripe keys for the platform and a primary-tenant maintainer stores keys for their own tenant (System Settings and Tenant Settings, /api/billing/stripe-config); a sub-tenant is charged through its primary tenant's keys when set, otherwise the platform's. Keys are encrypted at rest and the secret key and webhook secret are never returned.
- **Pricing configuration:** a platform price override and a primary tenant's override for its sub-tenants (/api/billing/pricing-config); saving emails the affected maintainers and re-prices active subscriptions from the next cycle with no proration.
- **Discounts:** a per-tenant discount of 0 to 100 percent, set by the administrator for top-level tenants and by the primary maintainer for sub-tenants, applied as a Stripe coupon with an email to the maintainer.
- **Tenant Management controls:** a Subscription Required toggle (permanent once saved, not allowed on primary tenants), a discount field and subscription status badges; turning the toggle on resets the tenant to a quota of 1 with no licences.
- **Webhook and records:** POST /api/stripe/webhook verifies the Stripe signature, stores event IDs so repeats are ignored, and records one payment-history entry per invoice (amount, paid or failed, period, hosted invoice link). Backups keep the live subscription values on restore.
- **Emails and audit:** a payment receipt per paid invoice, a pricing-updated mail and a discount-updated mail; the audit log records scheduled downgrades, cancellations and resumed cancellations.
- **Content policy:** the client allows Stripe's script and frames (js.stripe.com, hooks.stripe.com, m.stripe.network) and the payment permission; the backend uses the stripe Python library (11.0.0 or newer) and the client @stripe/stripe-js 10.2.1.
- **Tests:** 12 backend tests and a 9-case Cypress spec (25-subscription-billing), which runs against fixture routes rather than Stripe.

### Network Intelligence & Scans

- **IP Scan depth and host name:** users choose top 100, top 1,000 (default) or top 5,000 ports; IP Scan accepts an optional host name used as the TLS server name and HTTP Host header (Host Recon sends the searched domain); results show, per open port, the identified product or "Service not identified" with a reason, the HTTP status, server and page title for web ports, and the TLS versions with the cipher used for each.
- **DNS detail:** Host Recon returns the full record set (A, AAAA, MX, TXT, NS, CNAME, SOA, CAA), parent-zone records for host names, SPF, and DMARC with a fallback to the parent domain, and marks results partial when individual lookups fail.
- **Fresh scans:** Host Recon and IP Scan pass force_new through, so a search runs a new scan instead of reusing an earlier result.
- **Authenticated vulnerability scans:** URL vulnerability scans accept optional login credentials (username and password, or an API key), offered for High and Full depth scans; supplied credentials are encrypted at rest in the scan job and removed from scan details and resume data.
- **Findings explorer and PDF:** findings are grouped by issue type with an instance count, searchable and filterable by severity, each showing CVSS, status, parameter, CWE, WASC ID, confidence, source, other info, fix guidance and the test request/response with highlighted evidence; vulnerability PDF exports group findings by type in severity order with a severity chart, CVSS scores, fix guidance and up to 100 detailed instances per issue.
- **Crypto wallet watchlist:** crypto scans report whether a wallet is on sanctions, crime or risk watchlists (14 lists checked, matching list names, categories and sources) instead of a heuristic risk level.
- **Scan concurrency:** each user can run at most 3 scans at once, starting a new scan of the same type cancels the older running one as "replaced by a newer scan", and scan requests are limited to 16 KB.
- **APK Security Assessment:** APK results show a Security Assessment with a risk level, a score out of 100, counts by severity and findings carrying CWE, MASVS, OWASP Mobile, confidence, origin (app, library or both), evidence, fix guidance and scan coverage; it is included in the export and translated into 18 languages.
- **File scan status:** POST /api/file/scan/status polls a running file scan by filename and SHA-256 every 3 seconds instead of uploading the file again; APK uploads may be up to 256 MB (from 30 MB), other files 10 MB, and the File Scanner accepts .apk files.

### CTI Graph & STIX Export

- **Graph name search rework:** matches spelling variants (spaces, underscores, hyphens, camel-case, letter/digit splits, defanged URLs, `www.` hosts, md5:-style hashes, canonical actor/organisation/ATT&CK names), ranks exact matches first then spreads results across clusters newest first, tries prefix and actor-variant lookups when nothing matches, and caps each query at 30 seconds; a multi-criteria batch query and cluster-scoped queries are available.
- **STIX 2.1 export rework:** bundles carry the threat group as an intrusion-set, identities for the publishing source, victim and author, and targets/uses relationships; infrastructure only for onion or I2P sites, indicators only for attacker-controlled or compromised values, standard TLP:AMBER marking, session IDs stripped from URLs, and links to leaked data no longer exported.

### Exports

- **Redesigned PDF exports of graphs, alerts, credentials and scans:** a dark cover page with the tenant logo, an Executive Summary (records by section, summary details, report details: prepared for, context, generation time, classification), the tenant logo in page headers, severity badges, and a clear message when an export has no records.

### Orion Mail Integration

- **Tenant report mailbox:** every new tenant gets a shared &lt;slug&gt;\_report@&lt;mail domain&gt; mailbox (migration 1.0.3.20 backfills existing tenants); tenant admins and maintainers open it through Orion Mail SSO; it sends and receives takedown mail.
- **Mail domain and unread count:** a primary tenant with a verified custom domain can move its mail to mail.&lt;root domain&gt; (sub-tenants on &lt;slug&gt;mail.&lt;root domain&gt;), with the Mail link and SSO following that domain; GET /api/orion-mail/unread-count returns the user's unread count for the badge.

### Data Migrations (target 1.0.3.29)

- **Stemming (1.0.3.19):** adds an English light-stemming analyzer and .stem subfields to every text field in 9 indices and updates existing records in place.
- **Duplicate cleanup (1.0.3.22):** removes duplicate records from 30 collector feeds (20 leak, 4 exploit, 3 marketplace, 3 social) using title, link, rewrite and drop rules, backing removed records up to dedicated indices, keeping records referenced by alerts, cases, feedback or takedowns, and copying missing fields into the record that stays.
- **Team and actor attribution (1.0.3.23):** for 237 collector scripts across leak, defacement, exploit and malware indices, sets the team field to the publishing source and the attacker field to the threat group, using lower-case reference names and backing up previous values.
- **Unreliable field removal (1.0.3.26):** removes 35 unreliable extracted fields, mostly personal identifiers (US SSN, passport, driver licence and bank numbers, UK NHS and NI, Australian and Indian IDs, XMPP addresses), from 9 indices.
- **Sealed secrets and hidden alert credentials (1.0.3.27-1.0.3.28):** encrypts stored tenant SMTP passwords and alert-connector secrets, and replaces passwords and other secrets in stored stealer-log and email-breach alerts with "Hidden".
- **Billing fields (1.0.3.29):** adds subscription_required (false) and discount_percent (0) to tenants and subscription (false) to users where missing; no existing tenant becomes subscription-managed.

### Operations & Deployment

- **./run.sh pull:** resets the working tree and pulls trusted-main using the GITHUB token in .env.
- **Fast production deploy:** ./run.sh build -p rebuilds only the client and restarts the web container; ./run.sh build -p -full runs the full rebuild (dependency install, image builds, service recreation, NGINX validation, NGINX and Caddy restarts).
- **Edge TLS routing with Caddy:** production NGINX routes TLS on port 443 by server name; platform hostnames go to NGINX and all others to a new Caddy service that issues certificates on demand for verified custom domains, the two using the PROXY protocol on a dedicated edge network; caddy and tor join the production services.
- **Cloudflare maintenance worker:** cloudflare/maintenance-worker.js serves a maintenance page when the origin is unreachable or returns Cloudflare 520-530, and returns users to the site once the app responds.
- **Encryption key rotation:** backend/rotate_encryption_keys.py re-encrypts tenant data keys, tenant SMTP passwords, alert-connector secrets and parser files under a new ENCRYPTION_KEY, with ENCRYPTION_KEY_PREVIOUS still decrypting old data and export imports.
- **New configuration keys:** CUSTOM_DOMAIN_TARGET, TOR_ONION_PASSWORD, STORAGE_BASE_URL, STORAGE_TOKEN, STORAGE_CERT_PATH, STORAGE_TIMEOUT_SECONDS, COUNTRY_INSIGHT_DAYS, ENCRYPTION_KEY_PREVIOUS, ORION_MAIL_INTERNAL_URL, ORION_MAIL_CLIENT_TIMEOUT, ORION_MAIL_TAKEDOWN_TIMEOUT, ORION_MICROS_INTERNAL_TOKEN, ORION_NEXUS_INTERNAL_TOKEN.
- **Browser extension distribution:** the bundled Orion Social Chrome and Firefox packages move from 0.6.5 to 0.6.21 (Chrome store package and Mozilla-signed Firefox package, with updates.json naming 0.6.21), adding encrypted Vault autofill, case-communication screenshot and screen-recording capture, per-browser instance identifiers, Google profile search, LinkedIn session verification and the capture-window fixes listed under Orion Crawler.
- **Documentation PDF:** ./run.sh -doc (or -docs) also builds docs/\_pdf/Orion-Documentation.pdf with a cover page and a page-numbered table of contents; a failed PDF build does not stop the run.
- **Safer run.sh:** run.sh refuses to run under sudo outside production runs and edits .env values in place.
- **CI on persistent runners:** the Build and Test workflows wait up to 10 minutes for a leftover apt or dpkg process before installing packages, pass --batch --yes to gpg, and remove the trusted-search compose project (the one run.sh starts) in every cleanup step, with a clean-up before run.sh in Build.

### Documentation

- User manual split into Orion Core, Orion Mail, Orion Leaks and DeepIntel parts, with new sections for Marketplace, the reworked Social Monitoring, Identity Resolution, Delete Your Account, Orion Vault, Take Down (social takedown, tenant mail activation, Takedown Mail) and Tenant Settings (custom domain, webmail, tenant onion addresses), and rewritten Host Recon and IP Scan.
- Help manual adds task guides for Marketplace, Identity Resolution, Kill Chain and Deep Intel, and Orion Vault; API docs add a page-snapshot report page and rewrite the STIX, IP Scan, Host Recon, crypto scan and insight descriptions.
- New manual sections "Password Autofill on Other Websites" and "Create a New Login on a Sign-Up Form"; 219 documentation screenshots regenerated.

### Removed (for reference)

- Per-page Nexus chat widgets in search results, reports, API lookup pages, text analysis, IOC search and case details; Nexus is now reached from the AI Workspace and the sidebar Help & Support launcher.
- AI Workspace GitHub repository import and the workspace status, tree and file-read endpoints; the Working Directory lists session exports instead.
- Threat Content Type filter (40 categories) and the Exploit Type filter across Leak, Feed, Social, Consolidated and Threat Lens; built-in entity-filter suggestions (now served by the backend).
- **Retired entity types:** Australian IDs, US IDs, US bank numbers, UK NHS, US driver licences and XMPP addresses from entity filters, IOC types and CTI Graph.
- IP Scan vulnerabilities, risk flags, misconfigurations and weak protocols (IP Scan is reconnaissance only; vulnerability testing is in the URL scan).
- Defacement team-group cards and the Defacement_Groups response block; the Take Down Evidence column and "Closed" label (replaced by the View dialog).
- Keyword search payloads no longer written to the audit log (stealer-log searches and scans still recorded); account update no longer changes username or email; production Swagger UI (/docs, /openapi.json) served only in debug; SMTP takedown mail replaced by Orion Mail; crypto risk level replaced by the watchlist check; the per-request heatmap days parameter.
- The Impersonation Detection page, its sidebar item, the five /api/social/impersonation-detection routes and their manager, model and indexes (two-profile Identity Resolution remains); the Identity Resolution swap button, "Caveats & Analytical Limits" block and matched-keyword chips.
- The external-link confirmation popup and the client-side 20 MB case-upload pre-check (the server still rejects larger files).

---

## 2. Orion Dark Nexus

### Kill Chain (new agentic offensive-security engine)

- **kill_chain MCP tool:** a fifth MCP2 tool that executes one authorized-pentest kill-chain node against a scoped target (a chosen tool or auto-selected), and in design mode returns a complete multi-phase attack-chain plan as JSON.
- **Autonomous hypothesis engine:** forms, chains, confirms and refutes falsifiable hypotheses across recon, scan, access, exploit and actions phases, evolving one hypothesis at a time from accumulated evidence until the objective is met.
- **Red/blue stance:** each engagement is auto-classified as red-team (offensive, may use dangerous tools) or blue-team (non-intrusive, safe tools only), which gates what the chain may do.
- **Execution modes:** design, run, hypotheses, scenario (single-hypothesis), refine (revise a chain that produced nothing), report, data, catalog and stop, over a persisted node graph with live progress streaming.
- **Evidence perceptor and verifiers:** raw tool output is converted into typed, evidence-grounded facts (tech, service, port, vuln, cred, session, exploitation_proof and \~30 more), and separate verifier/finding/session validators judge whether each hypothesis is truly confirmed on the live target.
- **Credential/vuln/session/evasion binders:** discovered credentials, confirmed CVEs, captured sessions and WAF-evasion arguments are threaded into downstream tool invocations, and stealer-log credentials are projected as testable cred facts.
- **Orion intelligence as chain data sources:** chain nodes can query Orion's own data APIs (web search, entity lookup, breach lookup, stealer-log credentials, leak/paste search, dark-web search, threat-intel/IOC) as passive, licence-gated sources preferred before active tools.
- **Offensive code synthesis:** a transform node type synthesizes and runs exploitation/transform code in the sandbox against upstream findings.
- **Screenshot capture:** image-producing tools (e.g. gowitness) have their screenshots captured, encrypted and attached under a per-user quota.
- **Kill-chain REST container API:** a /v1/kill-chain surface lets the workspace create, load (by id, latest, or by chat), save and delete the persisted graph/scope bound to a chat.
- **Markdown engagement report + export store:** findings are consolidated into a senior-pentester-style Markdown report (executive summary, key findings with severity, hypothesis assessment, attack surface, recommendations), persisted and downloadable via /exports/kill-chain/{chat_id} backed by Orion Storage.
- **Multi-engagement campaigns [uncommitted]:** a campaign registry and store group related engagements, with a credential ledger, password-candidate generator, evidence posture/guard gating, report gate, intent classifier and payload-variant analyzer.

### Kill Chain conversational control (kill_chat)

- **Natural-language controller:** operators talk to a running engagement in plain language, and each message is routed to one operation (answer, inspect, semantic search, inspect a named node, form hypotheses, run/stop/re-arm, report).
- **Conversational graph editing:** analysts add, remove, connect, re-run or stop nodes and retarget the whole engagement to a new host by describing it, with confirmation before structural changes and an auto-run flag when they ask to execute now.
- **Semantic search over the engagement:** a per-engagement index finds nodes, tool outputs, extracted facts and hypotheses by topic and answers in prose without raw IDs or JSON.

### Penetration-testing arsenal

- **Declarative YAML tool engine:** rebuilt from \~50 hand-coded per-tool Python modules into 286 declarative YAML specs (264 committed plus 22 in the working tree) driven by a shared metadata/router/spec engine, output transformers and bundled wordlists, backing both the pentest tool and the kill-chain.
- **Phase- and safety-tagged catalog:** every spec carries a phase (recon/scan/exploit/access/actions), a safety level (safe/requires_review/dangerous) and a host/url scope, so the planner picks purpose-built tools and the blue stance restricts itself to safe ones.
- **Active Directory / Kerberos suite:** \~35 impacket specs (Kerberoasting, AS-REP roasting, secretsdump/DCSync, delegation discovery, DPAPI, LAPS/GPP passwords, ticketer/golden-ticket, lateral psexec/wmiexec/smbexec/atexec/dcomexec) plus ADCS enumeration (certipy, nmap) and kerbrute user enumeration.
- **NetExec (nxc) suite:** \~24 specs including Zerologon, PetitPotam, NTDS/SAM/LSASS dumping, LAPS/GPP, SMB spidering, BloodHound collection and per-service checks.
- **Credential brute-force suite:** Hydra brute/spray across 14 services (SSH, FTP, SMB, RDP, VNC, LDAP, SMTP, IMAP, POP3, MySQL, MSSQL, Postgres, Telnet, SNMP) and hash cracking via hashcat and John.
- **Web exploitation-proof tools:** SQLi/NoSQLi/command-injection/LFI/XSS exploit proofs, auth-bypass proof, SSTI and DOM-XSS (dalfox), request smuggling, CRLF, cache poisoning and SSRF callback checks.
- **IoT/OT and infrastructure probes:** Modbus (mbpoll), MQTT (mosquitto), CoAP and IKE probes, masscan/naabu/UDP sweeps, firmware analysis (binwalk) and broadcast discovery.
- **Cloud attack specs [uncommitted]:** AWS STS identity, IAM read enumeration, IAM policy simulation, role-trust walking, privilege-escalation audit and S3 bucket-ACL checks (SigV4 signing helper).
- **Identity-federation attack specs [uncommitted]:** OAuth redirect-URI probing, OIDC token substitution and SAML token replay.
- **Phishing / initial-access specs [uncommitted]:** mail-lure sending and IMAP mailbox polling.
- **Pivot and post-exploitation specs [uncommitted]:** SOCKS-over-SSH pivoting and Linux post-exploitation enumeration, with pivot transport threaded through tool arguments.
- **Additional exploit proofs [uncommitted]:** Chromium-confirmed XSS execution, webshell-upload proof, SSTI execution proof, deserialization gadget probing, allowlist-bypass fetching and TLS-fingerprint impersonation (curl-impersonate).
- **Adaptive pacing [uncommitted]:** request pacing/rate control so tools slow down when a target rate-limits.

### Agentic chat and reporting

- **Human-in-the-loop clarification:** the open-chat graph can pause once and ask a single clarifying question with concrete options when a major ambiguity would otherwise produce a wrong result, resuming with the answer.
- **Trained intent router:** a 6-class category_route classifier routes requests to api / docs / guidance / pentest / websearch / out-of-scope, replacing heuristic routing.
- **API-vs-Kali router:** a binary api_kali_route classifier decides whether a request is answered from Orion data APIs or the Kali arsenal.
- **Threat-report composer with downloads:** consolidated threat-intelligence reports with per-section record cards/tables, overview and "crux" narratives, and a downloadable full-record report when results exceed the inline cap.

### Report exports

- **DOCX, JSON and STIX exports:** the export manager renders reports as Word, JSON and STIX 2.x bundles (domain/IP/email/URL/hash indicators and CVE vulnerability objects), selectable by format.
- **Encrypted, expiring delivery:** exports and captured screenshots are Fernet-encrypted at rest under per-user directories with a Mongo-tracked TTL and authenticated download endpoints.

### Classifiers (Dark Nexus FastAPI service)

- **Scam classifier:** POST /scam_classifier/predict labels text as scam vs legit.
- **Hate-speech classifier:** POST /hate_speech_classifier/predict classifies text as hate speech, offensive or safe.
- **Ad-detection classifier:** POST /ad_detection_classifier/predict predicts the topic of ad text.
- **OCR / text-security classifier:** POST /ocr_classifier/analyze_text scans text for spam and malicious/phishing/suspicious URLs.
- **Threat-tag classifier:** a multi-label classifier assigns threat tags from an embedding vector using trained per-tag weights and thresholds.
- **Model integrity manifests:** SHA-256 manifests for the ad-detection, hate-speech, scam, spam and URL models so they are provisioned and verified rather than committed.

### Platform and operations

- **Classifier training/provisioning:** run.sh provisions classifiers through a preparation script with a selectable training device (CPU/GPU).
- **GPU auto-recovery:** an Ollama GPU post-resume hook restarts the local model engine after the host wakes, plus a GPU-availability pre-check.
- **MCP Inspector launcher:** a script/run.sh path starts the MCP Inspector against the gateway.
- **Three more specs [uncommitted]:** api_key_replay_curl, smuggling_diff_probe and dns_open_resolver_check; 50 existing specs retuned.
- **Session keepalive [uncommitted]:** the gateway refreshes its Orion session every 600 seconds and signs in again if the refresh is rejected.
- **Mongo timeouts [uncommitted]:** the Mongo client sets server-selection (30 s), connect (20 s) and socket (60 s) timeouts.
- **Kill-chain lab fixtures [uncommitted]:** the development compose file adds a vulnerable fixture server and an internal nginx target for kill-chain tests.

### Removed (for reference)

- The \~51 hand-coded Python pentest tool packages, replaced by the declarative YAML spec engine.
- The in-process pdf_export module, replaced by the multi-format shared/export system.
- \~22 one-off routing/coverage/ranking stress scripts, replaced by the structured probe-manager harnesses.
- Two committed classifier tokenizer artifacts (\~250k lines) moved to model provisioning (the classifiers remain in use), accounting for essentially all of the \~500k deleted lines.

---

## 3. Orion Mail

### Compose and Sending

- **Rich-text editor:** a Quill composer with Small/Normal/Large/Huge sizes, text and background colours, numbered and bulleted lists, four alignments, a link panel and clear formatting.
- **Undo send:** sent messages are held 10 seconds with an Undo button, and undoing reopens the composer with recipients, text, attachments, Storage files and password settings restored.
- **Schedule send:** a Schedule menu offers Tomorrow morning (08:00), Tomorrow afternoon (13:00), Monday morning (08:00) or any date and time up to 365 days ahead, storing the whole send package encrypted until it goes out; a Scheduled folder with a count lists pending sends, each editable (with attachments restored), reschedulable or cancellable to Drafts.
- **Attach from Storage:** an "Insert files from Storage" dialog (Recent, All files, Upload) adds stored files, shared as encrypted per-recipient links when the mail is end-to-end encrypted to Orion users and attached as normal files otherwise.

### Self-Destructing and Password-Protected Mail

- **Message expiry:** a per-message Expire setting (Never, View once, 1 hour, 1 day, 2 days, 7 days) destroys the body, attachments and raw source when the period ends; View once is wiped on first open, with attachments downloadable only during that view.
- **Self-destructing links for external recipients:** outside recipients get a notice email linking to a public self-destructing page and a generated 14-character password, with rate-limited attempts and expiry/already-viewed messages.
- **Password-protected messages:** when a message will not be end-to-end encrypted, a lock button seals it and its attachments in the browser with a password of at least 8 characters, an optional hint and an expiry of 7, 14 or 28 days; the recipient opens it on a public secure page and can send up to 20 encrypted replies to the sender's Inbox.

### Inbox and Reading

- **Pinned messages:** a pin button keeps chosen messages at the top of the Inbox, most recently pinned first.
- **Snooze:** a message can be snoozed until a chosen time, stays out of the Inbox and Inbox search until then, appears in a Snoozed folder with a count, and can be brought back early.
- **Unverified sender warnings:** the message view shows an "Unverified sender" banner with a Report spam action when SPF, DKIM or DMARC failed or an OpenPGP signature does not match, and confirms before opening an external link or loading remote images.
- **Theme-aware HTML mail:** HTML mail is shown in an isolated frame that follows the light or dark theme; in-app confirmation dialogs replace browser prompts for delete, empty folder, report, block, trust key, discard draft and cancel schedule.

### Encryption, Signing and Keys

- **Signed outgoing mail:** non-end-to-end mail is signed in the browser as PGP/MIME and can carry the sender's public key as an .asc attachment (new "Sign outgoing mail" and "Attach my public key" settings, both on by default), with the server checking the signature before sending.
- **Automatic encryption to external recipients:** mail to an outside address is sent as PGP/MIME encrypted when the recipient publishes a key via Web Key Directory or the user has trusted their key; the composer shows a "Not end-to-end encrypted" indicator otherwise.
- **Web Key Directory publishing:** public keys of mailboxes and disposable addresses are published at the standard WKD endpoint for external OpenPGP clients.
- **Incoming signature verification:** signed incoming mail is checked against keys from Orion mailboxes, disposable addresses, trusted contacts or WKD and labelled "Signed by", "Signed - unknown key" (with a Trust key action), "Signature invalid" or "Signed - key not available"; invalid-signature mail goes to Spam.
- **Zero-access storage:** when the recipient has an end-to-end key, incoming non-E2E mail is encrypted on arrival to that key (attachments after the virus scan) and its raw source is not kept.
- **Reading external OpenPGP mail:** PGP/MIME and inline PGP messages are decrypted in the browser (subject, HTML body, attachments), and mail encrypted to a mailbox's server-held key is decrypted on arrival.
- **Encryption key required to send:** sending requires an end-to-end key, so the composer offers "Set up encryption", "Unlock to encrypt" or "Unlock to sign" and keeps the message in Drafts until the key is ready, warning when an Orion contact's key has changed.
- **OpenPGP toolkit:** a standalone PGP Tools page works entirely in the browser, generating Curve25519 or RSA 4096 key pairs (optional passphrase and revocation certificate), encrypting with optional signing and decrypting with optional verification, with copy and download.

### Storage

- **Encrypted Storage drive:** a Storage page keeps files on Orion Storage after encrypting them in the browser, with drag-and-drop or button upload, Recent and All files tabs, list and grid views, search, thumbnails, a details panel, download, delete and a usage bar against the per-user quota (default 500 MB, up to 30 MB per file).
- **File viewer:** stored files open in a viewer with full-screen mode for images, text and source code, PDF, Word (including legacy .doc), Excel, PowerPoint, CSV and TSV.
- **Shared files in received mail:** messages carrying Storage links list the shared files with a download button that fetches and decrypts each file.

### Settings and Identities

- **Vacation responder:** a Vacation / Out-of-office section takes a start and end time, a subject and a message and replies once per sender per vacation period from the receiving address, skipping automated, mailing-list, no-reply and the user's own addresses.
- **Blocked senders:** Block sender now blocks the exact address (not the whole domain) and routes its future mail to Spam, with a Blocked senders list in Settings showing each address and domain with Unblock.
- **Disposable address keys and daily limit:** each disposable address gets its own browser-generated OpenPGP key locked with the user's passphrase (re-locked on passphrase change or recovery), can reuse a saved key, and creation is limited to one new address per day on top of the cap of five active, with the remaining allowance shown in Settings.

### Messenger

- **Conversation layout and keyboard:** conversations are grouped under Today, Yesterday, weekday or date headings with grouped bubbles and auto-scroll to the newest message; the input grows as you type, sends on Enter and adds a line break on Ctrl/Cmd/Alt+Enter (on touch, Enter adds a line break).

### Delivery and Limits

- **Delivery failure notices:** unreachable recipients produce a "Delivery Status Notification (Failure)" from mailer-daemon in the Inbox listing each address with its SMTP code or reason, with a "Not delivered" badge in Sent.
- **Outbound spam blocking:** every outgoing message is scored by rspamd, and messages at or above the outbound threshold (default 12) are rejected back to the sender as 550 5.7.1.
- **Daily sending limits:** each mailbox can send at most 20 messages a day, to at most 100 different recipients, and no more than 10 to the same recipient, replacing the earlier 200-per-hour cap, checked before sending.
- **Size limits:** a mailbox can keep up to 100 drafts, and a message body can be up to 5,000,000 characters (up from 1,000,000).

### Orion Intelligence Integration

- **Takedown mail through Orion Mail:** Orion Intelligence sends takedown requests from each tenant's report mailbox with the evidence screenshot and page HTML attached, and abuse-desk replies are recognised, threaded and passed back to Intelligence for unread counts, thread view, mark-as-read and in-app replies with attachments, without appearing in the normal folders.
- **Tenant report mailboxes:** an internal API (authenticated with the SSO client secret) lets Intelligence create a &lt;slug&gt;\_report mailbox per tenant and check whether the mailbox and keys are ready; tenant admins and maintainers open it through SSO, which creates it on first sign-in.
- **Unread count for Intelligence:** Orion Intelligence can fetch a user's unread Inbox count for its Mail badge.
- **Account purge:** deleting a user or a tenant report mailbox in Orion Intelligence removes the matching Orion Mail account, mailbox, stored mail, keys and disposable addresses.
- **Tenant custom domains:** Orion Mail serves tenants on their verified custom domains (mail.&lt;tenant domain&gt;), checking each host against Intelligence's verified-domain list and routing sign-in and sign-out to the tenant's own Intelligence domain.

### Operations and Deployment

- **MTA-STS, DANE and mandatory TLS:** Postfix uses the Let's Encrypt certificate, requires TLS inbound and outbound and follows recipient MTA-STS policies; Orion Mail publishes its own MTA-STS enforce policy and a DANE TLSA record for its MX host.
- **Certificate automation:** ./run.sh cert issues one Let's Encrypt certificate via Cloudflare DNS-01 for the mail, mx and mta-sts hosts and publishes the TLSA record; ./run.sh renew refreshes both (requires CLOUDFLARE_TOKEN).
- **Orion Storage connection:** the web service connects to Orion Storage over TLS using STORAGE_BASE_URL, the mail token in STORAGE_TOKEN and a mounted CA, with DRIVE_QUOTA_MB (default 500) and DRIVE_MAX_FILE_BYTES (default 30 MB).
- **Startup data migrations (target 1.0.0.5):** rotate server-held RSA PGP keys to Curve25519 (keeping retired keys so older mail still decrypts) and remove stored Orion email addresses, full names and Bcc lists.
- **New settings:** OUTBOUND_SPAM_SCORE_THRESHOLD (12), DAILY_DISPOSABLE_MAILBOX_LIMIT (1), SMTP_VALIDATE_CERTS (true); ./run.sh pull updates from trusted-main using the GITHUB token.

### Removed (for reference)

- Message translation (Translate action and endpoint for 20 languages); mailbox Signature setting, per-disposable identity signatures and the Signed Identity Text block (replaced by OpenPGP/MIME signing); editable attachment size/retention settings (stored values now apply); plain-text compose mode (always rich text); run.sh backup, restore, logs and perms commands. The upgrade deletes existing disposable addresses and their server-generated keys, so users recreate them with browser-generated keys; new message-made blocks apply only to the exact address (existing domain blocks remain).

---

## 4. Orion Micros

- **Authenticated URL vulnerability scans:** scans accept an auth block (username/password, or an API key as a Bearer token) or a detailed options.authentication (form login with explicit login URL and a JSON-path or cookie token, a static token, or HTTP Basic); the scanner finds and submits the login form, adds the session cookie or header to every in-scope request, adds the post-login landing page to the crawl and keeps logout paths out of scope.
- **Orion custom scan rules:** High and Full scans load seven Orion ZAP rules - active checks for Host header injection, query-string parameter pollution and hazardous HTTP methods (TRACE confirmed by echo); passive checks for password fields without autocomplete=off, email disclosure, a missing Referrer-Policy header and sensitive HTML/script comments.
- **Logic and TLS checks:** username/password scans check session-cookie renewal at login (session fixation); URL_VULN_LOGIC_CHECKS=true adds account-lockout, query-string login and integer-overflow probes; HTTPS targets are checked for TLS 1.0/1.1, SHA-1 and weak cipher suites.
- **API import, AJAX spider and OAST:** the scanner probes seven OpenAPI/Swagger and three GraphQL paths and imports any definitions into ZAP; URL_VULN_AJAX_SPIDER turns on ZAP's AJAX spider and URL_VULN_OAST_SERVICE selects an out-of-band testing service.
- **CVSS scoring:** findings carry a CVSS v3.1 base score, vector and severity from a per-CWE baseline vector, or estimated from the risk level where the CWE is unmapped.
- **Highlighted request/response evidence:** each finding includes the captured request and response with highlighted spans (injected payload in the request; validating evidence, reflected payload and text/html context in the response) and an evidence_corroborated flag, plus up to 100 distinct instances with ZAP's solution, references and CWE/WASC IDs, kept in scan history; evidence is captured for up to 250 findings per scan (100 for full scans) instead of 25.
- **Broader high-depth coverage:** High scans plan up to 40 URLs (from 15), keep at most three URLs per path template, put query-parameter endpoints first, turn GET form fields into injectable parameters, send only reachable URLs to the active scan and cap sequential page fetching at 150 seconds.
- **Focused active policy and shorter full scans:** High and Full scans default to a 15-rule active policy (SQLi, reflected and stored XSS, path traversal, RFI, CRLF, code and OS command injection, SSI, parameter tampering, integer overflow, external redirect and the Orion scripts) at medium strength/threshold, with the rules, strength and threshold listed in the result; the full-scan active budget drops from 60 to 25 minutes and ZAP restarts on a fresh JVM before each High or Full scan (URL_VULN_ZAP_RECYCLE_EVERY).
- **IOC extraction from a URL or text:** POST /ioc/extract/text/{user_id} returns IOCs immediately for submitted text, or for a url downloads the page and up to two linked non-social pages, skips anti-bot interstitials and returns de-duplicated IOCs with the extracted text.
- **Crypto wallet watchlist screening:** Bitcoin wallet analysis screens the address against sanctions and watchlists through CompliAPI (COMPLIAPI_TOKEN) and reports flagged or not listed, sanctioned status, lists checked, matching list names, categories, entity names and sources, accepting only valid Base58 or Bech32 addresses.
- **Software Scanner sources:** software lookups query 13 release and tracker indexes in parallel within a 6-second deadline (xREL, srrDB, predb.net, predb.club, Knaben, The Pirate Bay, Solid Torrents, Rutor, Torrents-CSV, BT4G, TorrentDownload, BinSearch, GameStatus), returning up to 10 listing-page links per source.
- **Takedown evidence fallbacks:** when domain WHOIS has no email, evidence capture uses the hosting provider's RDAP abuse contact, retries the page load over Tor on a direct failure, still returns the abuse routing with screenshot_captured and capture_error when the page cannot load, and accepts force_new.
- **Host Recon DNS details:** domain resolution returns the parent zone's NS, SOA, MX, TXT and CAA records for subdomains, the DMARC policy inherited from a parent domain with that domain, per-record lookup errors with a partial status, and force_new.
- **More complete IP scans:** IP scans re-sweep unanswered ports, retry service detection up to three times for ports that close before sending a banner, give a reason for each unidentified service, accept force_new and run with a 14-minute budget (from 9).
- **Operations:** ./run.sh pull resets and pulls trusted-main using the GITHUB token; the Tor proxy runs from a locally built Debian Tor image with a persistent data volume and a health check that restarts it when a working SOCKS proxy stops answering.
- **APK security assessment (new):** APK analysis returns a vapt report from the orion-apk-vapt engine 1.0 with a risk level, a severity-weighted score capped at 100 (critical 40, high 20, medium 8, low 3), counts, findings and coverage; each finding carries rule, title, severity, confidence, category, CWE, MASVS, OWASP Mobile, impact, remediation, evidence, locations, occurrences and origin (app, library or both).
- **Native APK rules:** a 52-rule catalogue covers the manifest and exported components, network security policy (cleartext, user CAs, pinning), signing (unsigned, AOSP test keys, debug certificates, v1-only signatures), bytecode call sites (WebView, TLS, cryptography, storage), hardcoded secrets and native libraries (executable stack), with a 240-second bytecode budget and scan caps of 8 MiB per file, 96 MiB and 4,000 files in total.
- **MobSF merge:** each APK is also uploaded to a new MobSF service (trusted-micros-mobsf, API only, MOBSF_ADDR and MOBSF_API_KEY) in parallel; its findings are merged under a MOBSF- prefix, MobSF rules that overlap the native rules are dropped so the native finding is kept, and trackers, packers, Firebase, keystore and certificate findings are added. If MobSF is down or exceeds 500 seconds, the error is recorded in coverage and the APK result is still returned.
- **File scan status and APK detection:** POST /file/scan/status/{user_id} returns a cached job's status by filename and SHA-256; APKs are recognised by content (AndroidManifest.xml with classes.dex or resources.arsc) instead of the file extension.
- **ClamAV sizing:** ClamAV memory rises from 1 GB to 3 GB with 256 MB stream and file limits and a 1,024 MB scan limit; the antivirus client reconnects for each scan.

### Removed (for reference)

- NVD CVE matching, TLS/SSH weakness scoring, risk flags and Redis/FTP misconfiguration probes from Host Recon and IP Scan (now reconnaissance only; vulnerabilities come from the URL scanner).
- The heuristic crypto wallet risk_assessment (replaced by watchlist screening).
- The APK weak-crypto string match (MD5, SHA-1, DES, RC4 names) and its crypto_info block, replaced by call-site cryptography rules.
- US passport, US driver's licence, US bank account, US ITIN, medical licence and UK NHS numbers from NLP/IOC extraction.
- Nine more ZAP passive rules off by default (URL_VULN_DISABLED_PASSIVE_RULE_IDS), and the browser-observability section (screenshots, DOM snapshots, network graphs, console, cookies, storage) from URL scan results.

---

## 5. Orion Social

- **Two-profile identity resolution:** POST /social/identity-resolution takes a reference and a candidate profile URL on any of 14 platforms (Instagram, Facebook, X, Threads, TikTok, YouTube, Reddit, Pinterest, Quora, Patreon, MeWe, OK.ru, Hashnode, Behance), collects both through custom recon, and returns an impersonation-risk score, a same-person likelihood, a field-coverage confidence score, a threat assessment (threat type, scam categories, takedown eligibility, recommended action) and an evidence matrix.
- **Impersonation signals:** the comparison weighs about 20 signals - handle similarity with homoglyph, leetspeak and digit-padding detection; display name; avatar similarity by perceptual hashes and colour histogram with mirrored-image detection (enable_image_comparison); bio similarity and cross-mentions; resolved link-in-bio targets; shared contacts and domains; follower, following, post-count and account-age gaps; verification, privacy, scam-intent keywords and parody disclaimers.
- **Impersonation takedown automation:** POST /social/automation/takedown files impersonation reports on ten platforms (LinkedIn added) and returns submitted or failed - Facebook, Instagram, Threads, Pinterest, Patreon and Quora through their support forms (reporter details, impostor URL, optional ID photo); Reddit, TikTok and YouTube through the in-app report flow of a captured session; TAKEDOWN_SKIP_SUBMIT fills the form, saves a screenshot and stops before submitting.
- **Ad monitoring of a chosen profile:** ad detection on all 15 platforms accepts an optional profile_url (checked against the platform's HTTPS domains) and scans that page instead of the home feed.
- **Hate-speech monitoring without a target:** hate-speech monitoring no longer requires profile_url; without one it scans the signed-in account's home feed (its own profile on X), and a supplied URL is domain-checked.
- **Recon over a Tor pool:** custom recon HTTP probes rotate across ten Tor instances (nine new compose services), and each of the five pooled recon browsers runs behind one; the nine new instances read per-instance torrc files provided on the server.
- **Repository pull:** ./run.sh pull resets and pulls trusted-main using the GITHUB token.
- **LinkedIn automation for LinkedIn's new page design [uncommitted]:** posting opens the composer through the "Start a post" button, uploads images through the file chooser and its Next step, waits for the Post button to enable and captures the published post's link from the account's activity page; ad and hate-speech monitoring read posts from the new feed and activity lists, and a profile URL is mapped to its activity page (or a company's posts page); the session check waits 45 seconds and for the network to settle and accepts the li_at sign-in cookie. Tested on a live account on November 21: a post with an image was published and its link captured, one promoted post was detected, and the account's posts were read.
- **LinkedIn impersonation report [uncommitted]:** the report follows More &gt; Report &gt; entire account &gt; "This person is impersonating someone" &gt; Submit report and fails with an error if any step is missing, instead of reporting success without filing; tested up to the enabled Submit button without submitting.

### Removed (for reference)

- The in-service toxic-bert hate-speech and DeBERTa ad-topic classifiers and their /social/hate-speech and /social/ad-detection endpoints, in-service result labelling and transformers/CPU torch (classification now runs in Dark Nexus and is called from Orion Intelligence); Pinterest board auto-creation now fails with a message asking the user to create one.

---

## 6. Orion Storage

- **Encrypted file storage service (new):** a TLS-only service at /api/storage/files/{type}/{userid}/{name} lets Orion Mail (mail), Dark Nexus Kill Chain (killchain) and Intelligence Case Management (case_management) upload, download and delete per-user files; each type opens only with its own bearer token, access can be limited with ALLOWED_IPS, and uploads never overwrite (a clash becomes "name (1).ext").
- **Encryption, integrity and malware scanning:** every file carries an HMAC-SHA256 integrity header bound to its location; all types except mail (already encrypted in the browser) are encrypted at rest with AES-256-GCM under STORAGE_ENCRYPTION_KEY; non-mail uploads are ClamAV-scanned and refused over 30 MiB; mail files expire after 7 days (STORAGE_TTL_SECONDS_MAIL); a one-off encode_legacy_files.py converts pre-header files.
- **Deployment:** ./run.sh build -storage -p (or -d) generates a self-signed TLS certificate with the host IP in its SAN, pulls ClamAV and starts the storage container on port 8081; ./run.sh pull updates from trusted-main using the GITHUB token.

---

## 7. Orion Tor2Web

- **Load shedding and failure memory (gateway 3.3.1):** each worker caps in-flight requests overall (T2W_MAX_CONCURRENT, default 2,000) and per onion site (T2W_MAX_CONCURRENT_PER_HOST, default 400), answering 503 with Retry-After beyond that; an onion service that also fails on a fresh circuit is answered from memory for T2W_FAILURE_CACHE_TTL seconds (default 20).
- **Upstream connection reuse:** GET and HEAD requests reuse connections to onion services (T2W_UPSTREAM_KEEPALIVE, on by default), keeping idle connections 30 seconds (up to 256 per worker) and replacing one silent for 15 seconds; uploads and POSTs always get their own connection.
- **Faster rewriting off the reactor thread:** bodies of 32 KB or more are rewritten on a separate thread so images keep streaming; the JavaScript scanner is about five times faster, HTML about 15% faster, and the backlog is capped (rewrite_backlog_max_bytes, then 503).
- **Wider rewriting coverage:** the rewriter handles quoted attributes with &amp;lt;/&amp;gt;, /-escaped and JSON-escaped clearnet URLs, CSS image-set(), lazy-loading data-\* attributes, bracketed or spaced paths, RSS/OPML url attributes and concatenated URLs; the injected runtime also covers script-set styles, insertRule, new Audio(), fetch(new Request(...)), Worker and EventSource subclasses, and spec-compliant srcset parsing.
- **Seamless worker recycling and status logging:** a recycled worker's replacement starts first while the old one finishes downloads for up to 30 minutes and tells kept-alive clients to reconnect; access log lines are batched and each busy worker writes a per-minute status line.
- **Throughput settings:** T2W_PROCESSES=auto starts one worker per CPU (2-8); T2W_CIRCUIT_BUCKETS can spread one site's visitors over several Tor circuits; Tor allows 128 circuits under construction (from 32); the edge nginx per-client limits rise to 256 connections and a burst of 600 requests; ./run.sh pull updates from trusted-main using the GITHUB token.

---

## 8. Orion Sandbox

- **Repository pull:** ./run.sh pull resets and pulls main using the new GITHUB token in .env.
- **Disk guard reclaims oversized workspaces:** when one workspace exceeds its physical storage limit, the disk guard deletes that workspace's sandbox and volume and keeps the OpenSandbox control plane running (instead of stopping it), and runs every 10 seconds instead of every minute.

---

## 9. Orion Control (new)

- **Crawler control panel:** a new web panel on the crawler host (FastAPI backend as the orion-ui systemd service, Angular 22 client behind nginx) to monitor and operate any number of k3s crawler servers, each defined in the environment file by id, label, kubeconfig, context, local/remote access, a build-host flag and log directories, with missing keys listed by name at startup.
- **Navigation and server switcher:** a collapsible sidebar (remembered, a drawer on mobile) holds Logs, Health, Environment and Commands and a server picker showing each server's label, id and a Build badge; the header shows the selected server, the running job with Cancel, a theme toggle and sign-out.
- **Live logs:** the Logs page streams the server's app and crawler logs into Live and Errors panes, reading dated log files every 2 seconds for local servers and tailing containers via kubectl every 5 seconds for remote ones, with source tags, level colours, file-change markers, search with match counts, clear, auto-scroll with a Latest button and a 5,000-line buffer.
- **Pod health:** the Health page polls pods every 15 seconds and shows healthy vs live pods, unhealthy pods, total restarts and finished jobs above a pod/container table (readiness, state, restart count highlighted above five, last exit reason).
- **Crawler environment editor:** the Environment page edits the crawler's instance-0.env ConfigMap with an unsaved marker, Revert, Save (applied at the next Stop/Start) and a confirmed Save and restart, rejecting text with no CELERY_COUNT\_ entries or over 200,000 characters.
- **Per-server terminal:** the Commands page runs shell commands on the control host with the selected server's kubeconfig and an ORION_SERVER variable, keeping each server's working directory between commands and showing output in a per-server console.
- **Build-host actions:** confirmed Pull and Build image buttons run on the build host (Pull fast-forwards the configured branch; Build logs in to Docker Hub and builds and pushes the image), writing output to every server's console.
- **Stop and Start:** Stop saves the current replica counts in an annotation and scales the crawler StatefulSet and app Deployment to zero while keeping queued work; Start restores the counts with a Tor-bootstrap warning; the action bar shows Stop or Start from a power state polled every 5 seconds.
- **Job tracking and cancel:** each action runs as a tracked background job (one at a time) with state, timings and exit code in the console, and the header's confirmed Cancel stops the job's whole process group; jobs run in separate lanes (shared for Pull/Build, per-server for Stop/Start/restart/manifests, per-server for terminal commands) so they no longer block each other.
- **Queue status and history API:** a per-server status endpoint reports, per enabled crawler category, configured workers from CELERY_COUNT\_\*, live workers, pending Redis tasks and any "stuck" category, saved hourly to a 48-hour SQLite history (API only, no UI yet); the backend also offers a five-step build-and-deploy, a per-server rollout restart and an apply-manifests-from-git Job (API only).
- **Operator sign-in:** a single operator account signs in with a show/hide password toggle, receives an HttpOnly SameSite=Strict session cookie expiring after 12 idle hours, and is locked out for 5 minutes after 5 failed attempts in 5 minutes.
- **Production install and modes:** ./run.sh build -p (as root) creates the env file with a random password, installs the backend venv, builds the client, installs and starts the orion-ui service and, after nginx -t, runs a Brotli nginx container on HTTPS port 8443 with a Cloudflare origin certificate, real client IPs, HTTP-to-HTTPS redirect, HSTS and noindex; -d runs on dummy data behind a self-signed local nginx; -l runs on a workstation against real clusters; ./run.sh kubeconfig USER@HOST copies both cluster kubeconfigs; stop, status, logs and pull manage the service.

---

## 10. Landing & documentation sites

### Orion-Intelligence-Solution (new public repo)

- **Public Orion repository:** publishes the Orion README and documentation with no platform source, including build/test/security/coverage/uptime and external scanner badges, a LinkedIn banner, documentation and live-status buttons, a quick start, a consolidated-search preview, a 43-screen gallery, an architecture flow and a ten-module table, with an orphan badges branch for the badge images.
- **Documentation project:** a Sphinx project (MyST Markdown, Shibuya dark theme, Read the Docs build on Python 3.12) containing only the public docs as five manuals (internal pages such as the Swagger API reference and organisational security policies left out).
- **User Manual (\~4,200 lines):** sign-up, login and onboarding, the dashboard and global search, every indexed module, Stealer Logs, the live lookups and scans, CTI Graph, Social Intel and Monitoring, report workflows, tenant/alert/takedown/IOC/feeder/case workflows, an Orion Mail guide, user/tenant/system administration including backup and restore, and a UI coverage appendix.
- **Help Manual:** 60 task topics (overview, navigation path, quick answer, steps, troubleshooting) from login and 2FA through search filters, each Entity API scanner, Social Intel, AI Workspace, takedown requests, custom alerts, backups and tenant exports, feeder scripts and branding to subscriptions, locked modules and error states.
- **Security Documentation:** 13 chapters written for SOC 2 Security and Availability readiness (authentication, access control, encryption, logging and audit, backup and recovery, monitoring, network and API security, tenant isolation, database security, secrets management, disaster recovery).
- **Introduction to Modules and Developer Documentation:** a module guide grouped by purpose, and a developer guide covering architecture, repository map, environment configuration, run.sh commands and flags, the scan-job runtime contract, Compose variants, local development and testing, the documentation and screenshot pipeline, tenant hierarchy and branding, and five contributor workflows.
- **Screenshot library and security policy:** 81 screenshots (71 platform, 10 Orion Mail) with sensitive values blurred; a security policy with GitHub private vulnerability reporting, an 8-hour acknowledgement and 2-day critical-mitigation target, scope, research rules, coordinated disclosure and safe harbour; a proprietary licence (v2.0, 26 Sep 2026) stating public visibility but no open-source rights, now naming Genesis Technologies [uncommitted].

### Orion-DeepAI-Landing (new)

- **DeepIntel.si landing site:** a single-page site (React 19, Vite, Tailwind) on Vercel at deepintel.si presenting DeepIntel.si, the Orion Intelligence AI analyst that watches an attack surface, flags anomalies, confirms them with a real pentest and writes up findings, running on the customer's own servers with local models.
- **Animated hero and capabilities stage:** a live telemetry animation (anomaly spike, detection ripple, captured event, time crosshair) with a typed triage log and status readout; a pinned scroll-driven stage through Continuous monitoring (15-minute cadence, 71 recon tools, sandbox confirmation), Automated pentesting (6-class router, up to 8 steps, STIX 2.1 exports) and Dark web monitoring (14.2B records, 4 source types, 240+ ransomware groups).
- **Navigation, contact and behaviour:** a fixed header (Monitor, Pentest, Dark web, Contact, Login, Get Access), a full-screen mobile menu, a desktop side navigator and section stepping; a "Talk to the Orion team" section above the Orion footer with booking and Get Access links now opening an email to support@orionintelligence.org [uncommitted]; both animations pause off-screen, cap pixel density and show a still frame under reduced motion; Vercel serves the page with a strict CSP, HSTS preload, frame blocking, a locked-down Permissions-Policy and Open Graph/Twitter metadata.

### Genesis Landing

- **New pages:** Team (26 people in six groups with co-founder cards and LinkedIn links), About (company figures, seven service lines, milestones 2018-2026, recognition, location), Portfolio (13 partners, 19 projects, 8 apps and games, 4 Upwork contracts), Careers (no roles open, invite to email), Contact (support email, booked call, LinkedIn, postal address), and Privacy Policy, Terms of Use, Security and Vulnerability Disclosure and Compliance pages (16 Nov 2026) with a switcher; a branded 404 page.
- **Home, navigation and footer:** the software stack gains a DeepIntel.si card, the Proof section adds the company figures and a "Check it yourself" row, and the contact section shows the support email; the header adds a Team link and skip-to-content; the footer links to the new pages and the four policies.
- **Full-site translation and metadata:** all pages translated into Spanish, French, German, Portuguese, Arabic, Hindi, Chinese, Japanese and Russian (\~1,090 strings each, loaded on demand, names in each script, ?lang=xx links, Arabic right-to-left); schema.org structured data and hreflang links for ten languages in the page and sitemap, a 1200x630 share image, a /.well-known/security.txt, an updated llms.txt, a new logo and favicon set, and a permanent www-to-apex redirect.

### Orion-Intelligence-Landing

- **New pages:** About Orion (platform figures, pipeline, module ecosystem, audiences, principles, company fact sheet), Contact (a topic-based message form opening the visitor's email app, with dedicated privacy, vulnerability-report, leak/incident and customer routes), and Privacy Policy, Terms of Service and SLA pages sharing an "At a glance" summary and a sticky contents list.
- **SLA:** first-response targets of 4 hours (critical), 8 hours (high) and 1 business day (normal) in US Eastern business hours, 48 hours' notice of planned maintenance, measured availability with 90-day history instead of a fixed percentage, incident updates until resolution, and exclusions.
- **Product tour and exposure check:** a seven-screen home-page product tour that auto-advances every 5 seconds (pauses on hover/focus/out-of-view/hidden tab, responds to keys and swipes, respects reduced motion); the home search accepts an email or username, keeps results in the browser for 24 hours (up to 100 searches), and the results page adds breached-site, associated-email, source and last-seen tiles, a 10-record table with expandable details (secrets hidden), a "Where it was breached" ranking and associated emails.
- **API documentation and pricing:** the API reference is rebuilt around 15 current endpoint groups in five categories with authentication, access, licence and limit notes (adding extension session, report-with-translation, stealer-log credential, social graph/identity and file/URL/crypto/phone/DKIM routes, dropping internal routes); the pricing page adds an SLA summary and all three plans are quote-based.
- **Status, links, footer and sitemap:** the stats strip links to the live status page, the contact block lists the headquarters, support email, documentation and status, Get Access and demo buttons open an email to support, and the header, footer and About page link to DeepIntel.si; the footer adds About, DeepIntel.si, Privacy, Terms, SLA and Contact links and a Genesis Technologies line; the sitemap and llms.txt list the new pages.

---

## 11. Orion Crawler (crawler, collector console & Orion Social extension)

### Collector families

- **Market collector family (new):** a new market family collects catalogues from darknet and clearnet marketplaces and vendor shops; its rules doc limits records to catalogue and presence metadata visible on the page (no card numbers, CVV, fullz or bank credentials, and no login, CAPTCHA or purchase automation), and it ships with an \_example.py template, a main.py runner and checkpoint dedup.
- **Market records and routing:** market records use a new market_model (market, vendor, category, price, currency, status, first/last seen, tags) and go into Orion Intelligence's market index with semantic embeddings, fed from a market feeder category and a crawler_market_queue Celery task sized by CELERY_COUNT_MARKET; console scan probes already run market scripts.
- **Ransomware group attribution on leak records:** leak entity records carry the publishing ransomware or extortion group as a normalised lower-case name in m_attacker and m_team; 152 of the 164 leak scripts set it, covering 122 groups (none did before).
- **FlareSolverr fetch mode for forums:** the forum rules allow FlareSolverr for Cloudflare-fronted boards that block both plain requests and headless Playwright, and 33 of the 92 added forum scripts use it (40 forum scripts in total).
- **News and tracking expansion:** the news family grows to 118 scripts (64 security-news and vendor threat-research sources, 54 government CERT, ICS-advisory and breach-registry sources), with rules for RSS/Atom/RDF parsing, fetching the article page when a feed body is under 1,500 characters, deep first-crawl backfill (stopping after 8 pages with no unseen items), multi-tag feeds and publisher-category relevance filters, and a 120-second budget per run.
- **CAPTCHA OCR for quarantined forum scripts:** ddddocr and Pillow are added to the requirements, used only by forum scripts still in the testing quarantine to read bot-guard and login CAPTCHAs.
- **Six social platforms restored:** Instagram, TikTok, LinkedIn, Quora, Imgur and Douban get their queue and rule types, Celery tasks and queues, worker lines and CELERY_COUNT settings back (default 0, so each is off until a count is set).
- **New platform scripts:** Instagram (six newest posts from the embed page), TikTok (ten newest from the embed page), LinkedIn (guest company pages, about ten updates), Quora (topic pages, ten answers), Imgur (web-app API through Tor), Douban (mobile API), Pleroma, HackerNoon and Stack Overflow (tag feeds at one request per 20 seconds; search through the Stack Exchange API, 300 calls a day per address or 10,000 with STACKEXCHANGE_API_KEY).
- **Reworked platform scripts:** Twitter reads tweets from X's embed timeline without Tor (30 requests per 15 minutes per address, with a pause kept in Redis) and emits the profile card every run; Reddit reads posts from Reddit's onion site with a browser over Tor; Mastodon uses the API only (3 requests per run, down from about 250) with a per-host rate-limit pause and comments from the thread context.
- 20 new leak-site scripts (19 onion, one clearnet), each setting the publishing source and the threat group; the leak family is now 164 scripts.

### Crawler pipeline and runtime

- **HTML evidence snapshots for leak and forum records:** each newly sent leak or forum record gets a self-contained, sanitised HTML snapshot of its own card area or thread page (scripts and handlers stripped, a strict CSP set, only used CSS kept, images and fonts inlined under size caps, capped at 1.5 MB); if the page is walled or the card is not found, a snapshot is built from the parsed fields; snapshots are uploaded to /api/snapshot, deduplicated for 30 days and linked from forum records' m_screenshot, so plain-requests sources now carry page evidence.
- **FlareSolverr clearance reuse:** FlareSolverr sessions keep each host's solved Cloudflare clearance cookies and user agent in Redis for 20 minutes, so later pages are fetched directly and FlareSolverr solves again only on a challenge or a 401/403/429/503.
- **FlareSolverr proxy routing and failover:** when a rule uses a proxy such as Tor, FlareSolverr solves through it; sessions fail over between the configured endpoint, the Compose service and local ports 8291 and 8191 with up to three backoff attempts.
- **Parallel category dispatch:** at start-up the dispatcher fetches and queues every enabled category's feeder list in parallel (optional cap in DISPATCH_PARALLELISM), so a large list no longer delays later categories.
- **Queue backpressure:** initial and periodic refills skip a category whose queue already holds REFILL_MAX_PENDING tasks (default 1,000).
- **Queue de-duplication:** the scheduler skips URLs already waiting in a queue and logs them as already queued; generic onion and I2P queues now count towards the backpressure limit (they never throttled before).
- **Telegram channel prioritisation:** each Telegram pass crawls channels with no stored offset first (chat and stealer-log lists) and waits 1-2 seconds between history pages instead of 1.5-3.5.
- **Telegram resumable backfill:** a new channel stores its newest message ID and then backfills 2,000 messages per visit, back to 900 days, through a cursor kept in Redis (one visit previously tried up to 500,000 messages).
- **Kubernetes-ready endpoints:** the crawler image reads Tor SOCKS and control endpoints from TOR_ENDPOINTS, the I2P proxy from I2P_PROXY_HTTP/HTTPS and Redis from REDIS_HOST, falling back to the Compose addresses.
- **Orion Uptime heartbeat:** when HEARTBEAT_URL is set, the crawler pings it after successful posts, at most once every 5 minutes per Redis instance.
- **Worker DNS settings:** CRAWLER_DNS_1 and CRAWLER_DNS_2 (default 8.8.8.8 and 9.9.9.9) set the Celery worker container's DNS servers.

### Collector control console

- **Market family in the console:** the console discovers the market family, runs scan probes for its scripts against the market index type, and pushes them to the feeder under the market rule key.
- **Session files in zip downloads:** family zip downloads include each script's stored session file next to the script.
- **Extension test coverage view:** the extension test page lists the new crawl types for 1,015 platforms but keeps the old verification flags, so it shows 1,015 as Partial and 111 as Verified until those platforms are re-verified.

### Orion Social extension

- **Evidence mode for case communications:** communication links opened from a case open in an evidence-mode capture window that reopens the last saved page at its saved scroll position and stores the page URL and scroll position on save.
- **Screenshot and screen-recording evidence:** the evidence card adds Take screenshot (a PNG of the visible tab, card hidden during the shot) and Start/Stop recording (video-only WebM at 1 Mbps, stopped automatically at 19 MiB), with a thumbnail and count, sending each capture to the open Orion tab tagged with its case and communication.
- **Orion Vault autofill relay:** in evidence windows, passwords come only from the Orion Vault - one badge per field and a drawer that opens below the address bar list the saved logins, "Click to auto-generate" creates and saves a password for the typed username, typed passwords are cleared, replies arrive over an encrypted channel (ECDH P-256, HKDF, AES-GCM), and a locked vault is brought to the front to unlock and then fills automatically.
- **Web-storage restore behind a storage cover:** evidence sessions write the saved localStorage, sessionStorage and IndexedDB on the site's own origin by first loading robots.txt under a full-page "Verifying session…" cover and waiting up to 5 seconds, so sites that keep sign-in state in web storage reopen signed in.
- **Address bar in capture windows:** session capture windows open as popups with an injected address bar showing the live URL with a lock indicator, a Copy URL button and, in evidence sessions, an Open saved page button; the bar pushes the page down by its own height (fixed and sticky site headers are moved with it) instead of covering the top of the site, and the Session Generator card starts below the site's header until the user drags it.
- **Keyword-narrowed crawling:** crawl commands accept a keyword and the extension returns only items whose fields contain any of its terms, scanning up to 5 more pages if the first has no match; Orion's social profile search uses this.
- **Per-install instance routing:** each install keeps a persistent instance ID sent on the task socket and in its presence message, so Orion routes commands to the extension in the user's current browser.
- **A resource module for every crawler:** all 1,097 crawlers now use their own per-site resource module (1,017 new), replacing the shared uCoz, OpenGraph, XenForo, Discourse and generic-forum modules that 590 crawlers used before.
- **Content crawling on almost every crawler:** crawlers fetching more than profile details rose from 346 to 1,038 of 1,097 - posts added to 589, comments to 510, connections to 333, answers to 184, followers and friends to 82 each, images to 81, plus products, videos, albums, projects, organisations, reviews, papers, games, tracks, pins, streams and repositories to smaller sets.
- **Major-platform additions:** Mastodon (posts, media, comments, followers, friends, pins, connections), Bluesky (comments, images, videos), Reddit (videos, images, albums), YouTube (products, comments), LinkedIn (posts, organisations, session sign-in verification), Pinterest (albums, reels, products), Patreon (videos, images, tracks, comments, connections), Hashnode (followers, friends, connections) and GitHub (friends).
- **Native load-more paging:** about 330 crawlers return cursor-paged results for load-more (up from about 70); the twitter platform key now runs a signed-in X crawler with profile details, posts, ad detection and login verification instead of OpenGraph-only; the platforms command flags 78 crawlers as profiling-capable (from 14); the manifests move from 0.6.5 to 0.6.21.
- **Google profile search:** a profile-search command runs up to four Google queries (two result pages each) in a background tab and returns only links on the requested platform's domains; on a Google captcha or consent page it brings the tab forward, waits up to 3 minutes and then returns to Orion.
- **LinkedIn session platform:** LinkedIn is offered for session capture, and verification reads the signed-in handle from LinkedIn after opening the feed with the stored session.
- **Sign-in windows in captures:** a tab or window opened from the capture window (for example a Google sign-in) is brought to the front, and focus returns to the capture window when it closes.
- **Autofill on ordinary sites:** a Passwords switch in the popup (off by default) enables Vault autofill on ordinary websites; sign-up password fields get the badge, and the drawer generates an 8 to 20 character password (respecting the field's length limit), saves it under the typed username and fills the form. Autofill never runs on a registered Orion site.
- **Per-site sign-in:** the extension keeps one sign-in token per Orion site, so switching sites keeps earlier sign-ins; the platform host, its https sibling subdomains and any localhost port are trusted without approval, crawl and session commands work for every approved site, and an unknown secure site sees "installed, signed out" with an Add site prompt.
- **Short links and brand domains:** crawl targets accept youtu.be, fb.com, fb.me, instagr.am, redd.it, pin.it, t.co, lnkd.in, vm.tiktok.com, threads.net, threads.com and t.me; the popup header shows the extension version.

### Developer tooling

- ./run.sh pull hard-resets the checkout and pulls trusted-main using the GITHUB token; ./run.sh extension -d builds the extension and opens a dedicated browser at the Orion app on localhost:4200 with the extension installed (extension build stays build-only).

### Removed (for reference)

- The interface/ layout moved (console backend, Angular client and rules docs to dev-client/, collector families to raw/collector/, still mounted under /app/interface/).
- Shared extension fallback modules (OpenGraph, XenForo, Discourse, generic-forum) in favour of per-site modules (the uCoz module remains for one crawler).
- Keyword content-type tagging (classifier.json keywords and the NLTK stemmer) from custom parsers and Telegram, so Telegram messages are typed only as text; the per-site topic-classifier cache for generic crawling; the unused page-bridge post command.
- **National-identifier and other entity types from extraction:** US SSN, passport, driver-licence, bank and ITIN; UK NHS and NINO; Indian Aadhaar, PAN, passport, voter and vehicle IDs; Australian ABN, ACN, Medicare and TFN; medical licences; XMPP addresses, AWS secrets, and event, facility and law entities.
- 11 leak-site scripts deleted (155 to 144, before the 20 added later brought the family to 164); 3 forum scripts archived; the markets.txt/markets.tsv/forums.txt build lists, the card-extractor fuzz scripts, social_seeds.txt, the extension CRAWLER_STATUS.md and the console scan_state.json removed.
- The extension popup's Crawlers activity panel and Orion-site dropdown; the capture page's own vault icon and menu; the local-storage backup and restore around captures and the storage reset on release.

---

## 12. New collector & extension scripts since 1.0.3.9

| Family | New scripts |
| --- | ---: |
| Leak | 46 |
| Market | 412 |
| News | 80 |
| Social/forum | 105 |
| Extension resource modules | 1,017 |

Family totals in the repository on November 21: leak 164, market 412, news 118 (64 news, 54 tracking), social 168 (134 forums, 4 forums in testing, 30 platform), defacement 29, exploit 14, api 5, malware 2, apt 1.

### Added scripts: leak (46)

<details>
<summary>Show all 46 leak scripts</summary>

```text
_3ev4metjirohtdpshsqlkrqcmxq6zu3d7obrdhglpy5jpbr7whmlfgqd.py
_4e3p3in2bl67hxchuwza7qvnpe7pyeloyztr5fnh257fxkovfhappjyd.py
_4k6plf4h2cm2nco6ae3inrsxnmqgl6lllmwefydhnlcq4tuhwbj4qpad.py
_5butbkrljkaorg5maepuca25oma7eiwo6a2rlhvkblb4v6mf3ki2ovid.py
_6hzpdc5pxt57jmjxyegwjyhcinr2z3ajucizndehykz7zfrtaptln5qd.py
_7t3zi3e7ki6iseun77ofqtr6wmbpgnpc2ada6gstcxp54lw6q2zb7jad.py
_7ukmkdtyxdkdivtjad57klqnd3kdsmq6tp45rrsxqnu76zzv3jvitlqd.py
_akiral2iz6a7qgd3ayp3l6yub7xx2uep76idk3u2kollpj5z3z636bad.py
_aoqinwwrizxbrwm3qb6l4jzgujgmcetp4jaug7fqylbf3xcwqwzvbuid.py
_black3gnkizshuynieigw6ejgpblb53mpasftzd6pydqpmq2vn2xf6yd.py
_bravoxxtrmqeeevhl7gdh2yzvlrjxajr66d33c7ozosrccx4cz7cepad.py
_cjfntkj5qeizxowuy3srceg7zo6namc3kfeor7pfn6bpdkl3w265ooid.py
_climaxf4srk4qzr5g7smfodx6q5ocpiwmls5qujfl7g3j33us4cksdad.py
_codice_ledger_io.py
_ctyfftrjgtwdjzlgqh4avbd35sqrs6tde4oyam2ufbjch6oqpqtkdtid.py
_destell6tk2la7w4d7d4apfzsywbt2lsdpui7i2rsfyrike7pxolrbyd.py
_direwolfcdkv5whaz2spehizdg22jsuf5aeje4asmetpbt6ri4jnd4qd.py
_ebhmkoohccl45qesdbvrjqtyro2hmhkmh6vkyfyjjzfllm3ix72aqaid.py
_emprdr4p7iwlhpky33tswt3k2qdeljyjcdpoysabudmmrz4z32laexad.py
_endzonezgz3sqzmtqg4acp4z7ao7xc6vunfhezjsnfqrm2ksudsredyd.py
_everstakewfma6sxdpshoq4ebtnqamsxnajrtzu2cflai77kfup5kzyd.py
_gammax6w3dkfdjfrjthtmqzsioue52vzy6lz54qy6jcvmogfpubutjyd.py
_genesis6ixpb5mcy4kudybtw5op2wqlrkocfogbnenz3c647ibqixiad.py
_helixr2sncrd3ndsz5oho6mzqw3x5u7mvox5zcsngc5wm7v4l5k7oryd.py
_hhevzhgcokx6xlzh4o4exuo2kryy6ectpb7trm5ut2g3u4trskptv7ad.py
_hptqq2o2qjva7lcaaq67w36jihzivkaitkexorauw7b2yul2z6zozpqd.py
_i62huw7ve22rpyw6lnq3kmfump2dmsg4xpveec3ere73njwatrz74gad.py
_iacjvmxjb2ivqkxxzmde4w6g53gn6ym7c3g6mfifvejyhqtp4wrkypyd.py
_iah6477wcvisc45wl54u524fbrh4dnmtcctpg2fpe5epf3jbpkiwu4qd.py
_ijzn3sicrcy7guixkzjkib4ukbiilwc3xhnmby4mcbccnsd7j2rekvqd.py
_incptbug3ziqym4rhgsj7h7hbpzt3v4ympcf42gsdb5haqq237fziaad.py
_j5o5y2feotmhvr7cbcp2j2ewayv5mn5zenl3joqwx67gtfchhezjznad.py
_kairosgeuzkzz3ajntqcyxl3ib36szz3mg62mb3zbui33pbk3uzo4qqd.py
_lockbitapt67g6rwzjbcxnww5efpg4qok6vpfeth7wx3okj52ks4wtad.py
_metacrpttdfpbm4qoxzcrqqgr6e6zafpazgxm72knmujw2mwvi34rwad.py
_mll5ddmdzgiq2siv3qnocmmqyiigfpajtc663xtf32qtp6weycyx2hyd.py
_neclc36yt4yaa5lv54kh4qbhvjcvuv6nnaurqowkellytpvj3afh4aid.py
_nongzecboljwv3yfndkggsybsglfrkffw7bvk2zemuteoxe6etpusnad.py
_o5lsqyar7ox25z734k6zaxt2vf7bsyi4q5rturi5iyxzqo3ica7bjsad.py
_shibaitobajtr6yctvrijfitnugfulkmprrqbmu2ysk3zyzx2ufe3yqd.py
_t33zoj4qwv455fog7qnb2azi5xcdxkixughmmduzbw2rtdgryqfbh6id.py
_uvm6hk4wwstfddja5z5htgtlehmfyflffijz6iozsuqyacyibzxefkqd.py
_vexytsr3chimdz6siwaqi2lvxxwfkxvffkpwyanr2llequ2hkm56jvqd.py
_whitearz6uvtfdefgsc4r3mzmyswtte65k5umcmv6cu6zhi4l5k4irad.py
_yhgphvfgaecacgtlbn67u7yfhafe6evnj4gs4s2oyb3hzjnlhq45azqd.py
_zyz6phauvraxghujxmecpwzlg34gtogybb4wzcmw6pvps5m2nzvsriid.py
```

</details>

### Added scripts: market (412)

<details>
<summary>Show all 412 market scripts</summary>

```text
_2ebgoo7w7dppzo5e6o3e5ioiby6nysjsbnqjtpgqs7jzr3enqfyamoqd.py
_2ih4qhbcgryixnmkjhxxmdn2nxqqbv25drffv34obzl6gqzkpeeh6pid.py
_2juhwbhepla2g34rt72ew6dbzni4t6csnhmd7s5ao56d4c7otb7xvfid.py
_2rrrawwmah2k5cgcadb43zqpw6m7wipirpzv4vvfc63qukdmj5yah3yd.py
_2x7hatz3mj37qp35vhrioszkosjylm7f67atoa7nmbs724w53jusc7ad.py
_2xng5ffokwvrwajyqi64nsbeuvmztcscx6vqoqghntlpdbdapiywsmyd.py
_2zjwjaepx465unk4x63uiabgyl2bzrcyctrcwuynwjdejgzlawtks4qd.py
_2zyxn2ywzzf57r3ntro7vgjuhlo5eeywcuc56jwz3rngasobkgkinrqd.py
_33npd62wscgifxlucpv4zt37xyadthgu3lweq5ooehlunveo4jxf7bad.py
_33z2xow6ipulu3vgc2nveeazfjq4y4gl4sotwbaesfnmupizsotam2yd.py
_3dalhbtxq6xsdl3w3ptrr2lu3btrzga6reovhc4rdam7njfuwcssyuad.py
_3gdkm7dk2s6orayra6bmblyj2boklhexeawsxinkcsc62mwdu5rcvyid.py
_3gq3v2kga2xunybot5kpo5fsv2ppgb5ubgft7go6hbqkgkdhs3okhmqd.py
_3ld7ttwf2yxioh3e4tialjgoonnmkc6cpasxb6mtb667juhiwztxwpqd.py
_3o4srgfaogeegko62scnatcwffrgvmdtyaswjfcbl433b4meak7jlbid.py
_3x3gvb5se3rol4azqxeqo2ekzlq3lngz3naacepj75lozoapuofbrjad.py
_46jdwy5n2ac35md6jcp5qmfw7sewldgwtlby45ubaduhqc7tcdv7qbyd.py
_4dyly2bb7sar7ebfigsu4fjmvnfxemsczkfiugmdigrx4xdi3gg4pqid.py
_4inqbcz4xdmde2x3yf4nxrxep2klyqzpg5prggpxvhowtxdpv27m7pqd.py
_4lxximyndvy4axbpukerjblqauvqmbj66knf46wufdm4kc36wsoculad.py
_4m2pasvwx5jux7heojpzrq6mlrzug5m7u23cs5hf4bswtgapwkdpcayd.py
_4ohwqvemntedxrshwwwz2eow5gfj2ow54udst72c7gzzmb4ui7d64vyd.py
_4p6i33oqj6wgvzgzczyqlueav3tz456rdu632xzyxbnhq4gpsriirtqd.py
_4priceuywvtntw54uw5wlte446xnz2uj2afk3vgldd6h3df67ua4riad.py
_4vjf2ae6lmthhwtxzbjlmfes4p34uohvqxcjd3v3hbgg7k3dlzbeiaad.py
_4wmicvgfju43ejudk2km3a7jhkwyewtgwwxbttcquca4fm3tilmtocyd.py
_52mfui76dnh2ffuvyd62rfigtqdsnhgcnfxnzohpux5js54yswxfrqyd.py
_53g2pdjqle55fhtwhzyt52yxcrfph45jubwlhb3znkqy4ul44zeqsnad.py
_55niksbd22qqaedkw36qw4cpofmbxdtbwonxam7ov2ga62zqbhgty3yd.py
_573ic6qov7odiybhuujwbkd2jo3wcceimktbttpsqxf3dlgjkwpgz6id.py
_577cbowntu2heyqirub5fyz7x6jj6owday3zkgfrq5zdsgvb4keswoad.py
_5deqln3agajaijgpjygglt7maajaoelw65ond2gzu5jetor73ztplbqd.py
_5h42qjldohrukp4o3pj3xnsqrk7mlznk6xyxxlwklvipwq7zlvx2aiid.py
_5toexthcap76m4jnedi2kq5lup45edprn7jr4edwcvpbzqecqiqnxeid.py
_5ymiapatkaeuq64h74jqjwqec3aozr4dxtebr2gyf2gbwkzqbuwj3bqd.py
_64ittpm2unuhcr2jyp6whywjipoz7y5usrudoqpvarsf6shk2p4yagqd.py
_6bxsvoei7et4apa6up5gajytzxom7dcmynpa4ltmryfwmuzn63jwhyyd.py
_6bz4x7gm3xswqoye3h2llweg4ce3aqg6acufzd4las3tbyeswmbdmlyd.py
_6f254bqulzeajexemolkpt4wofeebuzxmwhc5hjoq7io3j32cadoejad.py
_6hqdzbjehwq5gw6v3h6ifkgcgxlmgcw52lieunsmanmwformtuzyosid.py
_6izhcgnbnqzdub3mbx7t452jag2f2ynzdtw2nx7cfi4hjfscxvqxx6ad.py
_6jqdrz4qht3xwckqojtk663n4djlor43fj2iez3kg6iot2oztzzp2eid.py
_7777777qxnyosasomx7iockjxxtbkfe2qhnandq33jbjopwg5foxz7id.py
_7dkribx4ovp6khecy2ji3hmutarr5n7ppsrp5faag3jsqqwn4rwqlnyd.py
_7fe7sxfsmvhtyo7ca2llcd3zbboohmksnmca34dasqaouw2wc6ry72yd.py
_7guxw3regwohcpi46nxg3t5vqq4xpcjjcjswrautfayyesahxttiutad.py
_7oxh4f32vltd2xui5uio444kkvpkfc4jqp3gdg64qnciyjqsbhjcnnqd.py
_7pbwrhfxziwrsngxgfjumfnzdkowmf7osc5y5fswckwdkgx6fdrbfxyd.py
_7qz7ko425rg4o35pqgopigik225njzp3orei2hjbsd5ovxrz3ztat7id.py
_7ytp7qugur5afljyuxajit4r54i3xcktrkww4d6fj5pu2c2nnwumsjyd.py
_7zyybzdh5k4iefkfr7g7lrg3q5ehqm2yekplzngkpfwaq4fn4whoanqd.py
_abaqyx7cmlxqz3steusezkuzqamchsmh24ofzieyozx4i2wx3zdd52yd.py
_abyssou4y5ang24dn4cfkrwd3fsiczx7xg7n7mzlcthrpktfcar6j2qd.py
_accsmarket_com.py
_akpvzosh3wg4d3pktfmq7k6shdzuprfd243ofab2uy55b3wqojle6xqd.py
_amazonev4p77ihfmhuraexoc32glagy6q2s7cyturlttlxp5woo2kiad.py
_amazontg73dwdliigg4735esuhu2vga6bovipo7rjowmzgpjo63hnkad.py
_anon6vvnaieowcsqqwjvsy62ngbbni5dnf6zho7uvyjmytshizc74jid.py
_applbwku7dfadkfkumiojsbxekuiafpr44idl7bxb2xll6neykvx35id.py
_arsenal6vlbdak4hz4l2ooa7643omduu6prgkfe5hjm3y5xtnjetrqad.py
_arubaaq4rmcljbg7iqopfjxdupzmxda6jd4zwrz2kkhc2tv6rt633xad.py
_aunzkm5xfvjjfintizd5xg6nktuaryawunhyl4hzrrdkkd4pkizvthid.py
_b27qnaogxvibcd4ly5lrmuj7iqeq6rfkyvci3pxunvsd52teyjqucuad.py
_backpvulsstosyfdno6vpfn3mc3qvlsmozowlerj4s3jrqvg5oy7eoqd.py
_bankor5nng3x5yg2n5xg6ilu2mkk4abnojpzzvbmzhinkhr5p3fl6wyd.py
_bazaarhzchqatsfqc3dbnk5wg5ziqpsztmg57jzzydxt2lbjrts4mtad.py
_bepig5bcjdhtlwpgeh3w42hffftcqmg7b77vzu7ponty52kiey5ec4ad.py
_bhabhcp62efwn6akso6ttwlzl4kjwe3zgmyzvoupjgrucomw3sbbzlad.py
_binancewjsl2mir7n6zi6i6lrhyuduhjgtdvulb7lixmuqlrt3mxj4qd.py
_bitssjhmn5xanfdp65c4kv25bkyjdb6mwru7eepwhtwwfg3nmvolinqd.py
_blackma333zetynnrblc7uidfp2tewhtwpojxxvmty3n4cdsc7iyukad.py
_blackvtx2zw4m2irfe64szu4jpgvjnhxjuagx7ylr3pnm5dzwyhnhwqd.py
_bluewttxynlisunhmyogs7ucw6xjuedvrsviwswm5kdywre4u3dyijad.py
_bmarketjfejd7xrvfvisx22i2w7tjhbr3fbh4ggv4ycaijodcda6aqyd.py
_bmgunsyop5qa34nzrayd6shsovsukwbbscyo2hbu3ri7b2ghw6sjgrad.py
_bmvpr6tv6matswemmccr64avw4nkzckn5oer5pdltyohrf263ahf7eyd.py
_boom22iudr56znmpalhjrpaqyzlvr3dsbb4ylelz3lj4f4ce3mjjrjyd.py
_bqdg6vzg2teohu6oxj6fxpq24ieg2wzy7l6wakcfgzdhluq7upgm5cqd.py
_buyccvr3y72futr5noz7h5obrkpfgk6mgo4h2rdm3oif5pq75ucgtpyd.py
_bvzkxwt5lyyhoesvorikzmzee7gm2zpxifqjn377yip5jgq7dmohnbid.py
_c7nipjkv64llmeu2alhr7igvoq4gxb5rixebo5jlfvoiadztiqbwuayd.py
_c7zpm2ar3lgjglaosrt2er3kzrpxqbd5bui5vyme5wosbut22veyrpqd.py
_cacgitdiv742ztcc5ghwhx63aksmtbbwmjzcs33ectlvdrn6x4xuhmid.py
_caliceks2h3gjbpxl65uit4yiiom4x3wi3iosy5twrjiiyqbxcc3hhqd.py
_canna6n6umk5rt3bex23u6vxjbgbyqtvrwuujpozglksz6nmtkumv7yd.py
_cardbox54goxehw6epefe73g52ejfxftwebu527jddhhymjxccytarad.py
_cardht5mmdumnsnmj2jegwyrct2mzxlnazqwk4wtnzjezfq7bwijb6yd.py
_cardpl74ltmwe4o7pgpefljcng6qr36cnn7gzer2wermedxz3volxkqd.py
_cardsfo67ojoecng2ap4n6d2gyenweo7rnr4jysmcw5ncb62xjxviryd.py
_cardzonet7uuuhlupk56qsaf5omwhv44bxv5ushyvdv3u5qakjqpzvyd.py
_cascanbosjlqkmdskhnido7athd3autb6flbyxs37rh5axznwo2jzyyd.py
_cash2tuohd4juo2l6us2ya64qbjtavwtmydb7txbraq7m6qs36dc5tqd.py
_cashmyut73nq4ialwhr7sgf6kvznt3d4ocogu677c3e7gy4eoro5fvid.py
_cbpy6unzuxtbfs55sarvkenzf377bpbnefriyqp673vri7a4uizkl7qd.py
_cc365rfnwxcmeizmlxo2unibbodwqykjrxvaawjj6ofs5clondovalid.py
_ccshop5nujyb4p5eeonlsszhfaktd2kyhhebyupcz4xchjtkiviyusid.py
_cdumhlyjn3q6rzed5jtqzramrtznaahics5d2zhpspstw5stfkew3qqd.py
_chanelkk337yqaou4zrutxprrodvu6t7pburehryosyzczxmk4yzzsad.py
_cheapxkogk52atsqv72e47grn4nwssroy5ttfmrqd4wqmvweqruhkyid.py
_chemi5wtn2hs27wlwgaosi663wswrutofqzhvrjb2ogtxpb42gezebqd.py
_ckyiussv2bqbnnytoo7oqgxnx6z3gwc2rsa6t7zu6ra4epg4yhepkxad.py
_clonedvonrml2yz3m2xw67mafyp42qg6ob7obah536gmzmxk232vufqd.py
_coca3iiqflhspev423jybof6zaew7huaz6cmbqhiv443w4evlkulzxqd.py
_coyvh6travj7hqflscgilf6xvj2shdl5mhjpb2bpc3tb7l4j7kjl5aad.py
_cqzawspabusgicyd67d7oiucovw5rkuwuswissur64s4oxng6aavybyd.py
_csalryx3xenotyljyttsju6jfthrjyt6ijwd3zzykhkpyfoeao2nxaqd.py
_cwfo7cl77qkgo6nes4ei3lngee6s553ypmzc5rjm3qui3ojk43v46tad.py
_cx2o2vkuqlqudwgw3zftpserxjmztykiclmd5lj37byt7cmikmjaocqd.py
_cyllekiwqe3hu33wv2s5t2jj5ikjrpk3hpvyhqs7wf5yesipmy6ftnad.py
_d3c4o2mdjvh7w7v3bcn6zqhdnzm3v3bvslfd3pydt4whomwz774qpbid.py
_darkbv4acvt4dqfjphnlbjmq3ivvwsgpzvmuza57vcqzi2zhskt36oad.py
_darkhuo35yiohrsdlbdhhrtl5ds7jtv5x2cjusqqebalj5vtrdqgqoid.py
_darklmmmfuonklpy6s3tmvk5mrcdi7iapaw6eka45esmoryiiuug6aid.py
_darkmalgjflxvxrxdyfyjo6w4xrni6rekh3dofiyunserojlgbwkteqd.py
_darkmaptneigjb75d2pjnesct2iq7irnabyqfzffoj73bf36s2oyfayd.py
_darkmatidmfjxecldy5kf5as6fowfqh37l2q5plx4vasvichyahypcqd.py
_darkmonn6oy55o7kgmwr4jny2gi2zj6hyalzcjgl444dvpalannl5jid.py
_darkpb3w7h5mmuzsdkiurlfbzxi3snof6cmlmlkvxwjgt3ufmrd4sgid.py
_deba7hcel5t4nodxhyik2oamkf3bl3gyvae7kq3xtxs3fnw2jvl3doyd.py
_deepmarkbls2omnis622yecjmkluozl2rcwhzjkzdc4cxnzmhkhrsiyd.py
_deepmarn4jdhginkt2srb77hgktyomdrmmdeyxlmvdixr4dxn4mlszid.py
_deepmehruhlkmelzffkpcxathectjtnanzkgk7xw7ibuj6qieubpecqd.py
_dhgbcfvetdnhsb5kmbgxrr7vgialtmdoqyhbzzo7oqs7awxvw4sxb7ad.py
_di5nrcknelmiu4yhyce5dnnjib2otx3m5djhkys72cmgr5nqimq3rlyd.py
_dickvoz3shmr7f4ose43lwrkgljcrvdxy25f4eclk7wl3nls5p5i4nyd.py
_djco4g3kgv6y5fsfhciepomlbzl22ol5ew3zewyrbc2iek6t76ffmvqd.py
_dlaqt5del472zdkawrvibbn7ryir2n36krsmrlxdxharesrtiejshyad.py
_dmnxjl4qefpjngiqlsflh7gfuv6glydk23nbzpbcvxloe4soopcgkkid.py
_dnstorebfxgxr4rwrttbir7e4wqhsaup6apbmhgwn2eqqrj5sxkfd2id.py
_dollars3cd7yx375s7guarfmohtkz4z7h7nik4cpkjsjnat6thxzxgad.py
_dozrkm62j2uysnqg57q35cangl2lpgdirhxbcc2yzpcgvfyowy7syxqd.py
_drdj7jgn5nrnp4ebxgi36dy55uamqyow2tghbzinqmbazkkukb4lcaqd.py
_dreamqfus25lc7dwlnyrxrh46g3dasovbayv5jqhdykfa2az7nff7had.py
_dreamr6y7e7ilemgws2ccuegvwdge3sfdm2ijh4s4hc2eggz3cvgl2ad.py
_druglabe6gqdbiyxxeuwqwwakxmzbim75topprzq2ro3zklstqh3k2qd.py
_drugsxsn3vyx6p2bon5x5iumk3pms42dsf3gkeg7a24gv4ijuunbylad.py
_dumpsp6qpyi5ddcgcbsrn6ohoceuyody5pjghcgq2kam27d2bqd2znyd.py
_dusk5vko2rq4np5kxf4d7jwlsd2phoa2jdmq7cy2oupzcov3dsbl32qd.py
_e44trjrqlymvn7ifn77czuo3ksbmkm6257p3a4qgxkhp7fexh6h7idyd.py
_e6753birddv4kxbter4w27rtk27mjhtbpm3tikmlpfjem6agpatjwyqd.py
_e6otl3euuup2y2olm6dcrtpzbhruewxlia4dfxxpmpmk64ykgdhbnfqd.py
_eizwslfbimiqjiwck6s3sac37soq64fgcpxkwsdkujw5ogvwbvoz5oid.py
_emarketzhfjehkqd53lnjgmtvbf7tseydias4rjpazwifiqcotoi2gad.py
_endtovmbc5vokdpnxrhajcwgkfbkfz4wbyhbj6ueisai4prtvencheyd.py
_eumlu5qg7ff6fcpvw76canigq52kqrt2vi7nkhyz4aa5vceij6b2toqd.py
_eurolnfwsx5g7ilq7scjgnuizojmbgpr4eerry5ruglzjqqqxytzdvyd.py
_euroshopdks77zis7mkkdfqqlnejbhgza3hromndy3eyz2oa5ixhjgqd.py
_ez37hmhem2gh3ixctfeaqn7kylal2vyjqsedkzhu4ebkcgikrigr5gid.py
_f5a7q22hqs2hguqlv4rgj5pkm5pjhvu5rkboi6ejkr7cr5x2plz2jmad.py
_f6wqhy6ii7metm45m4mg6yg76yytik5kxe6h7sestyvm6gnlcw3n4qad.py
_factory3grnrr2ulgxw6js26n324vqkzlhblpttuqq3aw73zn6arhuqd.py
_fast2cfkxyyzozcxh5jvogu7fclilq5oglagtr7ru4yvuzskwviv65qd.py
_fef2acij7voshmat7c7ivd4zvoshfh5fqipk3xaugbs77dlyaqloscid.py
_fgirl5c6x2nc5kjtohua6ud7r3ta23fkqstfajnltoeidcpzuto4tfad.py
_fgun35p2eb7zrrdmyovrvud42ypifhltq5uhtzfzoxilveadgj2znmqd.py
_fiebrlrcfrram25fpxmvea2bz76sde3kkcstsjnwvdphm26goq373eid.py
_fktbw3ezeiyqijt7hudtqqhe6tevuri4kvlj4arconcoopwjditndpad.py
_forum7fdahiyqj4rrlwc3v7lhmxym6b4qlzyx4go6j5v5suqabwpjbad.py
_freshtools_net.py
_fsikub7scrh5ntzbnt3drmpl6urjgga4rtoiuzrbw7jnipx3krp3p5yd.py
_fullzcx37u3xcbngkgjqdldri4clckayfccs4ck5mzssvnbnfh2haeqd.py
_fvlldjogvixbzymmauk3sf7huxq5yz4vkpvn4istsjecotle6rtxzzqd.py
_fxfhynjekz4h5a4l2qq2ihbao3kywzab5muei2ffwhii3sp44oyzefyd.py
_g3mjl4vxaui2hvg4wp43tnkvl46j45zeaggct7l5sejv3mbrsxjfs4yd.py
_galaxy4ogmifzf2wwdkbedqeps7pzxa53bkluyszq6sqng4u5g5nixid.py
_gbti4rcwa6tq5lmmc7h6esgmuouobi6e6vs2d74sze5mrsq6u6y7tqid.py
_gbxubrsazxwku6mvu4oz44kyqa7uaziw5mmaxe6a4x624eibx6o7srqd.py
_geirovxoihey3elhsdczxencl2rzdetwfq64p6hczh25zuwmdx2e43id.py
_gifthub7u6pljb525i5z6ucs55wmwby7wkefiu75akctcwtopw2vkayd.py
_gjst6artnauq5xjprpx7fujwlpfhi7sxwniexqucpl5xbnngpa5fvmyd.py
_glusd4vexakozhsfc7qrsvrjaeusvv23yumomqp6qpf3rtvhc7maadad.py
_goldentgwgtrmxxfopbfxwl7enamd2zvray7jl3p46l6igrdpsbs27qd.py
_goodflrx7a4cromfjlocibgmrkveubozwz3tqtbshcnusnkppzgvmvad.py
_goodmfzqgh7u6wimc3vliulwx33zjvy6vr475vahj7r5i7224efolqid.py
_goodnj7xd3c2mxfx5bzu4kyvl5ff5nl5oawt5mjdrdkavdpaybpv6zid.py
_gooduz3xjwjow5n2hy6plv5d4zs4todhkytdbk4o5vbk7ntacihoduad.py
_goodvv2wiuyzid6dlawe7mhdy56t7j5ucte52r4odsy3kh6bs3fqp7qd.py
_goodxijllxrs77ppqxjh3b5qmqmuegneq5nncg2wdh62zmauvjrc45qd.py
_gruyszkchww4vaiujpd6omwiloruulgcwuzknfsi3zv7mbnd3sxlqvid.py
_gunshopocstwl3v3yoo7dn7aueodw6dnvfaqh7hbvnopumvlphfrcgyd.py
_gunshopuciykxsq2wrya3vvzfsbu56jq5ryuk2tu6m34zrrndpxktpid.py
_gutodvzf4bsqfgrgomzym42onleuj2ahzlkxnctcig3kptertx5vsiyd.py
_guxej24ogppwjoyrjyw3otvvadq6fq2afyymboplnqkxtfo6ufu2s5ad.py
_gwtwjkwofx75zerzvatdmw7zpsjozrddioackoq5gp65vz6jecgoovad.py
_h2ni4t22qasdz7dkuogwk6u2aacqealhmxloob3f4m3wcdxyc5ba6bqd.py
_h5jauggmexwnkvyh5yrususcmfposo4e7xo5d62ywcupk66dayer35qd.py
_hackrsrqb6qbdwihzcsus66tunvfkadyktuz5t6dg5v5h3z6beqfxdyd.py
_hahzcidhjx4b5nszjnaiizgxdfua5ypkkz22siqy22swrzbgpco6uiqd.py
_hbkkqz7f453cmtdldln67aglxqnmehvd5hckyojxjrghd2vuekrogrid.py
_hcards3denisr4c7wkbu4x2izwi6aa6xnkqsxkf6nph2bvjfiisjrsad.py
_heofal34cihiypk24iw7n6wutymmxhy4rz66thilcyf7as77vtunf2qd.py
_hkd2iwsubkdbowphg5i3fl7sj23mgsxf2edmrlq6si4npn3yuoh3kpid.py
_hpcphv5q3lhiebh6c765lslihne4tn6ui5dnd636eo2ic3ytv7c72qid.py
_hsf7ke3wxw7ev2egftwgyny6g3zlvrip7unsdndizwshsk5yhommgpqd.py
_hvcji6rfphysp6zoqbnnblb2y3yu53gi5butdgl55vc7i3kfuohqkyad.py
_i3b7id42chymsd62ew2oz6cdjatckr4u4544iwiqw4atj2p4y34dm7id.py
_infernosazlv4c77lzj5k7txev7iwvm53psn4crknun3qikppegc6ead.py
_intelrepository_com.py
_iphonexc4sii4jlsahsphnhit5wq7agpizeuucpac5gxkd3vm3lbg7yd.py
_ipqqmuukam6ngwydaqnoy6zutxrxnwnhtbeqdwxfi4e7dr7hurt5okqd.py
_ixcb67tpwxg6ogfynhpmwklfwd6ydqvhilxxa6sp4xg6y7ut4tb5u3yd.py
_j3b62fifdmgbiiivc2ffci6cu7fjvxdo3z4jtwrghtyazixyizsqz7ad.py
_jaat7efxnopgxe7ypxzqpb46qmbaca36r7shexsowofexbfqp6p7xaad.py
_jbogssc4zg2jaj54noe3setttrixai2piuk7pxzz65e3gt25znnnllad.py
_jg65p7rsaxtmishcx4eltcilsafwloemnbhcxkmj5mamsxwca5uzliyd.py
_jiauokq6i3tmngzepczazs7mgbskkzhtv2lodk6otebq6ytzgkclzhad.py
_jlljuam2ulqpbx3f2dp5sbruoikka3gnz3ruhjsrhccn7yedp6zqsrqd.py
_jlrhfq2tm2ocilttfafm6co4emoau6lhmw7rfynfsiyhjasuanonn2qd.py
_jokerj7ftkcpam6vqcm44bsvrtulhfhwownyy7bdxbiuiojiqnejwcyd.py
_jsolkayfqs4kskytrbfc22y4o5z5xyftm7yj7oupcuibp5tckxomurad.py
_juougjts62vqoa33ehdsulnvpmoxbp5t3o5eidxnwy6hg4zuesdorvid.py
_k3emqmv777di43fwxt735hvh3qhltis2uyrl66of6ar5oughzcqbsjad.py
_k6zcukplcko7wmstklbiig6augfxpun7e6txjqvhmcrcghreknhk3qyd.py
_kcz2hwttj5o4hjxy46fk4alk3w6upureycqd4wctyte43qzy56l4dnqd.py
_kemldgom3jujt32isp5nra5kl6b47xkd65o5axp5x7ulqj4x4eqnz2ad.py
_khyi2562jsgqahmuaxov6736onfply3yg3gskjaimmckqwmxcmxwk3qd.py
_kidmxjr7ebf5teyluh6npigmkamn3ykupm4u25bys5bkgu2ritzpnmqd.py
_km67fgiq5v6epeift2ywbebuu42p7dog7rtahx3fohz5ewxlrcd4cuyd.py
_kon43lsxbw57sk3gvenwtuqxe5acemrlw7j7vqtiob45wfi3i6wcfsid.py
_kq5gmlbnojgcqzxf6i2guvwi3xkyzy3gjbww3daugddfinxkftcvpfyd.py
_kuiperoux5jg4q6me5owly6r4aord2msjrynv4sotrg4dz22gy37ueqd.py
_kw4zlnfhxje7top26u57iosg55i7dzuljjcyswo2clgc3mdliviswwyd.py
_kwa7j7bdbo3eqfhmbggla5vnj2iu37n7b54pvejnscyrsveb5zt7l2yd.py
_kxyxzejb35dt6yaqt4vkxzgztav5bcb7q6bbpjlf26epa22ft3huokqd.py
_lab4llqv7w3a6ir63ny72zpnoena3gbquytng65zfnjzaavzuudc4eid.py
_leakfindrg5s2zcwwdmxlvz6oefz6hdwlkckh4eir4huqcpjsefxkead.py
_legithfwhggsa7f4n5gjenkktsbtv5jnex3pzqlt32hxnrugx4ssliqd.py
_legitv6lixtir6j67povearmupo7luhcf5tbk674ekdha2ebbecatcqd.py
_lfdy3huo44w4xsz4tahl2clspag2sq36rqyvif3vwkbndmokv22o57yd.py
_lk2id6jvmpwoqkuzrtge3tizr6prvha2zo7gbfnbgtp35vy2xm5pbxid.py
_lkbp4mwcptpce6ywqqxfcpyxnevgrap7m5c53jbn6a6jzxt5quwja2ad.py
_llmw4onj4zpupbrbd4nibs6hwpcighu64vcpjytktwgycd3hyvodnqid.py
_ln4mb5gvoqgjnaicree57ofxibofn2ea2cslsejvhyvvh7dpyj2hzdad.py
_lonelyyfzgs4k2cd4in2ggbzjncfy6eimwctbfjxaynxckw5chbuyyid.py
_luckp47htmgn3vm3wa3zw7hhowu6cb74grtbwiwxovjhnzkyui5qjpid.py
_lvu3jcztjlyblymfb75xudme24rbldjxu4jnyaur7pnm2ea474qxl3id.py
_m2zxa6omywrl2y5wzq3desqty53wgygxlrypicyceqonqo32pxhjswqd.py
_magbo_shop.py
_marketcq4qfjw4huiee5uc7cqqaibkh7ljhuvzui3rwjg6gozgko2uad.py
_marketoro75whw5on6zjpodurbsxbzhovx54j2phj7ij3nxiawxwzsad.py
_mcityef3eueeh26mo2e7jn6yypgnvtbu2w57kcka6g3zu7u4xv5cgkid.py
_mdzuovzic42i2lxkp6qu6hcchxgxylctbetfe6p7czjbo37ygzc35hyd.py
_mnz6y32bx5famt6lcbetmxosclrluykco75x2zlpypwgiz3diraahgid.py
_mrrskzifv2thomkkjcmyyh375gzt3pnidbpexdapdma2yu5jxvme5yad.py
_mxraaml2ybke5g5q2gwhubtvg5pgbldjyecpbp4s5nfus4ssurdux2yd.py
_n35xpvuxkwunxi4hovvjsobjtchne7f6csalmenom2mcsdj2dcuhevad.py
_narcosaamk7z46l4jfm5nq2f4fu3ky2krhp2bbl5wes3mtmeztx565ad.py
_nifmlapngcgownpngqiiryqb4rzefydzridgpnffsxixtumnkvsl7bqd.py
_nk3ulll4bfi2enhxhug5d7aszh2aimi7srjxts5nkkag5yz2nhtgziad.py
_nk5zqlbd7mopzvko2suyvgp2gmz5srkwyjkpnxbksy5x7vrnqikqerqd.py
_nljlakef4vxhlwikc5d4izugyqe2kvfyykmjc42k3clspg543jrlppid.py
_nmfx52est3mt42aza3z5v6ywqi52tllgvyv4atodb46rkc3xb2cumyyd.py
_nodabtczwxa3i47esdtrcaacubfy7xiv7kfzr2t4k7bf6byb444ifrad.py
_nok73efeq6htt4f3cm5sxj72u7h2rrop4c54ykhnglkr7twur5kc2mid.py
_o3zq2ipet2qpeajysjkbruvxntozjpssvovzgs63lir3grj6xswh25qd.py
_o5qulp2zod4cx2h3h67qahdy2f62bw3qhgma6dtrqcz4c4zztew4spad.py
_ocg5grqfsmh37w67g4godfdyuokgkkeana725i4aljhfmjfer2v4ftid.py
_ofl7wgynqgmfabva5jvamccmbntltexqr6x5y5p4pgvovrpv3diblsqd.py
_ofvdpld3dkz3lthzyjd5wuobdiji4gocslfaws2uxdjlpyf4zgg4hcyd.py
_ok3jewlqm4pwskskcupntze76ytgjuwsvtvfm44ztvf4mwb6chmiciqd.py
_orvx_pw.py
_osyxumky4evsotreib6qlos2zghq3nslh2ndrx6f7yban6v6z67llvqd.py
_otoxw54gzuig7ioszml6czfmqrjmie34anrcusqmkv3xdnnujncjurqd.py
_ovfmfla7kjrevuw5y7vashdmdbtgnrvjhz5xhluxi4hnmoavd4sxriqd.py
_ovygwcckgedc3mtg3qipa6mknwl6su6fgmbp327x574jmq3z5oj6tbad.py
_oxkn2z6rqalwif4bp7ejdvmme6f25pic72wvfxcnsvsnq2rjcbhbwxyd.py
_oyuc2ax7hglng674ftoplzdnjdweer3k2sndnj4632croxr2o4ybd3qd.py
_ozygjmg3v3senbe2gb4fd6twggd3kx4etqzllnpapzn73rokzukgxrid.py
_patronc353gi6rblcu6pwukcsnb24sjaveaaavhf2o5gdk73vlzctjyd.py
_paypalx3ptnurkntd4tvjxxjiz5mgnpcdlz2dwoz5o6eke4q4o2xhzqd.py
_pcfbdjz5q3fn6phagcmtiqmen2ayjopzx6da56u3mjb37zfk27iaajid.py
_pdkefplyysnye6ujytbreuixcrvvwrel5ycmeqmgz3vmtebsg6bup5id.py
_pexfqpsg2lvz2oh7oezmavhj236gqkpmz4zgvbod2xupyps65kit4ead.py
_pibgb4abspkjbiutulmyw3xmtc2wnuawyr4nt6y452d46m3etoef2cid.py
_plastic4xuqjt52uwbmxsbyrwor3garq4t5kvpes4i6hfmfyufkf54qd.py
_plugabpoixgz7yqs5feimhlklrwzbsxi2kvmvel4korzgmbqs23mb2qd.py
_pmmttbtq26z2ho4immcmxblrdi37t5dg5jamnc5domdq3l6iblo4moyd.py
_porf65zpwy2yo4sjvynrl4eylj27ibrmo5s2bozrhffie63c7cxqawid.py
_ppcentvcgdioqya5cb3ctcddvpny4ntuxmnrm5wl6kyjjqqkaamfmmid.py
_ppexperqmxugiobyobgzsoacktskplz44fcrfaznciv2xgdotrdcv2qd.py
_ppppps4kxvomg6xqifwip6mzcjt6zwbxdub4ac44whngr4hvusdqnoqd.py
_preplyqxgvapegf44xvv3c2kgccyi7enmcbqpck63j54c6av4fj2e2qd.py
_primelne6lil3t745xvfumcuwfo6wzzsoxzjiyo3juvgu6u32dgyc5ad.py
_pv3fe4ohyqixjkprespx5vvbnthnxdm5pm4cftl5ssssekuc4kptsjqd.py
_pxbzggitxizmnd5rttrzttg7n6dzcns52aykn2z2xbs3tjku4r4rt3ad.py
_pythongw3ffuucukjwk46nxnfjddxxa2dp2ko3tgyu65n3ooh5mx66ad.py
_qavhtpfs6fao6bi76vvuf2hp76y55xkxsig53nawj3w3ik5fnjdlu4id.py
_qh6xf4yc4fpsh4b5u3kd5y4bvdamaepmwwhuursvp4zk6texvgbmfuid.py
_qkfzkcbdvs4v23rsicc5zna5izo2nee2dbm2rktyu2fq7j4irn4ygfyd.py
_qqwtcqsvclhs7demkk6vlrtvhsp7yxcxwtd7xo7klmnbarvggfr6cbid.py
_qrsqjeuxmtpckhgek4xdsdm5pjiy4ppyrlr4ap6j6o3uhilxgkeygsyd.py
_qud5sn5juubzrvjdl2s5hxi3hmf32saw75njui5556srh223rkq2zwyd.py
_qzt7gn3n4wd57bnconl7zwlwc2ijzt7ff5d7jvxqydjzb4umtuxsqfyd.py
_r4hyxmieadsyhnqzccmib45qtwa3x74gpnp24ovicuiuc5jzj3jxj2ad.py
_r5cyorufgflx3sjqnqf4vgk2s3srj3esrqhwey7p5j4rmh67jtmvroid.py
_rbcxodz4socx3rupvmhan2d7pvik4dpqmf4kexz6acyxbucf36a6ggid.py
_real6hknkxbsv5lvwboqx4vj3gfqpfxmsbiuxhmmwdlrgz5nt4h3adid.py
_realayw36almkcejawo5hg3rsgwdlkqf2nlslxzbpl7vitbxnemsu3qd.py
_realdkt273i2dlpin76mzqk7wrl2gqydm7zub7vcp4besenntwecekqd.py
_realfzugpzzugjqypb4s4ctvchzdij7c4zmxbcu3afhldwjgg76wjmqd.py
_realnuiy7ndjzadaibeppcb3w62tpfcxd4zd5rfq5trehei4ulzfpoid.py
_realon2n3sru6qao5czkihat4bwdy3zvhiymobwsz2gxdbuoougssqad.py
_realrehfsv5h6qyz2ifyiw2noyfixiqlkeek57dayotvcqxb3g6lf5qd.py
_realsk3yya43jr6qbaeygfwcppybp5yffsjiph5cbtdsmo5h5precmqd.py
_rqdhydflzx727okymzhqpjl7s7rr3i67mc2i2sq4adgcftbqs74nz6id.py
_rqp3to76xsmcgjdokmqpc3kfswl4xxflhuovejqnpxgxfgnmbeciaeid.py
_rsh6u6qzzgscatg535t25ms7b6stlzdn7seeofqlmuwrkjcfsf3hrhid.py
_rxfpdzhcfmjxvkohawzisnvojy36gxustnznmwd7miwno54fyh2bw2qd.py
_s4ke67pow6gajinapu546mtkdvdl6i5ybc5p33aufg2owjtklfo5naqd.py
_s6lzhunjni5nx5obpoj2zn4645oja2hinjv73krf6dmg7dhh2r3ijxad.py
_sant2zsy3xlzdplhh2nhiiiqmslmmtnjxodl674agytzyrpvxckow7yd.py
_scplsya3b6dqiwil5y64xokn3qp4sakpztypmdpprdrrvnmi5wm6cvad.py
_sermona2lpmg6xcof7lx4fejk4bky776k6wlucgbgeqcd2i3dgbpobqd.py
_servomekr5jztbl7qe2dnpwlpsopbu6qzgg4gaq3pk4qlgdek4hnhdad.py
_shadowm4vpk2xffxqx5ngu5ckrq63oraoyb6hs7w23ypzckmgnqyeaad.py
_sharksp64elqkesxjgnwd73qqje3mpvhploeunhpyo6g745mgrayl2qd.py
_shop6bpatu4w6ku654kd3oguyvauxxtzt7ae6d4qqyksq37igzcf2jid.py
_silkr4lawr2zanmaqfbcpdtmybjmk2m6lu3iv5iw7jyivyz2cgt2xgid.py
_sktgdik74va3k2gvfkedxhvxgqy2h7dttozghkt7sdzkz2g7uke2piqd.py
_sl5fd6v6rb7mkvr3y4ind4a5od6lebs4recs6azarracma27pdxg4cqd.py
_slorebzyk76dhafdwvzdnqmql2h5vwxc23dt5lx4mlzxs2kv3uteliad.py
_so2tzxpqctbthsucjkw5szttm7cdflimfkbis7x6z25teiqnrnqzm4id.py
_so4o5ikku6y2vrihersn6ikz4f24unro6hlrke432ral5d2fb4d2jeyd.py
_sshxf74ha2vtzaxfdt7pxgaqerrrgqyzznnrcgyax5e7rphciljdgbyd.py
_sxkz3sf36hszsxzd2tzyzpdfzuccoqsse5waihx2vfmci2sahtpufwid.py
_tdoobwweuy365b5c3v42kvcdzj63bjhld5krjgxrqsqnyx56c2ss4mad.py
_thebankicntkxjghfomo7r75adyansy4slfsjmw2jq2czqqk77feeyyd.py
_themarkedjsty2dvgnrrxl76vwwta2ccygmgj35jdf3whyd3l5akybad.py
_thorlww2o5davptrfbgjyddijsea6q3biy3lnsq2imfagfbjyhwqapqd.py
_timeahjgdnis35equjvdh4ibpydll4sxrdfeslprskila5pmfductpad.py
_timeaiohfebuimkcvei67j6udgilmshtuqtoc5dwyfpuawj6hnh6yeid.py
_timeakw5x3bsi3wpkfqwnmzpay3wqcmaa5i4adhvdmh2nqzxlhgttcad.py
_timeapy77rkuyawjntf5kkupmlo5abco4rhua5qrdv7izx2tqzp4tfyd.py
_timeasp3d2c3bfsvy3yaxyetwe7a2brpppxyphvkslaoisy7cqyv65qd.py
_timeatxzf7wa5riffs6ufuoisl7pwifbstpmmiqkjhngb2nx6odeqtqd.py
_timeautu3epniwm7fgi2tidakg4a7dmjuv26bvhvzhmtlihi6wsiauqd.py
_timeavifjiqnyywzdtgdwkudkzf24ysga3cwuuzj73ibqaneq4wcv5id.py
_timeay4oanuv3kbyuovktwwsunaha7jy7qpsxgufhwcrwvva4ctr4cad.py
_tkvqg7vhyegatflkfsai2bn52pyvgacxuouy4wt4p3532ukbjigqtoqd.py
_tmexhlujjvyq7gvfzz5tarjwp3xmu2onjugbkddlviwzpz3nkavbzuid.py
_topguns3x4pkpfimodj4iqgfadnwncxg3l5lnui45jmjt2fiz5uwlxyd.py
_torbuyxpe6auueywlctu4wz6ur3o5n2meybt6tyi4rmeudtjsysayqyd.py
_torguns6gkwrtol7wv3oi2adixpaq2vtsiueny7nwtx2k3fkehr2hnid.py
_tormarta4d4zdiyqsnzqhbdh6v4ykq7z4ahqrwbq2tmt2j2c2vjigjad.py
_torzonrrxahftcnkbs63226536t653rp7g4loyjcxwwvcyzeo4n7ezid.py
_trqyj7l6h6ctiudvnr226vrrsgbewmppb4gd53vdeocmx3pyci5kc2qd.py
_tu5amwc74h5toreukzcydkc2wgoykenshbudmsnyw33y6qh4bbzh7aad.py
_twc6bgoqch6m256x2bgquqmnd5clxcalgddx7xgw5gbnvmfhk3j7g6id.py
_twxlibyonozvyssgzxqpylhxjdgk737ok6sgy75axflspgwhcoec2cyd.py
_u7nlyanrdlewtkxepbwchdkarm2f35jub2q2wwnbmshzttxavhhgk4ad.py
_uaeczqldohewb7pqibicvd4jfx3nnvmpyqkquttpw7mpvmmhfpbazlid.py
_uaedrlatywyi42gjhzz4ahzbheiutygs5oug365gqchncmp5yvqh22qd.py
_ukft62ok4t6w53doxazh3jdwrcihbdiebbr5eztfyzxz2g5t7qwdckad.py
_ul64z4bykoyqupbixrj2o57yvsojp7btkz3fhadmw7xzw4lsgoumcdad.py
_ulqeqvuu4pkskcppkuuuo3fs34twn7a5qjn7swcyqdcuyzqy5vc65wad.py
_undeb6m465pjocdl6kvyiwefj5xxzcu3hgzngpfe5eolw764suu5v3id.py
_ungz5kxgtfxd2ixowgltjhp7hci73z5yn6onxyjfyww4kj6nsfflcwqd.py
_uniphmaxuqsi6jppljgsl5wjncandwoibmyuhomgnc3v2nxvgr5fahad.py
_ursjznpelzrj3vygiyreqjajxlr2inimf7br2g3hrt5x53tefnbffpad.py
_uz2si36uyy5ii2rn6rylep4gmsotznvgjhif4o7hckzemu7xxjqwc7id.py
_v3hq5qnsnffv2jj3xj56rvrlu4ir2vconfhpmxbojxkiaygke5s2nuyd.py
_v3ndor4u55norfecywj4ekgqp7jiyrfp4id3rzsvd63sfdpsykylu6qd.py
_vbc6yz72yhi2hn2ezdl57pab5zztydlrd4wothqjgdzvueswyajkanqd.py
_vendorme52lzccihumm4w7gek26mznd47xq3ghvqz2cig5uaa7n7hlqd.py
_vhdvcsghrs65vzqwagcq6erhky5khkzofd6wu3lbbvlodx4b5n7554yd.py
_vhnbyk4gduhni77fsgalp3jfms4b7xwtbbjltpmwht46ysz2ckbqbjqd.py
_viodovwbnu3tv6m5klce6tm5p7jq7mwonmwu2m564ehqh6sx7ndqvyyd.py
_vivsawnkbum46uhlqnsbzlwbsvc4yyi2o7nci6qqafuam3vqwzu652yd.py
_vjbrzpjdjnymqdbuvcjwnj5w2xo65gfehe6c32eh3rv5t324xafo4kad.py
_vorodtm2plbuobna3wmkifs53saq4mlv5v7ye5ltj25puxsbt7hdrwqd.py
_vouapqsskzzdwwwgrzmy63dpjeb746n4kkyl3r6blsd3t5exlagf7rid.py
_vpirzthkbzedr53dnk3d73cvybb5rzpfwf7xjsdlte7cntk6r6l6tnyd.py
_vshvn7w7efva4vzuh4le5ep5tl2zwyxflngk6riupmuspanc6mk6bkyd.py
_w2aemo4mg3g2qxv47ra4p353iqflfgcw4zrkxlljv5er4qckjl3bctqd.py
_w4tyf3mbm36rtww237zhzlv6yahymck2ntt4ij6tf5qksu6qlhlpyjqd.py
_w4w7o3brn2nr6ithf52ap23ob4bxc622mnlkwup3zoln4qt6zggfklad.py
_w6qzqfdwwsaxt3kcghyi5zx3iql2s5lavjo7eujxfu3hgj7bzgjoldqd.py
_w75hu4da2hohnbcinpcsg4keew6cz6qhqi4k366sy455b7t2mxnsvjyd.py
_webdfirtbn43f3p47676qzx6wtlkhgfudj45zzmspy33aksnkllsr6ad.py
_weedme6yqggs33wiiefjqtn52s6hshoec5bras4wzpiq2ifibejbduqd.py
_wges3aohuplu6he5tv4pn7sg2qaummlokimim6oaauqo2l7lbx4ufyyd.py
_whcftdtu2wykwgaiymdtnenwi5iq74dxlp4imbh7egjinrq6jqsmnaid.py
_wolft3xm254bvmv63hoe5v66kazyp7qoy2donh6fniwhur3qfynwljad.py
_worldyyyi2ktoisdqjjq4zlt3jftejabo443lb67pfofr2i5gqlkaoqd.py
_wqst7wekf7sy5pma2mld3vwcdlbkdmwzlp237ewgelu4uywtrf364wad.py
_wqxnxz352hk32n2oxreacw6knaawv2upix26gmkrvz6cir2dwgzn67ad.py
_wtm5scpkbly5mrlqhodyotndtkz62j27lx4fm5k26hhst7jil44kq2qd.py
_x4fcceh6bfwxr4b54i6tgufhmut43jqqi4oeapg3oubjlrkzzm5bw5qd.py
_xblackmruj5bhifn5mmwd6zefslwccspys2rguj5oh627yrurzagiwad.py
_xcashtqgin6xa2p3bqzhpxdqbbzyiepxtu7sryy4czbojrup5mjh7bad.py
_xcluusuncmrjq6qw25aix7sxlpgpnjaudr5tyxp7j7egf4yydm2thzid.py
_xetqztj2st4f4jzfumome7s4htwaqqzhtgy7q4cwoimngmti5baggoid.py
_xhc5hl6tnyfwx47cy3zhhs2ny7p5vioswzzoqybxvhjgzobsv56af6qd.py
_xmrbazaar6wzcr3wvj4anpnfwv5tcikgdurtzzfba233g52fa2fjx3qd.py
_xnl5okec4cn2sbp4lkk6an42sxl6lbfctme324im5qqteoacovvpnrid.py
_xogsae4hbotk25udazyjm2uty47ggmiguvizl7egdlxaeyowndjoutqd.py
_xrxyxwt66ty3zkh6pxudl6hpkt6jm2f4oxcky3m7trrmglvlbzkqnbyd.py
_xx5ammkhkblz33a2c5msindsudfifuzhc4l44itq4dhrvysr63eojhyd.py
_y2ggeuanhkiafcey67tcdy3nm77ivefvk6hunbnqnsr4lsa6w5lauoad.py
_ycidiw3dtodc3j3paanmod2ki2brgm6pzoymg2om2grrcupxxb5tnjqd.py
_ydzkrn6mnw2m2h65ubel7xohbngxa5jiwh2iz3oyhagn57uxbysdjqqd.py
_ymsnqxf2kq7cikb5h46i52ltwqo3bqhnfd7bol3uavfeuexkj63cjqqd.py
_yn6almlq5zvmulilr5n3ojnimujezr4uexq6rufypu7eu6nkttsefyad.py
_yusk7trriln55hp2jr24v5hz4hfpusecsgt6dcrxojsj74jmp4ot6vyd.py
_z5lybkpu5rb6st23zltnq44out2w2usv2vyeaoa2yw5fyfrplwerujid.py
_z5vkhytcmj4smau7cvpappmrnccowdvtbsh7c6c5ufqbglutwezhhfad.py
_zbrud5h5kvktk6ze4pr76wuyfy7khigmlccusyrziompzkbky65btsqd.py
_zkxvemrksvvwups4zlizjkzafntbfhd6w7w7mmjqpwrbolamlhhwyxad.py
_zl5ymhsyz2nwvqnmv4b2qbvvp3i3pzqeah3dd7ezvja6hodbg54cb4id.py
_zmzixhbbzmyjx4n4otxk7ykgownocognxf5k2ws3zb3gial6okezd4yd.py
_zoernfjhvk6eusgqgop34f55yly7zfcg3d6rjbilwyrwp6fdkq3aw2ad.py
_zxgq7ijyqoih4ypnyll6te5njftzgpzitgwlo6dnr6xtoqvcn3uccpid.py
```

</details>

### Added scripts: news (80)

<details>
<summary>Show all 80 news scripts</summary>

```text
_amnesty_security_lab.py
_arcticwolf_labs.py
_ars_technica.py
_asec_ahnlab.py
_bitdefender_labs.py
_bsi_wid.py
_cert_at_warnings.py
_cert_hr.py
_cert_pl_cve.py
_cert_ua.py
_certeu_advisories.py
_certfr.py
_certfr_cti.py
_checkpoint_research.py
_cis_msisac.py
_cisa_advisories.py
_cisa_alerts.py
_cisa_analysis_reports.py
_cisa_ics.py
_cisa_news.py
_cloudflare_threat_reports.py
_cybersecuritynews.py
_cyble.py
_datadog_security_labs.py
_dfir_report.py
_elastic_security_labs.py
_f5_labs.py
_flashpoint.py
_fortinet_research.py
_gbhackers.py
_github_security_lab.py
_google_threat_intel.py
_helpnetsecurity.py
_hibp_breaches.py
_hkcert_bulletin.py
_horizon3_research.py
_hudson_rock.py
_incibe_cert.py
_incibe_citizens.py
_incibe_ics.py
_industrialcyber.py
_ipa_jp.py
_jpcert_alerts.py
_jvn.py
_kaspersky_ics_cert.py
_kisa.py
_malwarebytes.py
_microsoft_security.py
_mkd_cirt.py
_ncsc_nl_advisories.py
_ncsc_nz.py
_ncscuk_reports.py
_project_zero.py
_proofpoint_threat_insight.py
_qualys.py
_quarkslab.py
_recordedfuture.py
_risky_bulletin.py
_samsik.py
_sans_isc.py
_securelist.py
_securityonline.py
_sentinelone_labs.py
_si_cert.py
_socket_research.py
_sucuri.py
_symantec_threat_intel.py
_talos_blog.py
_techcrunch.py
_thecyberexpress.py
_theregister.py
_unit42.py
_volexity.py
_wa_ag_breaches.py
_watchtowr.py
_welivesecurity.py
_wired.py
_wiz_research.py
_wordfence_research.py
_zscaler.py
```

</details>

### Added scripts: social (105)

<details>
<summary>Show all 105 social scripts</summary>

```text
_52pojie_cn.py
_anti_malware_ru.py
_at4re_net.py
_bbs_kafan_cn.py
_bbs_kanxue_com.py
_bf_st.py
_blast_hk.py
_cardforum_cc.py
_chinapyg_com.py
_codeby_net.py
_community_avast_com.py
_community_zeek_org.py
_cracking_org.py
_crackings_one.py
_crackingx_com.py
_crdcrew_cc.py
_cyber_gerki_ws.py
_darkmonn6oy55o7kgmwr4jny2gi2zj6hyalzcjgl444dvpalannl5jid_onion.py
_darknet77vonbqeatfsnawm5jtnoci5z22mxay6cizmoucgmz52mwyad_onion.py
_darkstash_com.py
_decodehub_org.py
_demonforums_net.py
_discuss_grapheneos_org.py
_discuss_privacyguides_net.py
_douban.py
_elitepvpers_com.py
_enclave_cc.py
_exploit_in.py
_ezcarder_is.py
_facebook.py
_fearlessrevolution_com.py
_foro_elhacker_net.py
_forospyware_com.py
_forum_90sec_com.py
_forum_cheatengine_org.py
_forum_eset_com.py
_forum_exetools_com.py
_forum_flipper_net.py
_forum_hackersploit_org.py
_forum_hackthebox_com.py
_forum_hashkiller_io.py
_forum_kaspersky_com.py
_forum_kasperskyclub_ru.py
_forum_qubes_os_org.py
_forum_suricata_io.py
_forum_torproject_org.py
_forums_hak5_org.py
_forums_kali_org.py
_forums_malwarebytes_com.py
_forums_spybot_info.py
_forums_whonix_org.py
_geekstogo_com.py
_hackerboard_de.py
_hackernoon.py
_hacktowns3sba2xavxecm23aoocvzciaxirh3vekg2ovzdjgjxedfvqd_onion.py
_hard_tm_com.py
_hashcat_net.py
_htdark_com.py
_imgur.py
_instagram.py
_leakbase_su.py
_leakzone_org.py
_linkedin.py
_malwaretips_com.py
_mazaoneiwx7fi3jdsflm3rlhjhrt345drhtl3tqpwxiyjpbt2xb3vwad_onion.py
_moonclub_cards.py
_mpgh_net.py
_nodo313_net.py
_nullcave_club.py
_nulled_cc.py
_ownedcore_com.py
_piratebuhta_net.py
_pleroma.py
_quora.py
_rehubcom_ag.py
_reshax_com.py
_reverseengineering_stackexchange_com.py
_reversing_center.py
_rootsploit_org.py
_rstforums_com.py
_safezone_cc.py
_security_stackexchange_com.py
_stackoverflow.py
_sysnative_com.py
_tforumcf7j3mxrrketsyaydivmhjza457dvna3xuumbmrmipktar4bid_onion.py
_tieronemkfevyizxcnt355agysp2iemvhon6iyclwrc7yuc7oszgzrid_onion.py
_tiktok.py
_toolba_se.py
_trojaner_board_de.py
_turkhacks_com.py
_tyoeevaqn6alon623bfm7r4e7z3so54ky6uvbqkza3fyun7g4cm24hyd_onion.py
_underc0de_org.py
_unknowncheats_me.py
_verd_st.py
_verified3vr2kdbnza6c3e5ak4z5xmtti4hx36dfg3kbi6pwekztvsqd_onion.py
_voided_to.py
_warriors_to.py
_wasm_in.py
_wilderssecurity_com.py
_xss_mx.py
_xxe_li.py
_xz_aliyun_com.py
_yougame_biz.py
_zhwjzfb6zcpngk35h3vhtpmui5ptaytuftf3wklv3bvsy3xamw34fzyd_onion.py
_zismo_biz.py
```

</details>

### Added scripts: extension resources (1017)

<details>
<summary>Show all 1017 extension resources scripts</summary>

```text
abcaccountingucoznet
aboutme
actikomucozru
adminsoftucozru
aetherhub
afsocucozru
aheraru
aiidadiscoursegroup
aikidomariupolucozru
akniga
aliliangardendiscoursegroup
alisaclubru
allhockey
allmusucozru
allthelyrics
ameba
ameblo
americanthinker
aminus3
analitikaforexru
anchorecommunitydiscoursegroup
androidforums
angara
angelgothics
animebase
animegrandmoysu
animelendmy1ru
animeuknews
aniworld
ankorducozru
antalyaucozru
antihackucoznet
antiquebottles
antivirusmoysu
antizombieucozru
apelmonodua
appairnftscom
appleinsiderru
archiveofourown
arduinoforum
arena
argoscommunitydiscoursegroup
aributru
artinvestment
artnatamy1ru
artsy
asciinema
asecuritydoam
askbioexceleu
askmrrobotdiscoursehostingnet
askvrchatcom
atcoder
atlasdiscoursegroup
atmclubmoysu
au
audiojungle
audiomack
authortoday
autolada
av3dnru
aviaforumucozru
awfulsystems
awgosdrspace
awsskillsprofile
azhackucoznet
backstagepolyendcom
baggiucozru
bambulabforum
bashteploventucozru
bayoushooter
bbclubucozru
bbseeclubtop
bbsfit2cloudcom
bcetytru
bchartscombr
beatlucozru
belgaesocial
bereal
bereaucozru
bestfriendschat
bezuzyteczna
bgforum
bgindiscoursegroup
biggerpockets
bigo
binhot3dnru
biohackingforum
bitchute
blast
bldgsimonebuildingorg
blenderartistsorg
blipfoto
blitztactics
blizzhackersdiscoursegroup
blocsforumdiscoursehostingnet
bloggerbloggercom
blogzzaclyscom
bnddiscoursegroup
boardmddcdev
boardphpbuildercom
boardscore77com
boardttvchannelcom
bookandreader
boosty
breakerstv
broadbandgeniediscoursegroup
brusheezy
budo52ru
buildermetamaskio
bunpro
buyforexucozru
buzznet
cadaverzianucozru
caddycommunity
cajunthredsdiscoursegroup
calculixdiscoursegroup
canadianfootballforum
castingcallclub
catinbootsucozru
caves
centrspektrru
cfdonline
chaosfurssocial
chapeldiscoursegroup
chastyscucozru
chatujmecz
chelentanoucozru
chemistlab
chess
chiefdelphicom
christianvideo3dnru
clarityfm
clashlangdiscoursegroup
cldesmoscom
clozemaster
clubcomedyclansu
clubministryoftestingcom
clubmodevolcom
cmet4uk
cnet
cnodejs
cocoscommunity
codebynet
codecanyon
codedex
coderwall
codesellerru
coffeeworlducozru
collectorscom
collegyucozru
colorlibsupportcom
comedy
commonscommondreamsorg
commonsishtarcollectivenet
communityabbyfr
communityadminforgede
communityaliceblueonlinecom
communityamazondevelopercom
communityamperecomputingcom
communityankihubnet
communityanovaculinarycom
communityanvizcom
communityaphhiveorg
communityapollographqlcom
communityappfarmio
communityappinesfr
communityappinventormitedu
communityarduboycom
communityaristotlemetadatacom
communityasepriteorg
communityasteriskorg
communityauth0com
communityautomationedgecom
communityavgcom
communityawebercom
communitybaserowio
communitybearapp
communitybitmovincom
communityblokadaorg
communitybloomreachcom
communityblynkcc
communitybrainmaporg
communitybrevocom
communitybrewwcom
communitybrightpatterncom
communitycambiumnetworkscom
communitycarbide3dcom
communitycartalkcom
communitycertifythewebcom
communitycesiumcom
communityclarkcom
communitycoopstech
communitycrewaicom
communitycrossreforg
communitycybozudev
communitydataquestio
communitydatocmscom
communitydeltaexchange
communityderivcom
communitydeveloperatlassiancom
communitydirectuscom
communitydremiocom
communityefoundation
communityelfsightcom
communityemmaappcom
communityendlessoscom
communityepinowcastorg
communityespboycom
communityeufycom
communityevolveauthoringcom
communityfastlycom
communityfiberyio
communityfirebasestudiodev
communityfireblockscom
communityfirecorecom
communityforestadmincom
communityframework
communityfreepbxorg
communityfunnelishcom
communityfutureaudioworkshopcom
communitygainiumio
communitygamedevtv
communitygemsofwarcom
communitygeodynamicsorg
communitygetmailspringcom
communitygetswipein
communitygigperformercom
communitygladysassistantcom
communityglideappscom
communitygrafanacom
communitygraylogorg
communityhedgedocorg
communityhitpawcom
communityhomeyapp
communityicingacom
communityicons8com
communityinfiniteflightcom
communityinfluxdatacom
communityinkbirdcom
communityinvvestco
communityiotawattcom
communityipfireorg
communityipinfoio
communityjenkinsio
communityjitsiorg
communityjoinmastodonorg
communityjupitermoney
communitykodulario
communitylatromicombr
communitylivekitio
communitylocalwpcom
communitylsstorg
communitymailpileis
communitymappedincom
communitymemfaultcom
communitymilkvio
communitymindstudioai
communitymonzocom
communitymorphmarketcom
communitymorsemicrocom
communitymrtrixorg
communitymycroftai
communitynabaicellscom
communitynaturephotographersnetwork
communityneo4jcom
communitynetdatacloud
communitynethserverorg
communitynetwrixcom
communitynextwcom
communitynianticspatialcom
communitynlnetlabsnl
communitynocodbcom
communitynolocoio
communitynortoncom
communityntppoolorg
communityomniavisit
communityonixcom
communityopenaicom
communityopenastronomyorg
communityopenbisch
communityopenconversationalai
communityopendronemaporg
communityopenemsio
communityopenflorg
communityopenfoodnetworkorg
communityopenhaborg
communityopenpbsorg
communityopenrentcouk
communityopentargetsorg
communityovhcloudcom
communityp2puorg
communityparticleio
communitypenpotapp
communityperplexityai
communitypickaxeco
communitypinterestbiz
communitypix4dcom
communityplotlycom
communitypodloveorg
communitypostmancom
communityprivacyideaorg
communitypuzzlequest3com
communityquickfilecouk
communityrachiocom
communityrampcom
communityrapydnet
communityretoolcom
communityrevolutionarygamesstudiocom
communityrobotimecom
communityroonlabscom
communityrootsmagiccom
communityrstudiocom
communitysat4allcom
communityshipherocom
communityshopifycom
communityshopifydev
communitysigmacomputingcom
communitysimplefoccom
communitysmarthomecom
communitysmartthingscom
communitysolderedcom
communitysparkpntcom
communityspritelyinstitute
communitystapeio
communitystardogcom
communitystarknetio
communitystepsmashcom
communitystereolabscom
communitysuitecrmcom
communitysunnypilotai
communitytagoio
communitytawkto
communityteableai
communitytempestearth
communitytheboxingmanagergamecom
communitytheta360guide
communitythevirtualinstructorcom
communitythunkablecom
communitytogglcom
communitytourradarcom
communitytrading212com
communitytraefikio
communitytransifexcom
communitytransloaditcom
communitytulipco
communityultralyticscom
communityumbrelcom
communityunixcom
communityupstoxcom
communityusconcealedcarrycom
communityvercelcom
communityvtexcom
communitywayfarerscopelycom
complineucozru
cplusplus
cpuucozru
crates
crevado
crossfaernetmy1ru
cryptohack
cryptomatorforum
csruucozorg
cssbattle
cssnn52rusucozru
csstrikezorg
ctftime
cubecraftnet
curseforge
cyberharvardedu
cybersclansu
cytoidio
dangerousthingscom
dasauge
dcinside
deathlegionucozru
deathnoteucozru
deepdreamgenerator
demotywatory
deutschauto68ucozru
devto
devtribe
dhelpucozru
diablocoolucozru
digitalocean
diigo
dimensionalme
dioramaru
discoursehaskellorg
discoursejulialangorg
discoursejupyterorg
discoursemcstanorg
discoursemozillaorg
discoursenoderedorg
discoursepihole
discusscirclecicom
discusselasticco
discussfastpitch
discussflarumorg
discusshashicorpcom
discusshuelcom
discusskotlinlangorg
discusskubernetesio
discusspixlsus
discussprosemirrornet
discusspython
discusspytorchorg
disqus
djagi
djfintucozru
donationsalerts
douban
doytruntucozru
drawingsbaseucozru
drdenisovru
dremeldoam
dushschoolmoysu
dvkstyle3dnru
dzhida2000ucozru
easyjobucozru
edumonchru
eintracht
ekzoticsad3dnru
elibrarytips
elpizzaucozru
empowher
englishinforu
enillogicopediaorg
enuncyclopediaco
ercevoru
ethereummagicians
ethresear
excelworldru
exophase
exploretalentcom
fableroucozru
facultyofmedicine
faillyuboiucozru
fareastclansu
fatucozru
fclmnews
fcrubin
ffmbio
firesofheavenorg
fireteamclansu
fishingsib
flarumes
flashflashrevolution
flickrgroups
flightradar24
flirticee
flru
flutterforumorg
flymy1ru
focuspocus3dnru
fordatingru
forest
forextraderdoam
forumbananapiorg
forumbonsaimiraicom
forumcockroachlabscom
forumcscartru
forumendeavouroscom
forumfreecodecamporg
forumgarudalinuxorg
forumghostorg
forumgitlabcom
forumgolangbridgeorg
forumhistoryru
foruminaturalistorg
forumjucecom
forumleasehackrcom
forumobsidianmd
forumodua
forumsbalenaio
forumsdockercom
forumsdromru
forumseeedstudiocom
forumshotcutorg
forumsimmigrationcom
forumsindiegalacom
forumslawrencesystemscom
forumsnapcraftio
forumsoperacom
forumspimoronicom
forumssketchupcom
forumssonicretroorg
forumssteinbergnet
forumstnationcom
forumsublimetextcom
forumsvisualparadigmcom
forumswyzecamcom
forumtauck
forumtradeprintru
forumuipathcom
forumwordreferencecom
forumyuristov
forumzorincom
foxrecorducozru
fragment
framapiaf
freeproxyucozru
freesound
fsmodsrusucozru
fstkolosdoam
futajiststudiomoysu
g2gcom
gadjetmoysu
galactictalkorg
gamemobiucozcom
gcupru
gebirgsucozru
geeksforgeeks
gentlemint
geocaching
gettr
giantbomb
giphy
gitea
glbyh
globalvoices
goddamnucozru
golangbridge
golasavkfree3dnru
goldderby
goodgameru
gorposmosru
gotovimdoma
grigorovoclansu
grodnofish3dnru
gsmstandartclansu
gtgarazh3dnru
gulfcoastgunforum
guru
gym5net
hackaday
hackappucozru
hackersploit
hackingwithswift
halolucozcom
haogan3dnru
hardforum
hokagetv
holodilshchikucozru
honda
housemixescom
hubpages
huggingface
hulyagankaucozru
hunting
ibmt3dnru
iceberg116ru
icqbotmoysu
icuucozcom
ifunny
igraonlineucozcom
igrarena
illustrators
imageshack
imgflip
imood
inetjobucozru
infourok
insanejournal
intigriti
ionicframework
iphonesru
irteamru
israelrentinfo
issuehunt
itemfix
itvdnforum
jbzd
jigidi
jigsawplanet
joplinapp
juce
jump3dnru
juventuz
kakvkontakteucozcom
kapustadoam
kashalot
kfirzahavucozru
kidicaruswikiorg
killerucozua
kinohitclansu
kinotv
kloombacom
klubskidokru
komarovoclansu
konibodomucozru
kosmetista
kotikmy1ru
kraskiprazdnikaucozru
kristallovnet
krumucozru
kursk46ucozru
kuziniucozru
l2bestclansu
laiucozru
lakshmifmucozru
laracast
laserwar48ru
leasehackr
ledvectorru
legendarusveoucozru
lesbeyankaucozcom
liderdzrmy1ru
lifewayucozru
lightstalkingcom
linkedin
linktree
linuxfoundation
listedto
listography
litgeroyucoznet
lizamy1ru
lnameucozru
lnkbio
lock3dnru
lovemagicclansu
machelp
macosx
magicsquareucozru
magictarotru
mamasuperru
mamuli
manifoldmarkets
manualsclansu
manylink
mapillaryforum
marchenkovdoam
margaritasclansu
mariupol4x4clansu
martech
marymucozru
max
mednolitru
medvestnicru
memory57ucozru
metadiscourse
metadiscourseorg
metroidwikiorg
metroman3dnru
mikeleloconteru
milnerelenaucozru
mineplexcom
minfincomua
minnacucozru
mintmecom
mircasovru
mix
modxpro
moedeloucozcom
momentsucozru
moneysfirstru
monkeytype
morozovkamy1ru
morshanskucozru
moskoviamoysu
motorka
mouthshut
movimy1ru
moxvouz
mozganetucozru
mql5com
mstdnio
muffingroup
music2djclansu
musicbunkerru
musiconemy1ru
muzika3dnru
mydramalist
myinstants
mylot
mymailrubkru
mymailrugmailcom
mymailrulistru
mymailrumailru
mymailruok
mymailruvk
mymailruyandexru
mynicknamecom
mytechbookucozru
mytrans3dnru
mytucsonucozru
mywed
n1001tracklists
n2ndcareerscommunitydiscoursegroup
n3dtoday
n4401013dnru
n4948ru
n4g
n5levelucoznet
n655iapucozru
n74507ucozru
n783doam
n7dach
n8ncommunity
n8wayruncom
n96moysu
n999md
n9interi3dnru
narutofanucoznet
narutorolegameucozru
nesiditsa
newreporter
nfclubru
ngllink
nhl
nicefriendcatsucozru
niflheimtop
niketalk
nojayurtru
nokia6233ucozru
nookipediacom
not606com
novomoskovskmy1ru
npm
npmpackage
nucastlecouk
nygunforum
oakleyforumcom
obkonucozcom
obmanunetclansu
observable
odonvvru
odysee
ofc65ru
officeforums
ok
okmorgru
omoimot
onlinemovies3dnru
onlyfans
oopkmoskvaucozru
openframeworks
opengameart
opensea
opensource
openstreetmap
opinionucozru
oskolfishingucozru
ostaucozcom
overclockers
ovoucozru
p1ratucozru
paltalk
panzer35ru
paragraph
partner3dnru
partnerkincom
partyvibe
patchcom
pbase
pentesterlab
periscope
periscopemain
physicsforums
pik100ucozru
pikabu
piobetsucozru
pirohimicucozru
pixelfedsocial
planetminecraft
playstrategy
pogz5615ucozru
pokemonshowdown
polygon
polymarket
popugiucozru
portalcs163dnru
poshtovikucozru
potystoronyucozru
prenatalclubucozcom
procssteamucozru
prodigymoysu
profiru
profsouzauru
promodj
promptbase
pronounspage
prosmart3dnru
prosvetucozru
provincialynewsru
prozaru
prvpl
psyera
pttwebcc
punxucozru
qbn
quartertothree
questionablequesting
rabotenkaucoznet
rapbeatucoznet
rappad
rasslabyxa
rawgio
razborkajapan3dnru
rcloneforum
realspucozcom
redbubble
redcafe
reincarnationforum
relaskoru
remont56ucozru
replit
residentadvisor
rielt55ucozru
rigczclub
rmmedia
rollitup
rotarusofiucozru
rottentomatoes
ruanekdotru
rudtp
rusmmmucozru
russemyadoam
rustlang
salavat3dnru
salutmru
samlib
sampucozde
satelectronicsucozru
savingadvicecom
sayty3dnru
scalalang
scbucozru
schlock
schoninucozru
school1065moysu
scooterhelperucozru
scoutwiki
sebastopolucozru
secretkompas3dsu
sefirutru
semenovaklassmoysu
senseucozcom
seoforum
serwisucozru
sessionizecom
sfinxcatsucozru
sharzhportretru
shazoo
sherwood3dnru
shikimori
shitpostbot5000
shorby
showme
shporgalkiucoznet
sibcoinsucozru
signal
sisvcom
sketchfabcom
skyblock
slashdot
slides
smartlabru
smartplayucozru
snapchat
so4ineniyaucozru
socforum3dnru
softwm3dnru
sokalucozlv
somersoftcom
sony1273dnru
soup
sourceruns
soylentnews
spaces
spells8
splatoonwikiorg
spotify
ssbwikicom
sstalkersru
starcitizen
staroverovkaucozua
statuscafe
steam
steamgroup
stellerco
stereo
stihiru
stratege
strategywikiorg
strava
strupravlenieucozru
studfile
sublimeforum
sufficitucozru
sugoidesu
suomi24
supportbluesystemscom
supportwirenboardcom
swapd
symbian9clansu
szmerinfo
tachographucozru
talkmacpoweruserscom
tamtam
tanukipl
tarjaturunenucozru
tatyanaartucozcom
taxibelgoroducozru
teampros3dnru
telescopeac
teletype
texasguntalk
thaicatru
theatremy1ru
themeforest
theodysseyonline
tinkoffinvest
tks
tomdoam
tonetoucozru
tonometerbot
topcheatsucozcom
topmate
topreklamaucozcom
torrentsigraucozru
torrentsoft
toster
touristlink
tradingview
trainzvlucozru
trashboxru
travellerspoint
travis
trepupcom
tripline
truckersmpru
truelancer
turpravda
tvigraclansu
tvtropes
twitter
ubuntumatecommunity
ugriucozru
umhoops
uralslobodaucozru
urmaiurmaevoucozru
usalife
usersrustlangorg
v3deru
vadyaucozru
valinorcombr
vch34693dnru
vdvbelarusucozcom
velog
vero
vgorahucozru
vgtimes
vgtimesgames
videhelpcompmy1ru
videoforumsru
videohive
videomuzonucozru
vipcccpclansu
vipicqucoznet
virgool
vitalfootball
viupetra3dnru
vlr
voices
volkswagenlvivua
vracing3dnru
vse1ucozcom
vsemobilemy1ru
w3challs
w7forums
wakeupucozcom
warframe3dnru
warframemarket
warpcast
wasmin
weaponsasucozru
webdom3dnru
webmediaucozru
weddingimageru
weforum
wewinru
wikithemanaworldorg
wimkinpublicprofile
windows10forums
wmmailwmmail3dnru
wmucozcom
wolframalphaforum
wolpy
wordnik
wordpressorg
wordpresssupport
wowcircle
wowgameru
wowhead
writeas
writercenter
wworkmy1ru
wwwkinokopilkapro
wykop
xakerminusucozru
xgmguru
xiaohongshu
xing
xitlarucoznet
xn246kcaal6ajt1cpibnu7d5dtcxnp1ai
xn80aqkf5cbxnp1ai
xorazmviloyatiucozcom
xristosvouz
youpic
yrasucozru
zabseloucozru
zareshetkoimy1ru
zdorov10ucozru
zebestucozru
zennenhunducozru
zerkalasteklaru
zidmoysu
```

</details>

---

## 13. Security fixes, hardening & improvements (audit items, fixes after the audit, open items)

Security review findings and their status, imported from the internal audit (2026-11-19).

Three sections added on 2026-11-21 follow the audit lists: fixes made after the audit, audit entries whose behaviour changed after the audit, and open items (what is left).

Fixes are verified where noted; prod-verified items were confirmed on production the same day.

Every entry keeps its audit ID (e.g. RT-7, CLI-9) and severity, grouped by area and ordered high-to-low severity.

### Security fixes & hardening (514)

**Residuals closed 2026-11-19 (in code; pending redeploy):**

- **RT-10 / RT-233** — alert-summary cache-write returns a generic error, not the raw Redis exception.
- **RT-25** — SMTP sender address returned only to admin/maintainer, not demo/members.
- **AUTH-23** — tenant slug is a placeholder until email verification, then promoted (no pre-verification squat).
- **INF-11** — dev run.sh no longer makes workspace dirs world-writable.
- **INF-89** — Storage now scans mail files too (already encrypted at rest).
- **CLI-45** — password reveal re-confirms each time (latch removed).
- **CLI-56** — "sync all" only crawls targets on the platform's own host.
- **EXT-18** — extension host-validates the crawl cursor, not just the url.
- **RT-240** — already correct by design (send-time validation in prod; local dev intentionally exempt).

#### Authentication & sessions (54)

- **AUTH-2** `HIGH` — The 2FA code check has no attempt limit, so the 6-digit code can be guessed.
- **AUTH-3** `MED-HIGH` — A disabled account can re-activate itself through resend-verification.
- **AUTH-14** `MED-HIGH` — The /admin panel runs delete actions on plain GET links, which the cross-site write check does not cover, so a link opened by the signed-in admin can delete a tenant or user.
- **AUTH-4** `MED-HIGH` — /api/signup/verificaion checks passwords with no rate limit, bypassing the login lockout.
- **AUTH-5** `MED-HIGH` — The extension WebSocket accepts its cookie without checking Origin, so another site can connect as the user's extension.
- **AUTH-6** `MED` — Password change or reset does not end existing sessions, and disabled users are not rejected by the session check.
- **AUTH-8** `MED` — Signup stores a client-supplied bcrypt hash as the password, skipping the password rules.
- **AUTH-9** `MED` — Every reset and verification mail is also copied to the sender mailbox.
- **AUTH-10** `MED` — /api/support lets anyone send the platform's branded mail with their own link to any address.
- **AUTH-15** `MED` — The login lockout counter is read before the password check and written after it, so parallel attempts count as one; the public stealer-log quota has the same flaw.
- **AUTH-7** `MED` — The normal login route gives a 30-day "extension" token to any client that asks for it.
- **AUTH-26** `MED` — Password checks run on the main request loop, the demo login has no limiter, and logins with a new unknown username each time never trip the address lock, so a few requests per second from one address can stall every worker.
- **AUTH-36** `MED` — The secret sealer stores any submitted value that already carries its sealed prefix unchanged and later decrypts it with the platform master key, so a tenant maintainer can submit a chosen ciphertext as the tenant SMTP password and receive the plaintext as …
- **AUTH-17** `LOW-MED` — /api/subscription/request needs no login and has no limit, and mails the platform's branded message to any address.
- **AUTH-18** `LOW-MED` — The password is set at signup before the email is confirmed, so someone can pre-register another company's address and own the account once the recipient clicks the link.
- **AUTH-19** `LOW-MED` — Cross-origin requests with cookies are accepted from every subdomain of the platform domain, so a script flaw on any tenant or webmail host reaches every other tenant's session.
- **AUTH-27** `LOW-MED` — Failed logins are counted only per address and username pair, so a few guesses against many usernames from one address, or one account from many addresses, are never slowed.
- **AUTH-28** `LOW-MED` — Trial expiry is enforced only for the member role, so an analyst created during the trial keeps signing in and refreshing after the trial ends.
- **AUTH-29** `LOW-MED` — Turning on 2FA only sets a flag and the authenticator is enrolled at the next password login, so someone who already holds the password can bind their own device and lock the owner out.
- **AUTH-37** `LOW-MED` — The relaxed admin content policy is chosen by a bare "/admin" prefix, so any app address that merely starts with those letters is served the normal dashboard with inline scripts and framing allowed; a variant of CLI-9 and CLI-10.
- **AUTH-38** `LOW-MED` — A disabled or suspended tenant is blocked only by a path-prefix test in one HTTP middleware, so its users still sign in and refresh, the maintainer can still write public settings, share links keep working, and the extension socket skips the tenant, block a …
- **AUTH-16** `LOW-MED` — Fixed: the parent-tenant quota is now checked under quota_lock at signup.
- **AUTH-20** `LOW` — Password change and password reset do not apply the password rules.
- **AUTH-21** `LOW` — Current-password checks on profile update, recovery-key creation and account deletion have no attempt limit, and the data-protection key update skips the lockout its unlock route has.
- **AUTH-23** `LOW` — The tenant address is taken from the first label of the signup email domain and reserved before verification, so a name can be taken ahead of the real company.
- **AUTH-24** `LOW` — User delete and update answer differently for unknown, admin and maintainer usernames before checking the caller's rights, which reveals which usernames exist.
- **AUTH-25** `LOW` — Token refresh accepts the temporary 2FA token for crawler-role accounts.
- **AUTH-11** `LOW` — The admin/crawler seed password becomes the text "None" when the env value is missing.
- **AUTH-22** `LOW` — The public demo account can overwrite its stored preferences with no size limit, and every other demo visitor then loads them.
- **AUTH-12** `LOW` — The extension WebSocket ticket can be reused during its 30 seconds.
- **AUTH-13** `LOW` — Smaller auth gaps: 2FA path skips trial/status checks, account existence leaks, /api/forgot unthrottled, chat shares not revocable, localhost in default SSO redirects.
- **AUTH-30** `LOW` — A used 2FA code is not remembered, so the same code is accepted again for about 90 seconds.
- **AUTH-31** `LOW` — The data-protection unlock counter is read then written with no lock, so parallel wrong attempts count as one and the five-attempt cap can be exceeded by someone holding the session.
- **AUTH-32** `LOW` — AUTH-13 is incomplete: signup answers "username not available" before it rejects a free-mail address, and forgot-password waits for the mail send only when the address is registered, so account existence still leaks.
- **AUTH-33** `LOW` — The shared demo account can create the data-protection key and store vault entries, so one visitor fixes the passphrase for everyone and leaves entries later visitors see.
- **AUTH-34** `LOW` — Logins completed through 2FA, failed and locked-out logins, recovery-key creation and tenant deletion leave no audit entry.
- **AUTH-35** `LOW` — RT-10 is incomplete: when the Orion Mail call fails during signup, the mail service's response or connection error text is returned to the caller who is not signed in.
- **AUTH-39** `LOW` — For custom mail hosts the SSO redirect check compares only host name and path, so plain http, any port and a user-info part pass, and "mail." under any verified custom domain is accepted even when custom mail is off.
- **AUTH-40** `LOW` — AUTH-13 is incomplete: the env template still lists localhost SSO redirect addresses and any env value replaces the safe default; production .env not read.
- **AUTH-41** `LOW` — The SSO code is not tied to the browser that started the flow, so a leaked code can be replayed from another browser within 60 seconds.
- **AUTH-42** `LOW` — The forgot-password limit is counted per target email for all callers and each request replaces the stored token, so anyone can use up a victim's three requests and void the link already mailed.
- **AUTH-43** `LOW` — /api/recover and /api/updatePassword have no request limit, and a recovery key stays valid after use.
- **AUTH-44** `LOW` — A web session has no maximum lifetime: refresh keeps the same session id and the old token stays valid, so a copied token can be renewed indefinitely beside the owner.
- **AUTH-45** `LOW` — The access cookie has no host-locking name prefix, so script on a sibling host can set a parent-domain cookie of the same name and the server uses the last one sent, placing the victim in an account the attacker chose.
- **AUTH-46** `LOW` — The cross-site write check accepts every Origin that is not http or https and compares host name only, ignoring scheme and port, so another installed extension or a page on another port can post to the extension routes.
- **AUTH-47** `LOW` — A forced password reset is enforced only by the shared route dependency; the admin panel check and the extension socket skip it.
- **AUTH-48** `LOW` — The data-protection key bundle, unlock verifier, vault password check and proof fields have no length limit, so megabytes can be stored per user and hashed per call.
- **AUTH-49** `LOW` — The vault's password-update proof is bypassed by deleting an entry and creating a new one, and entries can be deleted one by one with no proof.
- **AUTH-50** `LOW` — Admin panel sign-out is a plain GET outside the cross-site check, so any link ends the administrator's session.
- **AUTH-51** `LOW` — One master key serves as cookie cipher, tenant key wrapper, secret sealer, feeder file cipher, export key seed and stealer-key fallback, with no rotation path.
- **AUTH-52** `LOW` — In production the outer header middleware overwrites the stricter transport-security value, dropping includeSubDomains.
- **AUTH-53** `LOW` — Every request that fails validation writes a line with caller-supplied path and content type to the system log, and each write re-reads the current log file inside the request loop, with no rate cap.
- **AUTH-54** `LOW` — AUTH-17 is incomplete: the subscription route still mails the branded message with caller-chosen free text to any unvalidated address, 10 per 15 minutes per source, and the forms have no field length limits.
- **AUTH-55** `LOW` — The env template sets the production domain to a wildcard, which turns the host allow-list off, and tenant resolution then accepts localhost names as tenant hosts in production; production value not read.

#### API routes, input & tenancy (186)

- **RT-1** `HIGH` — /api/ranked/ai returns breach, stealer-log, social and all other sources to free-licence and demo accounts.
- **RT-2** `HIGH` — A tenant maintainer can grant the Monitoring permission and read system logs of every tenant.
- **RT-47** `HIGH` — RT-41 is incomplete: the kill-chain tool takes its container id from inside the message text, which nothing checks against the caller, so one AI user can read the findings of, and re-run, another user's investigation.
- **RT-312** `HIGH` — A tenant maintainer can change only the SMTP server in the tenant's mail settings while the stored mail password is kept, and every tenant's settings record is created as a copy of the platform's (or the parent tenant's) record, sealed mail password include …
- **RT-46** `HIGH` — The Micros Docker network uses the public range 172.15.0.0/24, which passes the internal-target check, so any scanning user can scan, fetch and screenshot the Micros containers (ZAP with no key, Redis, ClamAV, Micros itself) and the Docker host gateway.
- **RT-48** `HIGH` — SEC-1 is incomplete: IOC URL extraction follows redirects and also fetches links found on the submitted page, then returns the text, so a public page that redirects or links to an internal address hands the caller the internal response.
- **RT-49** `HIGH` — Takedown evidence capture opens the submitted page in a headless browser inside Micros with no request filtering, so a redirect, frame or script on a public page reaches internal services and the screenshot and page text come back in the takedown record.
- **RT-70** `HIGH` — The nightly alert job runs for every non-default tenant with no check of tenant status, approval or email verification, and signup creates the tenant and its watch list (the signup email's domain) before the email is verified, so anyone without an account c …
- **RT-51** `MED-HIGH` — The kill-chain fact, hypothesis, graph, proposal and run collections carry no owner field and are read by run or container id only, so nothing behind the gateway stops cross-user access.
- **RT-50** `MED-HIGH` — RT-42 is incomplete: the scan's sign-in request follows redirects to any host, so a scanned site can bounce the scanner and the supplied sign-in data to an internal address.
- **RT-74** `MED-HIGH` — The maintainer licence, which every self-signup tenant owner holds as their only licence, bypasses the module check on stealer-log search and on social, exploit and defacement search, record, STIX and snapshot routes and the CTI graph, so a free-plan tenant …
- **RT-76** `MED-HIGH` — Manual and scheduled alert scans send watched domains to the scanning service with no scanning licence, no public-target check and no limiter, so a free-licence maintainer gets scans the scan routes would refuse, and a watched domain that resolves internall …
- **RT-16** `MED` — The alert list skips the licence filter when compact is set without paging and returns full alerts of every category, stealer-log credentials included, to any member.
- **RT-17** `MED` — Extension socket replies are stored and unpacked with no size limit, and tenant import reads a length from the upload before checking it, so one user can exhaust backend or Redis memory.
- **RT-31** `MED` — Stealer-log masking for non-enterprise licences covers only the password field; card numbers, cookies and tokens in the same records are returned in clear.
- **RT-42** `MED` — The vulnerability scan's sign-in address is not limited to the scanned site or to public addresses, so the scanner sends the supplied sign-in request and credentials to any address.
- **RT-15** `MED` — A custom domain two or more labels under another tenant's verified domain passes the availability check, so a tenant's site can be served with a valid certificate under another customer's domain.
- **RT-3** `MED` — Any ordinary member can replace the tenant's Slack/Jira alert connection.
- **RT-6** `MED` — The tenant SMTP host and port are connected to with no internal-address check or timeout.
- **RT-8** `MED` — AI, crawl and micros routers do not check account status, so disabled users keep access.
- **RT-320** `MED` — RT-127 is incomplete: real mail sends through a tenant-chosen SMTP server still run in the shared worker-thread pool with a 20-second timeout per socket read and no overall deadline, so a tenant maintainer whose mail server answers slowly pins one shared th …
- **RT-27** `MED` — Scan jobs made through /api/scan-jobs/create are sent to the scanning service on poll without the public-target check or the request limiter, and the URL-scan (domain, subdomains, wayback), social-scrape and DKIM routes also skip that check.   ⟨note: PRIVILEDGE IOC TENANT CAN DO IT REST CANT FIX⟩
- **RT-32** `MED` — /api/stix/convert runs a slow text pattern over request text of any size inside the request handler, with no licence or limiter, so a few requests from any account, demo included, can stall the workers.
- **RT-41** `MED` — The AI chat takes a kill-chain container id from the request, and the agent writes chat text and a busy flag to that container without checking its owner, so one user can alter another user's investigation.
- **RT-14** `MED` — The takedown route forwards a user-supplied address to the social service's browser without the public-target check.
- **RT-4** `MED` — Kill-chain and Nexus download routes skip the licence, AI-enabled and limiter checks, and the MCP tool name is free-form.
- **RT-5** `MED` — Manage-profiles session routes need no role or licence and accept unbounded uploads.
- **RT-9** `MED` — Profile URLs are forwarded to Orion-Social without a public-target check.
- **RT-52** `MED` — A job made through /api/scan-jobs/create skips the rule that full-depth vulnerability scans are for root-tenant administrators, and lets a scanning-only licence run the two geo camera scans.
- **RT-53** `MED` — Stealer-log and email-breach alerts store the password as the alert description and the alert list returns it with no masking, so tenants without the enterprise licence read in clear what the search route masks for them.
- **RT-54** `MED` — The public-target check validates the host as Python parses it but forwards the raw text, and Micros and the browser parse some forms differently, so the checked host and the connected host can differ.
- **RT-56** `MED` — The phone and domain lookup and the cracked-app lookup forward user input to Micros with no target check, and Micros fetches the supplied site with redirects followed; the AI unified search reaches the same lookups unchecked.
- **RT-57** `MED` — RT-14 is bypassable: the TikTok and YouTube takedown flows open the supplied impostor address in a headless browser in the Social container, the address is not limited to the platform's domain, and Social checks nothing itself.
- **RT-58** `MED` — The profile address used by ad and hate-speech monitoring is opened by the signed-in headless browser with no public-address check, so manual and daily runs can be pointed at an internal or outside host.
- **RT-83** `MED` — The full-depth scan rule compares the depth text without trimming while Micros trims it, so any scanning user can start a full scan on the direct route by padding the value with white space.
- **RT-85** `MED` — The CTI graph document query walks to a caller-chosen depth of up to 5 from any vertex, a cluster included, with no result limit or runtime cap, so one request can pull most of the graph into memory and stall ArangoDB for everyone.
- **RT-90** `MED` — SEC-1 is incomplete: ad monitoring sends every detected ad link to the Micros URL fetch with no public-target check, so link text from a monitored page drives server-side requests.
- **RT-95** `MED` — APK analysis decompresses every archive member fully into memory, so a 10 MB upload with one highly compressible member can exhaust the container's memory and kill all running jobs.
- **RT-82** `MED` — Page parsing after each recon fetch uses title, meta, JSON-LD and email patterns that take quadratic time on bodies up to 600,000 characters, so whoever controls one fetched page can freeze the single Social process for minutes and stall every tenant's jobs …
- **RT-84** `MED` — RT-31 is incomplete: stealer-log masking replaces only values in named secret fields, but raw-type records carry the unparsed log block, so passwords inside it are returned in clear to non-enterprise licences.
- **RT-91** `MED` — Custom-domain verification proves only that the name points at the platform, not which tenant controls it, so any primary tenant can verify a domain another customer has pointed here but not yet verified, and the claim deletes the real owner's pending entry.
- **RT-92** `MED` — The breach-database and identity lookup parsers are process-wide singletons that keep results on the shared instance, so two lookups running at once return one user's records (names, ID numbers, phones, breach hits) to the other.
- **RT-93** `MED` — A Micros job timeout or cancel only marks the job; the worker thread keeps running and several fetches have per-read timeouts only, so a slow server pins a thread indefinitely and 60 such jobs stop every scan and the crawler's NLP calls.
- **RT-94** `MED` — IOC extraction runs inside the request loop, parses fetched page text of any size with slow patterns and loads a new model on every call, so one request to a large page stalls the whole single-process service.
- **RT-311** `MED` — The extension socket stores every acknowledgement message as a Redis key built from a request id the client chooses, of any length up to the 16 MB frame, for 90 seconds, with no size check and no check that the id is an open request; Redis has no memory lim …
- **RT-314** `MED` — RT-31 is incomplete: Unified Search masks only the password field of stealer-log rows for non-enterprise licences, so a member with the stealer-logs module gets card numbers and CVV values in clear, up to 1,000 rows per call; the normal stealer route masks …
- **RT-315** `MED` — The relative-time rewrite applied to crawled record text has a pattern whose cost grows with the square of the text length, and it runs inside the request loop on the breach list and on leak, market, social and general record routes; whoever controls text o …
- **RT-316** `MED` — RT-127 is incomplete: the public-target check does its DNS lookup in the shared worker-thread pool with no timeout, on routes with no request limiter (takedown create, IOC extract from URL, IP resolve, IP scanner, DKIM, phone lookup), so one analyst or memb …
- **RT-317** `MED` — RT-121 is incomplete: the extension socket reply limit counts characters, not stored bytes, and each user may hold 20 replies of up to 12 million characters for 300 seconds; replies are re-encoded with escaping before storage, so one 16 MB frame of two-byte …
- **RT-318** `MED` — The result-scope name for extension crawls is built from the request's type and cursor fields, which have no length limit, and is written to Redis about five times per request (sorted-set member, two key names, a value and the sealed command), so the same u …
- **RT-321** `MED` — A tenant maintainer can store an application name of any size (the only check is "not empty"; about 15 MB fits in one request), and that tenant's whole settings record is then read from Redis, parsed and written back to Redis on every settings lookup, inclu …
- **RT-18** `LOW-MED` — Any ordinary member can create, edit, dismiss and delete tenant alerts; bulk delete needs the maintainer licence but single delete does not.
- **RT-20** `LOW-MED` — Comments and reactions on a record show each author's user id and username to all tenants, ignoring the profile-hidden settings.
- **RT-21** `LOW-MED` — Evidence file id, hash and integrity status are accepted from the client on case create and update, so a swapped file still shows as verified, and the id goes unencoded into the storage address.
- **RT-22** `LOW-MED` — The case report picker searches breach, stealer-log, social and other sources with no licence check.
- **RT-23** `LOW-MED` — A failed custom-domain check still stores the unverified claim, which blocks every other tenant, including the real owner, from adding that domain.
- **RT-34** `LOW-MED` — Forced safe search for the demo account works on one route only; the other search routes call the check without the flag that turns it on.
- **RT-35** `LOW-MED` — /api/search/autocomplete returns record titles from breach, marketplace, social, chat, exploit, APT and malware sources with no licence check, and the user activity route does the same for title and preview.
- **RT-102** `LOW-MED` — A job timeout only marks the job failed: the worker thread, the Node runner and its browser keep running and the queue slot is freed, so runaway runs pile up and exhaust threads and memory.
- **RT-105** `LOW-MED` — The YouTube and Reddit takedown flows write the tenant's saved local storage into every origin and frame the browser loads, so third-party frames and the supplied impostor page can read it.
- **RT-111** `LOW-MED` — The stealer-log password-shape filter is applied to the real password before masking, so a non-enterprise user learns the exact length and character classes of every masked password by repeating the search.
- **RT-117** `LOW-MED` — RT-20 is incomplete: feedback is one shared record per document for all tenants, so comment text, username and user id of other tenants' analysts are shown unless the author hid their profile, although profiles are meant to be visible only inside the tenant.
- **RT-120** `LOW-MED` — The extension socket is authenticated once at connect and never re-checked, and logout closes only sockets on the worker that handled it, so a socket survives logout, password change, account disable and token expiry and keeps receiving commands, session co …
- **RT-124** `LOW-MED` — Stopping one's own scheduled social run sets one platform-wide stop flag, so one user halts the remaining daily runs of every other user and tenant for that day.
- **RT-125** `LOW-MED` — RT-10 and RT-60 are incomplete: assign-analyst changes who can open a closed case and clears its task assignments, and file download and verify re-save closed and archived cases.
- **RT-126** `LOW-MED` — A maintainer can send platform-branded mail from the platform mailbox to any non-free-mail address with up to 256 characters of free text in the password field, repeatedly, by creating and deleting users.
- **RT-127** `LOW-MED` — The SMTP test in tenant update and the custom-domain DNS checks run in the shared worker-thread pool with no limiter, so one maintainer sending parallel requests to a slow host can hold every thread for 20 seconds and stall mail, target checks, backups and …
- **RT-128** `LOW-MED` — Deleting a user or tenant leaves that account's saved social-session files, vault entries, data-protection keys, scan jobs, graph sessions and profile photo; user deletion leaves public chat shares live and tenant deletion leaves mailboxes.
- **RT-129** `LOW-MED` — A tenant's watch list is the whole domain of the one mailbox used at signup and the free-mail block list has 22 names, so an approved user with a mailbox at a shared provider or large employer receives stealer-log and breach alerts for every address at that …
- **RT-130** `LOW-MED` — Direct fetches in Micros read whole response bodies with no size cap (SEO fallback, IOC URL fetch, phone lookup, subdomain live check, app lookup, favicon).
- **RT-147** `LOW-MED` — Identity scoring can be steered by whoever controls the candidate profile: a "parody" or "unofficial" word in the bio forces "Monitor" and no takedown at any score, and the same handle elsewhere plus a link to the victim's site is rated an official account.
- **RT-306** `LOW-MED` — The ship feed is one platform-wide subscription whose map box is replaced by every ship request with no range check, so any geo-licensed account, and every analyst's automatic 8-second poll, narrows the live ship picture for all other users and tenants.
- **RT-307** `LOW-MED` — Places and areas under examination leave the platform to third parties with a User-Agent naming Orion: location text goes to public Nominatim as the analyst types, facility boxes go to public Overpass servers with a mail.ru mirror as fallback, and aircraft …
- **RT-19** `LOW-MED` — The public demo account can post comments and trust votes on any record, and every tenant's users see them.   ⟨note: DEMO ACCOUNT CANNOT COMMENT⟩
- **RT-33** `LOW-MED` — The days value on /api/insight and /api/insight/country has no bounds and is part of the cache key, so each new value runs eleven large queries and stores a large cache entry.
- **RT-36** `LOW-MED` — Feedback comments have no length limit, so one request can store megabytes on any record id, which every reader then loads.
- **RT-37** `LOW-MED` — Record routes make a blocking third-party translation call when lang is set, and semantic search a blocking embedding call, inside the request loop.
- **RT-59** `LOW-MED` — Any case participant, including an assigned analyst, can rewrite or delete other people's case comments, and the changed text stays under the original author's name.
- **RT-60** `LOW-MED` — RT-10 is incomplete: evidence files can still be uploaded to or deleted from closed and archived cases.
- **RT-62** `LOW-MED` — The DKIM raw-email parser runs a slow pattern over a header with no size limit, and Micros is one process, so one large request stalls all scans; 112 KB took 4.7 s on the pattern alone.
- **RT-103** `LOW-MED` — A takedown for a platform the runner does not implement writes the whole request (session cookies, reporter name and email) to the container log and returns success, which is recorded as submitted; Intelligence does not restrict the platform value.
- **RT-104** `LOW-MED` — Email recon runs holehe without its no-password-recovery option, so four modules start a password-recovery request for the investigated address, which can notify the person under investigation.
- **RT-116** `LOW-MED` — Legal item 4 is incomplete: the CTI graph stores and returns each record's feed source address, which the search, record and STIX routes strip.
- **RT-123** `LOW-MED` — Any member or analyst of the default tenant, not only its admin, sees every takedown request operated by the root tenant, which covers all standalone customer tenants: target, requester name, note and captured page evidence.
- **RT-133** `LOW-MED` — The rule that attaches the scan's session cookie or token matches any address that merely starts with the scanned origin, so a look-alike host with that prefix would receive the credential; the path that makes the scanner request such a host is inferred.
- **RT-61** `LOW-MED` — Identity resolution fetches shortener and link-page addresses found in a compared profile's bio and follows redirects, so whoever controls a bio can make Social request internal addresses.
- **RT-112** `LOW-MED` — RT-32 is incomplete: the STIX flattening pattern still costs 0.5 to 0.9 s per 20 KB title and the batch route takes 500 items, so a few batch requests degrade every worker for minutes.
- **RT-113** `LOW-MED` — RT-33 is incomplete: days is bounded to 1–3650 but is still part of the cache key, so any role, demo included, can cycle the values, each running eleven large queries and storing a multi-megabyte entry.
- **RT-114** `LOW-MED` — The vulnerability scan's sign-in username, password or key for the scanned site is saved in clear in the scan-job record and returned by the job routes until the user deletes the job.
- **RT-115** `LOW-MED` — Scan jobs have no cap on count or payload size, and the count, list, incomplete and create paths load all of a user's jobs with full responses, so one user can make the polled count route exhaust backend memory.
- **RT-121** `LOW-MED` — RT-17 is incomplete: extension socket replies are still written whole to Redis with no size check, and a user can open unlimited result scopes by varying the profile name.
- **RT-7** `LOW-MED` — Fixed: the SMTP password is never returned (field always empty) and is sealed at rest via SecretSealer.
- **RT-25** `LOW` — Any member can replace or remove the tenant logo, and the node endpoint returns the SMTP host, port and sender address to every role, demo included.   ⟨note: only maininaer should be able to do this⟩
- **RT-10** `LOW` — Smaller route gaps: failed custom-domain claim is saved, closed cases can be overwritten, app_name unescaped in docs and mails, internal error text returned, limiter is global not per user.
- **RT-12** `LOW` — The admin user-edit handler would copy any form field onto a user record if it worked.
- **RT-233** `LOW` — RT-10 is incomplete: the alert summary returns raw Redis exception text and the mail test returns the SMTP server's error text.
- **RT-237** `LOW` — The admin panel's user view lists and exports every field: password hash, email-verification token (stored unhashed), 2FA secret, recovery-key hash and session id.
- **RT-238** `LOW` — The guard against deleting system settings in the admin panel covers only the GET row action; the POST row action and the bulk delete still reach the panel.
- **RT-239** `LOW` — The tenant id in the export routes is used to build a backup path before it is validated; only the ownership check stops a non-admin, and no exposure was found.
- **RT-241** `LOW` — The log writer does not neutralise line breaks and the alert job logs watched values as-is, so a privileged-IOC tenant can plant forged entries in the system log viewer.
- **RT-248** `LOW` — The file-scan job id is built from user, file name and byte length only, so a different file with the same name and size gets the earlier verdict for up to five minutes.
- **RT-251** `LOW` — A wallet value is accepted on prefix and length alone and placed raw into the path of outside calls, one of which carries the platform's bearer token.
- **RT-257** `LOW` — The IP-scan value is passed to ping and the DNS tool with no IP check and no end-of-options marker; no impact found beyond errors.
- **RT-258** `LOW` — The wayback lookup uses plain HTTP, so the snapshot list can be altered in transit.
- **RT-259** `LOW` — Storage's user-id and file-name patterns accept a trailing newline; no cross-user reach.
- **RT-152** `LOW` — Automation changes the tenant's account beyond the request: the YouTube takedown creates a channel named "Time Orion" when none exists and the Pinterest publisher creates a board named "Posts".
- **RT-154** `LOW` — The post image download has no address check, follows redirects, reads the whole body with no size limit, and on failure posts a random picture from a public image service to the tenant's account.
- **RT-156** `LOW` — Identity resolution runs outside the job queue with no concurrency cap or timeout and shares the thread pool with recon and automation.
- **RT-162** `LOW` — Recon verdicts are cached per platform and username in a process-wide dictionary with no size limit or expiry, so memory grows and stale answers are served to all tenants until restart.
- **RT-187** `LOW` — RT-35 is incomplete: /api/search/suggest builds spelling suggestions from all eight indices with only a role check, so unlicensed sources leak single terms.
- **RT-199** `LOW` — Deleting a case leaves evidence files and captures in storage, and a case update that omits a file drops the reference without deleting the file.
- **RT-200** `LOW` — Evidence uploads trust the content type declared by the browser, keep any file name, and the 5-file cap is per request, so files and captures are unlimited per case.
- **RT-202** `LOW` — The public case share returns internal comments, tasks, linked case ids with reasons, and the user ids of authors, assignees and the closer.
- **RT-205** `LOW` — Share links are not tied to the creator's account: chat shares stay valid after the chat or user is deleted or disabled, case shares after the creator is disabled.
- **RT-208** `LOW` — Session capture accepts any platform name while the 10-session cap is per name, so a user can store unlimited session files.
- **RT-209** `LOW` — Session cookies that are encrypted at rest are written to Redis and published on the socket bus in clear while a command is in flight.
- **RT-213** `LOW` — The once-a-day manual post rule reads results written only after a run ends and manual triggers have no concurrency cap, so repeated calls publish several posts.
- **RT-218** `LOW` — Screenshot fetch walks the whole screenshot folder on every request inside the request loop, on a route the demo role can call.
- **RT-222** `LOW` — Stored social profile data has no size or count limit.
- **RT-226** `LOW` — The alert licence filter adds the tenant's licences to the user's, so a member holding only a free licence still receives paid-category alerts when the tenant holds them; may be deliberate.
- **RT-227** `LOW` — The mail domain is built from the last two labels of the custom domain, so tenants under a multi-part or shared suffix get the same mail host.
- **RT-234** `LOW` — INF-17 is incomplete: the public stealer-log search counts by forwarded address only, so all onion visitors share one daily quota of 35.
- **RT-240** `LOW` — RT-6 is incomplete: the tenant SMTP host is checked once at save and only in production, never at send, and any port is accepted.
- **RT-243** `LOW` — The connector callback and token refresh make blocking outbound calls inside the request loop.
- **RT-244** `LOW` — The account-created mail carries the initial password in clear text; the forced reset on first login limits it.
- **RT-247** `LOW` — If removing the scan's credential rule fails, nothing clears it later, so later scans of that origin by other users carry the first user's session.
- **RT-250** `LOW` — Google API keys travel in request addresses and the connection error text containing them is returned to the user or logged.
- **RT-252** `LOW` — Repository scan clones before checking size and into a folder inside the mounted source tree, so a very large repository fills the host disk.
- **RT-253** `LOW` — The cracked-app lookup fetches result links taken from third-party search pages on any host with redirects.
- **RT-255** `LOW` — Evidence capture takes a full-page screenshot and the whole page HTML with no size limit and keeps them in memory for five minutes.
- **RT-261** `LOW` — Storage caps each upload at 30 MB but has no per-user or total quota, and only mail files expire.
- **RT-275** `LOW` — The "published post" address is the first matching link on the page or the current page address, so another account's post or a feed-supplied link is stored as the tenant's own.
- **RT-276** `LOW` — The Reddit takedown builds a page selector from the impostor address text unescaped, so the request can decide which element is clicked in the tenant's signed-in session, and it clicks the first element whose text contains "me" and any button named accept o …
- **RT-282** `LOW` — Fourteen of fifteen platforms run headless Chromium with the automation flag visible, and a session without a stored user agent presents a Windows Chrome string on a Linux build, raising the chance the tenant's account is flagged.
- **RT-283** `LOW` — Monitoring results take the item address from the first absolute link inside the post or ad, so anyone posting a link in a monitored feed gets it stored as the post address.
- **RT-285** `LOW` — The like and view count patterns in every ad and hate-speech file are quadratic on long digit runs; reasoned, not timed.
- **RT-286** `LOW` — When no reporter e-mail is given, takedowns use an address on orion.com, not the product's domain, so platform replies go to whoever holds that mailbox.
- **RT-287** LOW (dormant) — With the skip-submit or screenshot switch on, takedowns save screenshots of the signed-in session into the mounted source folder and never delete them.
- **RT-26** `LOW` — The stored file name goes unescaped into the case artifact download header; feedback records are created for any id on read and vault entries have no count or size limit.
- **RT-38** `LOW` — A takedown request's report id is not checked, and its status is then written onto that shared defacement record for all tenants.
- **RT-39** `LOW` — The key lookup route has no role check or query limit, so the public demo account can list the email, user id and licences of platform-tenant users.
- **RT-44** `LOW` — A generated AI report can be downloaded by any signed-in AI user who has its file name; the owner is read from the name and not compared with the caller.
- **RT-64** `LOW` — The forum-profile search returns up to 100 full social records to users holding only the scanning licence, without the social module the other social routes need.
- **RT-66** `LOW` — The crawler account is not protected from user management: the update guard compares the role with "crawl" while the stored value is "crawler", and the delete guard blocks only "admin".
- **RT-67** `LOW` — RT-10 is incomplete: scan and lookup forwarders still return the raw upstream error and connection text, which can expose internal addresses.
- **RT-153** `LOW` — The Quora takedown rewrites every non-Quora cookie in the stored session onto the Quora domain, so other sites' cookies are sent to Quora.
- **RT-158** `LOW` — Avatar comparison downloads the whole body before its 5 MB check and decodes with the default pixel ceiling, so a small crafted image expands to hundreds of MB.
- **RT-159** `LOW` — No Social route limits input size: recon image upload, base64 ID photo, session state, post text and hate-speech post count are unbounded.
- **RT-163** `LOW` — Raw exception text, temp-file paths and browser error logs are returned as the job message and stored by Intelligence as the failure reason.
- **RT-180** `LOW` — The demo account sits in the platform's own tenant and may call the user profile and activity routes, so a visitor who takes a staff user id from a comment gets that person's email, role and licences.
- **RT-182** `LOW` — A user with the dismiss permission can push unlimited hashes of any length into the tenant record, which can reach the database document limit and break later tenant updates.
- **RT-183** `LOW` — The graph-session routes put graph_type from the body into the database filter untyped, so an object value acts as a query operator on the caller's own records, and distinct type names allow unlimited session records.
- **RT-184** `LOW` — The CTI graph batch route accepts any number of items and runs one graph query per value, each scanning up to 50,000 edges.
- **RT-185** `LOW` — The map-entities by-ids route needs only the general module while the list needs the geo licence, takes an unbounded id list, and ids are plain hashes of public name and coordinates, so a free user can fetch the geo dataset piecemeal.
- **RT-186** `LOW` — /api/cti/fetch makes a blocking 120-second call inside the request loop with unbounded text.
- **RT-188** `LOW` — RT-34 is incomplete: the threat-lens route uses its own copy of the demo safe-search check without the flag that turns it on; not reachable by demo today.
- **RT-196** `LOW` — An analyst can add a forged copy of a colleague's task under the original author's name by sending the same task id twice.
- **RT-197** `LOW` — The rule that analysts cannot view archived cases is enforced only on the list, so an assigned analyst still opens an archived case and its files by id.
- **RT-201** `LOW` — Case requests have no length or count limits on title, description, comments, tasks, entities or case id, so one member can grow a case toward the database document limit.
- **RT-206** `LOW` — Creating or revoking a case share and assigning analysts skip the view rule, so an admin who cannot open a case can still mint a public link to it.
- **RT-210** `LOW` — AUTH-12 is incomplete: the socket ticket is read and deleted in two separate Redis calls, so two connections opened together can both use one ticket.
- **RT-212** `LOW` — The daily social job runs stored profiles for any account that still exists without checking status or licence, so posting continues for disabled or downgraded users.
- **RT-217** `LOW` — The intake-crash fix is incomplete: the social, leak, stealer-log and swarm paths still raise unhandled errors on a missing title, missing hash or non-object body.
- **RT-223** `LOW` — The manage-profiles takedown body is a free-form object with no size limit, re-sent to Social every two seconds for up to 110 seconds.
- **RT-225** `LOW` — The takedown note and reply attachments have no size or count limit.
- **RT-228** `LOW` — Tenant restore and import pin licences, quotas and status from the live tenant but not the verified custom-domain flag or the AI-endpoint setting.
- **RT-229** `LOW` — AUTH-22 is incomplete: every user other than demo can store a preferences object of any size, which is loaded on each request and returned in the tenant user list.
- **RT-230** `LOW` — RT-33 is incomplete in a second place: the tenant's stored heat-map days value has no bounds and replaces the bounded query value.
- **RT-231** `LOW` — The alert "seen" route takes an unbounded list and compares each item against every stored alert inside the request loop.
- **RT-274** `LOW` — After a Quora or TikTok post the signed-in browser opens whatever address the first matching feed link holds, absolute addresses included.
- **RT-288** `LOW` — Five probes (AniList, LeetCode, Misskey, Roblox, Twitch) send the searched handle straight from the server's own address because the POST helper takes no proxy.
- **RT-157** `LOW` — The avatar address check does one IPv4 lookup and tests only private, loopback, link-local and reserved ranges, so 0.0.0.0, 100.64.0.0/10, the Micros 172.15.0.0/24 range, IPv6-only answers and a changing name all pass.
- **RT-161** `LOW` — 468 recon probes use plain HTTP through Tor exits, so exit operators see which usernames are investigated and can alter the answer.
- **RT-181** `LOW` — Reactions and comments are accepted for any document id of any length without checking the record exists, and the activity route then runs up to eight lookups per stored item.
- **RT-190** `LOW` — Several inputs raise unhandled errors (HTTP 500 and a traceback in the system log): lang on a record without a translated field, an unknown platform on consolidated or threat-lens search, a malformed user id on profile routes, a malformed address in STIX co …
- **RT-204** `LOW` — Case and chat share tokens travel in the page address, so they land in proxy logs, browser history and referrers.
- **RT-219** `LOW` — Breach and forum snapshots share one folder and one fetch function with no module check, so a user licensed for either can fetch the other's snapshot when the name is known.
- **RT-232** `LOW` — Alert scan start reads the running flag and then sets it, so parallel requests start several scans; custom-alert create and update and the mail-test route send mail through the platform SMTP with no limiter.
- **RT-235** `LOW` — The public config route returns the app version, whether the admin panel is enabled, the backup schedule flag and whether SMTP is configured, with no login.
- **RT-236** `LOW` — Settings saved through the public-config update go into one JSON value that skips the per-key validators, so a maintainer can store any address as the onion link and header links of any scheme and size.
- **RT-242** `LOW` — Account email uniqueness is a read-then-insert with no unique index, so parallel create or signup requests can produce two accounts with one address.
- **RT-246** `LOW` — RT-42's host check is missing from two optional logic checks and compares host name only, so sign-in data can go to another port or plain HTTP on the same host.
- **RT-256** `LOW` — The aircraft and ship caches keep one entry per requested map box and never evict.
- **RT-289** `LOW` — Per-platform probe patterns are slow beyond the shared parser: Plurk's title pattern is cubic (over 6 s at 4 KB) and six others quadratic.
- **RT-310** `LOW` — RT-67 is incomplete for satellite and live tracking: Micros returns raw upstream text (provider endpoint addresses, status, the first 200 characters of the upstream body, connection errors) and the client prints it in the map panels.
- **RT-24** `LOW` — Fixed: backup job status is scoped to the caller's tenant (job_status returns an idle record for other tenants), so no cross-tenant filename/error/id leaks.

#### Security (9)

- **SEC-1** `MED-HIGH` — /api/ioc/extract/url fetches any URL server-side, including internal addresses.
- **SEC-8** `MED` — /api/s/static/user|tenant/{id} accepts any bogus cookie and returns any user's photo or tenant's logo.
- **SEC-1b** `MED` — All scan and lookup forwarders pass user targets to micros with no internal-address block.
- **SEC-6** `MED` — GET /api/takedowns?q= puts raw input into a Mongo regex.
- **SEC-4** MED (non-prod) — Debug mode sets CORS "\*" together with credentials.
- **SEC-12** `LOW-MED` — Production mail is sent without verifying the SMTP server certificate.
- **SEC-9** `LOW-MED` — POST /api/index/swarm needs no login, so anyone can push data into the swarm queue.
- **SEC-7** `LOW` — A stored file name goes unescaped into a Content-Disposition header.
- **SEC-5** `LOW` — The IP-detail panel loads a scan-derived ip:port in an iframe.

#### Infrastructure & deployment (55)

- **INF-1** `HIGH` — Login lockout and per-IP quotas trust a caller-chosen X-Forwarded-For value.
- **INF-2** `MED-HIGH` — nginx serves the whole workspace/resource folder publicly: profile photos, tenant logos, encrypted session and case files.
- **INF-37** `MED-HIGH` — The basic ZAP scanner runs with its API key disabled, accepts API calls from any address and sits on the shared network, so any container there can read other users' scan traffic, sign-in requests included, and change or stop the scanner.
- **INF-5** `MED` — PRODUCTION=1 is not pinned in the production compose file; without it debug behaviour turns on.
- **INF-15** `MED` — Orion-Micros accepts every call with no credential and its compose file publishes port 8010 on all interfaces, so any container on the shared network can run scans without the licence, limit and internal-target checks; the port is closed from outside on pro …
- **INF-3** `MED` — Port 80 proxies the extension socket and the backups-status path in clear text.
- **INF-4** `MED` — The .onion site is reachable on public port 80 with a forged Host header.
- **INF-6** `MED` — Backup import accepts 4 GB uploads before authentication.
- **INF-7** `MED` — CI runs every push and PR on self-hosted runners with the full env secret on disk.
- **INF-26** `MED` — INF-19 was not applied to the mail stack: the mail web container, which parses inbound mail, receives the whole .env including the Cloudflare DNS token and the GitHub deployment token.
- **INF-27** `MED` — The Micros and Social containers run as root and drive Chromium with its sandbox off against outside pages, with the full .env and writable mounts of their own source, so a browser flaw gains root in the container and every secret.
- **INF-18** `LOW-MED` — The backend port and nginx's internal listener are reachable from every container on the shared network and trust the client address those callers supply.
- **INF-19** `LOW-MED` — The whole .env, including the GitHub deployment token, is passed into the web and Redis containers.
- **INF-29** `LOW-MED` — A deliberately vulnerable demo application (Juice Shop, latest tag) starts with the production Micros stack on the same network as the scanner, Redis and ClamAV.
- **INF-28** `LOW-MED` — INF-1 was not applied to the mail block: mail nginx keys its limits on the edge proxy's address, so all webmail users share one budget of 20 requests per second and 50 connections.
- **INF-50** `LOW-MED` — At start-up Micros downloads translation model packages from a fixed Google Drive link and the GeoLite file from third-party GitHub latest releases with no checksum.
- **INF-17** `LOW-MED` — All onion visitors reach the backend with one address, so one visitor's failed logins lock login for every onion user and they share the signup and reset quotas.
- **INF-52** `LOW-MED` — The ship-tracking connection turns certificate checking off, so someone on the path can read the API key and feed false ship positions.
- **INF-30** `LOW-MED` — Besides Micros 8010, the Tor SOCKS and control ports, the Social API and the tool server port 8300 are published on all host interfaces and rely only on the host firewall.
- **INF-49** `LOW-MED` — The backend loads its licence rule table, URL rules and mail templates from the served frontend build folder with no integrity check, so any file write into that folder (see INF-33) becomes a licence or role change or server code execution at the next start.
- **INF-51** `LOW-MED` — The tracked Micros env template holds what look like working OpenSky, AISStream and Sentinel Hub credentials; not tested.
- **INF-54** `LOW-MED` — Fixed & prod-verified: the Elasticsearch node runs with xpack.security.enabled=true.
- **INF-16** `LOW-MED` — Fixed: the edge sets limit_conn / limit_req and a client body timeout (nginx-prod.conf).
- **INF-21** `LOW` — Elasticsearch and Redis passwords still appear in process arguments through the health check and start command.
- **INF-9** `LOW` — Mongo starts about 10 seconds without auth on each start; DB passwords are visible in process arguments.
- **INF-11** `LOW` — run.sh makes workspace folders world-writable on every run.
- **INF-13** `LOW` — The image build can bake backups and key files into layers; two pip packages are unpinned.
- **INF-14** LOW (dev only) — Dev and test compose files publish databases on all interfaces.
- **INF-88** `LOW` — Storage writes no access log in production, so reads, deletes and rejected tokens leave no trace.
- **INF-91** `LOW` — Storage's base, Elasticsearch and curl images are pinned by tag only.
- **INF-35** `LOW` — Several builds fetch unpinned or unverified content: Social pipes a remote setup script into the shell and uses a third-party Tor image on latest; Micros uses unpinned base, ClamAV and Juice Shop images and an unchecked subfinder download.
- **INF-70** `LOW` — Session cookies, the full takedown payload and the decoded ID photo are written in clear to /tmp and removed only in a finally block, so a killed process leaves them behind.
- **INF-72** `LOW` — Social's image build installs 15 of 16 Python packages and the global playwright npm package with no version pin.
- **INF-83** `LOW` — Lookup queries (usernames, emails, ID and phone numbers) are written to world-readable logs and results are stored in Redis for an hour though nothing reads them.
- **INF-89** `LOW` — Mail-type files in Storage skip both the malware scan and encryption at rest.
- **INF-90** `LOW` — The breach-dump runbook and parsers extract untrusted archives as root with no size or count limit; operator procedure.
- **INF-95** `LOW` — Several secrets have no "must be set" guard, so an empty value silently starts the service without one: Redis in Micros and the ArangoDB root password.
- **INF-97** `LOW` — The deploy "pull" command runs a hard reset and an untracked-file clean; Social's saved-session and raw folders are untracked and not ignored, so they are deleted on pull and can be committed by mistake.
- **INF-99** `LOW` — Intelligence CI installs client packages without enforcing the lockfile on a floating Node version, force-removes every container, network and volume on the runner, and uploads full container logs of a stack started with the env secret as artifacts.
- **INF-103** LOW (dev machines) — Development stacks publish on all interfaces: the Intelligence dev site and Mailpit (all dev mail, reset links included).
- **INF-20** `LOW` — The certificate check route for custom domains is public and unthrottled; each unknown TLS name triggers backend lookups and it confirms whether a domain is a customer.
- **INF-8** `LOW` — The production build ships source maps.
- **INF-12** `LOW` — Some nginx locations drop HSTS and nosniff; the TLS cipher list is weak.
- **INF-86** `LOW` — The Micros API container is given NET_ADMIN, which nothing in the code needs.
- **INF-104** LOW (dev machines) — Scripts use fixed names under /tmp for pid files, logs and a runtime folder, and the docs script will run a Python interpreter from a /tmp path if one exists.
- **INF-33** `LOW` — The Intelligence web container mounts the whole workspace writable, including the served frontend build and the extension packages, so any file-write flaw becomes a lasting change for every tenant.
- **INF-69** `LOW` — Social's API docs, schema and health routes answer without the internal token, so anyone who reaches port 8020 reads the full internal route and field list.
- **INF-71** `LOW` — Social's tracked Tor config holds the control-password hash, binds the control port on all addresses and accepts SOCKS clients from every private range; whether the hash matches production was not checked.
- **INF-84** `LOW` — INF-19 was not applied to Micros: the Redis and Tor containers receive the whole .env.
- **INF-85** `LOW` — INF-21 was not applied to Micros: the Redis password and the full-scan ZAP key appear in process arguments and the health check.
- **INF-87** `LOW` — Micros serves its interactive docs and API schema without the service token.
- **INF-93** `LOW` — INF-18 is incomplete on reachability: the onion listener is the only server on its port, so Tor visitors can address the main site or any tenant host through it, the edge limits are off there, and the dot-file and traversal blocks are absent.
- **INF-10** `LOW` — Fixed & prod-verified: the backend container runs cap_drop ALL, a read-only rootfs and a PID limit.
- **INF-22** `LOW` — Fixed: backups are encrypted at rest (ExportCipher.encrypt_stream) and log files are written 0640, not world-readable.
- **INF-23** `LOW` — Fixed: Dark-Nexus calls carry the internal service token (InternalServiceAuth.nexus_headers); the plain HTTP hop is internal-only on the private network.

#### Web client (69)

- **CLI-71** `MED-HIGH` — Confirmed on the live site (headers only): a static data file shipped with the client is served with no login and holds a slice of indexed intelligence, about 2,280 email addresses, phone values, card or ID-like numbers, person names and dump links, data th …
- **CLI-1** `MED` — CSV exports do not neutralise formulas in crawled text.
- **CLI-2** `MED` — One result card renders raw crawled text as HTML, allowing remote images and fake links (dashboard-results-general.component.html:46).
- **CLI-10** `MED` — The /admin policy allows inline scripts and framing from anywhere, which browsers honour over the frame header set later, so any same-site host such as a tenant subdomain can frame the admin panel with the admin's session.
- **CLI-16** `MED` — Opening a link to the network-intel or entity-lookup pages with a target in the query string starts the work with no click, so any site or message can make a signed-in analyst run host recon, port scans, subdomain enumeration, paid lookups and text analysis …
- **CLI-73** `MED` — The full posting script for managed social profiles (11,662 persona sets with captions and image addresses, personas aged 13–17 included) is a public static file, so a platform or a target can fingerprint Orion-run accounts by their captions.
- **CLI-114** `MED` — Case ids (free text chosen by the creating member, up to 64 characters) and artifact ids (chosen by any case participant, analysts included) are stored as sent, and the web client puts them unencoded into every case write address; an id holding a backslash, …
- **CLI-9** `MED` — Fixed & prod-verified 2026-11-19: /docs, /redoc, /openapi.json now carry the full CSP + X-Frame-Options in prod.
- **CLI-11** `LOW-MED` — The markdown renderer turns relative links in untrusted text (report content, AI chat output) into same-site links that open with no confirmation.
- **CLI-12** `LOW-MED` — The content policy is chosen by the first page loaded and holds for the whole session, and the social-intel page's policy allows connections, images and frames to any host, so entering through that page removes the limits everywhere.
- **CLI-19** `LOW-MED` — A link to the social-intel page carrying a profile and platform adds it to the analyst's results and immediately asks the extension to fetch it, with the handle placed unencoded into the platform address.
- **CLI-22** `LOW-MED` — Opening a consolidated Deep Search link starts site scans, a repository scan and social, user and app lookups against the target in the address with no click; the standalone scan page does the same.
- **CLI-56** `LOW-MED` — "Sync all connections" takes every item address found in already-crawled feed data and sends each as the next crawl target through the extension with no click and no check that it belongs to the platform, so crawled content can steer the analyst's browser t …
- **CLI-4** `LOW-MED` — The ArcGIS script is loaded from a CDN at runtime without integrity.
- **CLI-17** `LOW-MED` — Route parameters (report id, scan id, feedback document id, export type) are decoded by the router and placed unencoded into API paths, so a crafted in-app link makes a signed-in user's browser send an authenticated request to another same-site API path and …
- **CLI-21** `LOW-MED` — Route values are placed unencoded into API paths on two write calls (welcome password confirmation, alerts "delete all"), so a crafted link can point the confirmation at a different same-site API path; the user must still confirm.
- **CLI-74** `LOW-MED` — The PDF export's removal of Cookie, Authorization and API-key header values skips the findings register, so test request and response blocks carry session cookies and tokens of the scanned site into the PDF; JSON and CSV exports are never redacted.
- **CLI-75** `LOW-MED` — The phone lookup and DKIM checker start a scan on page load from the q value in the address.
- **CLI-14** `LOW` — Vault passwords are encrypted without binding to their site or username, and autofill picks the record by the plain stored address, so whoever can alter stored records can have a password filled on a host of their choice.
- **CLI-29** `LOW` — The markdown renderer has two quadratic-time paths on untrusted report and AI text with no size cap, so one crafted record freezes the analyst's tab; 20,000 links took 17.6 s.
- **CLI-31** `LOW` — Changing the data-protection passphrase re-wraps the same private key and vault key with the same salt, so an old database backup plus the old passphrase still opens every current and future vault entry.
- **CLI-32** `LOW` — A recovery key used to reset the data-protection passphrase stays valid afterwards and no new key is issued.
- **CLI-33** `LOW` — Logout ignores a failed server call: the UI shows the login page while the session cookie stays valid until expiry.
- **CLI-34** `LOW` — Logout is not passed to other open tabs; they keep showing data and hold the unlocked vault key until their next request fails, up to two minutes.
- **CLI-39** `LOW` — The same unencoded pattern on read calls lets a crafted link make pages load a different same-site API response: user activity, backup contents, case details, shared chat and shared case.
- **CLI-44** `LOW` — Threat Lens starts a 500-address IoT and camera detection run over a 12,000 km radius whenever the view loads, and the address can open that view.
- **CLI-45** `LOW` — Legal item 3 is weakened in the client: the raw stealer line with the password is shown in full under the masked field, the reveal confirmation is remembered after one yes, and the consolidated page shows stealer passwords in clear with a copy button (root- …
- **CLI-46** `LOW` — The credentials page runs a stealer-log search on load from the address, and the last search term (card number, email, password filter) is written into the address.
- **CLI-52** `LOW` — The file scanner puts the uploaded file's name into the address and checks only size, not type.
- **CLI-54** `LOW` — The extension privacy page says all data is sent only over an encrypted connection, while the product also runs on plain-http origins (legal/extension-privacy/extension-privacy.component.html:92, :156).
- **CLI-57** `LOW` — Opening a social profile runs a stealer-log search with no click and copies the returned records into the user's stored social profile document, so credential records stay readable from the social store after a licence change.
- **CLI-64** `LOW` — The vault has no idle auto-lock, revealed passwords stay on screen until hidden by hand, and a copied recovery key is never cleared from the clipboard.
- **CLI-65** `LOW` — Opening a case communication grants vault autofill and capture rights for that case for the rest of the login, even after the capture window closes.
- **CLI-70** LOW (legal and safety) — A persona can be declared as age 13–17 and bound to a real social account for automated posting and ad monitoring.
- **CLI-8** `LOW` — Two image tags load third-party images without the no-referrer setting used elsewhere, which sends the tenant's address to the image host (manage-profile-results.component.html:228, social-user-graph.component.html:45).
- **CLI-5** `LOW` — Vault autofill offers saved logins to related hosts and over http.
- **CLI-7** `LOW` — Smaller client gaps: auto-loaded remote thumbnails, links without rel=noopener, results cached in sessionStorage, no role route guards.
- **CLI-15** `LOW` — CLI-5 is partial: the autofill host match ignores the port, and a login saved without a scheme is still offered on plain http.
- **CLI-28** `LOW` — The external-link confirmation covers only http and https links; any other scheme in a record or lookup result goes straight to a window open with no prompt.
- **CLI-30** `LOW` — CLI-1 is incomplete: the shared CSV helper quotes a field only when it has a comma, quote or line break, so a value with a semicolon or tab followed by a formula can become its own formula cell in some spreadsheet locales; locale behaviour inferred.
- **CLI-35** `LOW` — The markdown renderer does not strip its own placeholder characters from input, so untrusted text can get link markup substituted inside another link's attribute; Angular's sanitiser is the only remaining barrier.
- **CLI-38** LOW (dev only) — The Angular dev server accepts any Host header and its proxy skips certificate checks.
- **CLI-40** `LOW` — SEC-5 is incomplete: the IP-detail camera frame still loads the scanned host's live page automatically with no sandbox; the default content policy blocks it except in a session that started on the social-intel page (root-searches/network-intel/ip-detail/ip- …
- **CLI-41** `LOW` — Crawled addresses are bound to links with no limit on the scheme, so a record can carry an app-protocol link, and these skip the external-site confirmation (intel-panel/dashboard-results/dashboard-result-chat.component.html:105, report/templates/report-chat …
- **CLI-42** `LOW` — Homepage "latest" cards paste the crawled title unencoded into the report address, so a crafted title can add parameters to the report route, the language one included.
- **CLI-43** `LOW` — Satellite map tiles are requested from Esri and OpenFreeMap with the default referrer, so those hosts receive the tenant's address with the area being examined.
- **CLI-59** `LOW` — Deleting a captured login session always reports success even when the server call fails, so live cookies the user believes are gone can remain stored.
- **CLI-66** `LOW` — The profile-address check accepts any host beginning with "pinterest." or containing ".pinterest.", any host for a platform without a rule, and plain http.
- **CLI-76** `LOW` — CLI-6 is incomplete: phone lookup results show the "official portal" and web-mention addresses as plain links that skip the external-link confirmation (client/src/app/sections/api/phone-lookup/phone-lookup.component.html:101, :150).
- **CLI-78** `LOW` — The lang value from the address is appended unencoded to the report API query, so a link can add parameters and make the server translate the record through the third-party translator on open.
- **CLI-82** `LOW` — The automated takedown form accepts any address whose host merely contains the platform name, and shows the "Orion Mail" badge for any reporter address containing "@mail.".
- **CLI-83** `LOW` — Outgoing mail replaces the literal word "appname" across the whole rendered body after untrusted values are inserted, so a domain or username containing that word is rewritten in alert and takedown mails.
- **CLI-89** `LOW` — A record value that looks like an image data address but is malformed makes the PDF builder throw, so one crafted crawled field stops the export of that report; not run.
- **CLI-93** `LOW` — The results insight panel runs an email pattern with quadratic cost over crawled content with no length cap, so a long crafted record can freeze the results page.
- **CLI-95** `LOW` — Copy buttons put raw crawled strings, line breaks and control characters included, on the clipboard.
- **CLI-101** `LOW` — The Data Collection Overview is guarded as an admin or maintainer screen in the browser only, while its API returns platform-wide source group names, health counts and index sizes to every role, the demo account included.
- **CLI-109** `LOW` — Evidence uploads are checked for count and browser-reported type but not size, and the social image scan posts any chosen file with neither a size nor a type check.
- **CLI-110** `LOW` — The chat and social report puts the record's screenshot name unencoded into the snapshot API path.
- **CLI-61** `LOW` — Restoring, importing or deleting a full platform backup and deleting a tenant need only a yes/no popup, with no password.
- **CLI-68** `LOW` — The production origin address is hard-coded in the public bundle, so anyone who downloads the script learns the address behind the CDN.
- **CLI-77** `LOW` — Search filters, Safe Search included, are read from the address at start-up and sent to the server, so a link can open a General search with Safe Search already off.
- **CLI-80** `LOW` — When a session expires, the previous user's session data, last search results and running scan pollers stay in memory for the next person signing in on the same tab.
- **CLI-85** `LOW` — The licence table and its enforcement have drifted: some flags and modules are checked by no route, while routes check modules the table never grants; no over-grant found.
- **CLI-87** `LOW` — The demo tour pre-fills a named person's corporate email as the search target and auto-submits searches and a network lookup against a third party's domain.
- **CLI-90** `LOW` — The CTI Graph route checks only the graph licence and never receives the caller, so documents and extracted entities of every cluster are returned regardless of the caller's module licences.
- **CLI-91** `LOW` — Search filter values (emails, domains, names under investigation) are kept in local storage and survive a session that ends by expiry or a browser close, so the next person on that browser inherits and silently applies them.
- **CLI-6** `LOW` — Fixed: external/clearnet links now go through a confirmation dialog (ProxyController) and open with noopener/noreferrer.
- **CLI-58** `LOW` — Fixed: the social-data route admits only ADMIN/MEMBER/ANALYST with the scanning licence; the demo role is not accepted.
- **CLI-92** `LOW` — Fixed: Threat Lens loads country boundaries from local /assets/data/map/world.json and the ArcGIS assets locally, not Esri's hosted service.

#### Browser extension (36)

- **EXT-2** `HIGH` — On Chrome, saving a session collects the login cookies of every site in the user's normal browser profile, not only the captured site, and uploads them.
- **EXT-1** `HIGH` — Any web page can drive the browser extension: run crawls, write cookies, open session capture, register itself as an Orion site.
- **EXT-3** `MED-HIGH` — A stored session is replayed without checking that its cookies and start address belong to the captured site, so a case participant can plant cookies for unrelated sites in a colleague's browser.
- **EXT-8** `MED` — EXT-2 is incomplete: on Chrome, saving a session for a site under a multi-part suffix (co.uk, com.au, github.io) collects and uploads the cookies of every site under that suffix.
- **EXT-14** `MED` — On Chrome the capture window runs in the normal profile and, when a saved session is supplied, nothing is wiped first, so Save session uploads the site's whole local storage, session storage and IndexedDB, the user's own login tokens included, into a sessio …
- **EXT-15** `MED` — Post commands publish from whatever account is signed in in the normal profile: on the socket path the supplied session is never applied and on the page path an empty session object passes the check, so script in the Orion page or the server can post from t …
- **EXT-9** `LOW-MED` — EXT-3 is incomplete: the crawl, crawl-all and post messages still write a supplied session's cookies for any domain into the default profile; the web client never sends one, so this needs script running in the app page.
- **EXT-10** `LOW-MED` — The extension's tab, fetch, verify-window, socket-ticket and run-task handlers accept messages from any extension context without the sender check the crawl and session handlers use.
- **EXT-11** `LOW-MED` — EXT-6 is incomplete: the capture card and vault menu are ordinary elements in the captured page, so that page can restyle them so a real click lands on Save session or a vault choice.
- **EXT-16** `LOW-MED` — On Chrome, opening a capture with no saved session clears local storage, IndexedDB, caches and service workers of the target site in the user's normal profile before any click and never restores them.
- **EXT-17** `LOW-MED` — EXT-3 is incomplete: seeded cookies are accepted for any sign-in provider domain (Google, Facebook, GitHub, Microsoft, Amazon and others) whatever the captured site is, so on Chrome a stored session can put someone else's provider login into the normal prof …
- **EXT-18** `LOW-MED` — Crawl commands are not checked against the platform's host: several collectors open or fetch the supplied address or a host built from the username, so Orion-page script or the server can make the normal profile request any address, internal ones included, …
- **EXT-19** `LOW-MED` — Any page can raise the "asks to be added as an Orion site" prompt in the popup; approval gives that origin full app rights, and on the unpacked Chrome build before first sign-in it becomes the selected site, so the popup sends the Orion password to it.
- **EXT-20** `LOW-MED` — The verify window keeps the user's own cookies in one global backup and does not restore it on the error path, and a second verify overwrites it, so on Chrome the user's cookies are lost and the stored session's cookies stay in the normal profile.
- **EXT-21** `LOW-MED` — In evidence captures the list of saved vault usernames for the site is requested with no click as soon as a password field is visible and written into the page, so the investigated site can read which accounts the analyst holds there.
- **EXT-22** `LOW-MED` — Seeded and captured web storage is never removed: after a verify or seeded capture the stored session's tokens stay in the site's storage in the normal Chrome profile, and in the Firefox container they can be saved into another case's session.
- **EXT-4** `LOW-MED` — The extension marks every page as installed and answers any site's ping with its version, so a site under investigation can tell the visitor is an Orion analyst.
- **EXT-5** `LOW-MED` — The saved extension token is not tied to the site that issued it, so after switching sites it is sent to the new one.
- **EXT-6** `LOW-MED` — The screenshot and record buttons on the capture card accept scripted clicks, so the captured page can file screenshots as case evidence.
- **EXT-39** `LOW` — The packaging scripts accept plain http for update and package addresses; current value is https.
- **EXT-12** `LOW` — The 30-day extension token is kept in extension storage that the bridge script injected into every page can read.
- **EXT-13** `LOW` — EXT-1 and EXT-4 are partial: every one-label sibling of the platform host and any localhost port is treated as an Orion site with no popup approval and receives the extension's presence and version.
- **EXT-23** `LOW` — EXT-2 is incomplete for provider domains: provider cookies that reappear in the normal profile while the window is open are collected and uploaded with the session.
- **EXT-24** `LOW` — Any page can make the extension call the Orion session endpoint with the user's token as often as it likes and keep the worker awake.
- **EXT-25** `LOW` — The sender check for crawl, post and session relies on the selected-site value in extension storage, which the bridge content script itself writes, so code in any page's content-script context can make its own origin the Orion site.
- **EXT-26** `LOW` — The evidence screenshot, chunk, notify, socket-state and activity handlers accept any extension context, take the case and communication ids from the message, and buffer chunks with no size cap.
- **EXT-27** `LOW` — On Chrome the backup of the user's cookies during capture and verify exists only in worker memory, so a crash, reload or update leaves the user signed out and the captured account's cookies in the normal profile.
- **EXT-28** `LOW` — The Add site flow accepts any origin, plain http included, after which the 30-day token, the password and socket commands travel unencrypted.
- **EXT-29** `LOW` — The list of the last 40 crawl targets stays in extension storage after sign-out and site switch.
- **EXT-31** `LOW` — EXT-4 is incomplete for capture, verify and visible-crawl pages: Orion-named elements are added to the investigated page, so it can tell it is viewed through Orion.
- **EXT-32** `LOW` — The address bar drawn inside the capture window is an ordinary page element, so the captured page can cover it or change the host it shows.
- **EXT-33** `LOW` — Outside evidence mode, seeded storage is written into whatever origin the tab landed on, so if the saved address redirects elsewhere the session's tokens go to that origin.
- **EXT-34** `LOW` — When exactly one vault login matches the host, the password is filled with no click on any page of that host showing a password field.
- **EXT-35** `LOW` — The capture navigation guard treats the last two labels as the site, so a captured page under a multi-part suffix can move the window to any other site under that suffix; same root as EXT-8.
- **EXT-37** `LOW` — Captured storage has no size limit, so a captured page can make Save session exhaust memory or fail.
- **EXT-38** `LOW` — The visible TikTok crawl window takes focus back every second, so keys typed for another window can land in the crawled page.

#### Orion Mail (18)

- **MAIL-1** `HIGH` — A user can register any address at the mail domain as a "disposable" identity, so they can send as another user or a role address, signed by the platform, and receive mail for role addresses and for users who have not yet set up a mailbox.
- **MAIL-2** `MED-HIGH` — Each inbound message beyond the 100-message cap permanently deletes the mailbox's oldest stored mail from any folder, so anyone on the internet can erase a user's older mail, sent and archived included, by sending about a hundred messages.
- **MAIL-47** `MED` — After the HTML sanitiser runs, message HTML is parsed again and every style block is rewritten by a pattern that deletes text and writes the result back raw, so a hostile mail can make that deletion form a closing style tag followed by markup the sanitiser …
- **MAIL-3** `MED` — Inbound mail whose subject or body contains "abuse", "copyright", "takedown", "dmca" or "infringement" is tagged as takedown mail for every mailbox and then hidden from folders and search, so ordinary mail silently disappears.
- **MAIL-4** `MED` — The chat user search returns username, mail address and internal id of every active mailbox on the platform to any signed-in user, across all tenants.
- **MAIL-6** `LOW-MED` — Images with same-site relative addresses in a message load automatically with the reader's cookies, so a message can call GET routes of the mail API.
- **MAIL-13** `LOW-MED` — The public expiring-message open route runs a 200,000-round password hash on the request loop with no attempt limit, so anyone holding one valid link can stall the workers.
- **MAIL-14** `LOW-MED` — Drafts and scheduled-send packages are stored with no count limit and no quota check.
- **MAIL-21** `LOW` — SPF, DKIM and DMARC verdicts are taken by a loose first-match pattern over the whole results header, which contains sender-chosen text.
- **MAIL-31** `LOW` — The expiring-message token and shared-drive file id go into request paths unencoded.
- **MAIL-37** `LOW` — Folder, label and thread listings return and decrypt every message when no limit is given.
- **MAIL-39** `LOW` — Chat messages to any platform user have no rate limit.
- **MAIL-42** `LOW` — INF-11 was not applied to mail: run.sh falls back to making the attachment folders world-writable when chown fails.
- **MAIL-44** `LOW` — INF-12 was not applied to mail: neither mail nginx nor the backend sets HSTS.
- **MAIL-58** `LOW` — Opening a message downloads and decodes every attachment whose declared type starts with image/, SVG included, with no size guard, so a crafted image can hang the tab each time.
- **MAIL-66** LOW (dev only) — The mail dev server listens on all interfaces, accepts any Host header and has CLI analytics switched on.
- **MAIL-67** `LOW` — On any network failure every mail user is told to open the developer address http://mail.localhost:4200, and other errors show the server's raw text.
- **MAIL-55** `LOW` — Fixed & prod-verified: sanitize_header_value strips RTL overrides and zero-width characters from mail headers.

#### Other services (1)

- **OTH-66** `LOW` — Sandbox firewall reload deletes the table and then loads it, leaving a short window with no containment rules.

#### Dependencies (3)

- **DEP-2** `LOW` — Only direct Python packages are pinned, so the image keeps whatever transitive versions its last build pulled; the local image has certifi 2026.7.22 with distrusted roots, dnspython 2.8.0 and soupsieve 2.10.0.
- **DEP-1** `LOW` — pymongo 4.18.2 has a crash bug; upgrade to 4.18.3.
- **DEP-3** `LOW` — The extension's @angular/router 22.1.4 and its build tooling have published advisories; the router is fixed in 22.2.1.

#### Product, data & other (83)

- **#1** — A search for an ordinary company name in General Intelligence returned a page advertising child sexual abuse material, and Safe Search was off by default.
- **#2** — Leak records showed raw download links together with the archive passwords to every analyst.
- **#3** — Stealer-log credentials were shown as plaintext URL:login:password to any analyst, with no masking or role gating, which raised GDPR questions.
- **#4** — A signed PhishTank download URL is stored inside records.
- **#9** — The UI hit unhandled 500 errors.
- **#10** — The version label was inconsistent: Account showed 1.0.3.15 and the sidebar showed 1.0.3.8.
- **#12** — Public contacts were a personal Gmail address, a Lahore office and a Sydney PO box, with no named legal entity or SLA.
- **#15** — All four top "latest" leak cards were re-crawls of old posts stamped as today, for example TMA Group (Apr 2025), Prefix Corp (18 Sep), U.S. Electrical Services (21 Sep) and Calmont (8 Nov).
- **#17** — Counter labels were wired to the wrong fields: "Languages Tagged" showed email_document_count, "Organizations Tagged" showed phone_document_count and "Countries Tagged" showed clearnet_document_count.
- **#18** — One weekly-change figure read 8,002%.
- **#19** — The leak tile said 95,422 documents while search reported 69,043.
- **#35** — Bluesky posts from 9 Nov showed as 8 Nov; social dates were a day early because of a timezone bug.
- **#36** — Record text kept stale relative times such as "6m ago".
- **#38** — 464 of 500 netflix.com lines have no date, so there's no way to filter for "new since last week". …
- **#40** — The default "Match any term" mode splits domains on the dot: citicpacific.com returned 15 unrelated \*.com records, while all-terms mode finds the 6 correct ones.
- **#41** — The Social index ignores all-terms mode: "Binance ShinyHunters" returned 18,912 hits.
- **#47** — The intrusion-set is named Ransomware_live (the scraped source) instead of Global Secret Group.
- **#48** — The victim is modelled as attacker infrastructure ("uses" infrastructure Heartland Honda).
- **#49** — The malicious-activity indicator includes the victim's own domain, its Microsoft 365 MX host, \_spf.google.com, spf.protection.outlook.com, sendgrid.net, enom.com and ransomware.live.
- **#32** — Compromise Monitoring files Phishtank, BlocklistProject, hack-db mirrors and Turkey's USOM URL list under "Defacement".
- **#33** — Actors & Malware lists Phishtank and OpenPhish as "defacement actors".
- **#60** — News Feed "Tracking" items are old 2014–2025 regulatory filings.
- **#54** — Text Analysis is broken: "ocr_classifier/analyze_text failed: Not Found".
- **#53** — DKIM Lookup said xcitium.com has no SPF and no DMARC; public DNS shows both.
- **#52** — Crypto Scanner rated the WannaCry ransom wallet "Low" risk.
- **#55** — Software Scanner is thin: 2 PreDB entries for Adobe Photoshop.
- **#50** — Host Recon returns only A and AAAA records.
- **#51** — IP Scan is noisy: 178 version-matched CVEs for scanme.nmap.org.
- **#20** — Records aren't de-duplicated: TMA Group has 15 copies (Sarcoma pages re-ingested on 7 dates) and UNIC Technology has 6 (Krybit mirrors plus ransomware.live).
- **#27** — 5 of the 27 hits only appeared after rewording the query ("MCS Steel", dropping ".com") or switching to strict match.
- **#45** — CVE-2024-27834, added to CISA's Known Exploited Vulnerabilities list on 4 Nov, returned 0 results.
- **#46** — CVE-2026-58644 returned only 3 items.
- **CASE MANAGEMENT** — Case details "Created by" shows raw user ID 694c79694368d50009a54ee9.
- **CASE MANAGEMENT** — next-id is count+1; after a case is deleted the suggested ID can already exist.
- **CASE MANAGEMENT** — Case List: sort dropdown and Actions column cut off at 1440 px.
- **CASE MANAGEMENT** — Status Tracking Board: 4th lane cut off at 1440 px.
- **AUDIT LOG** — Pagination ignored without a date range (pages 2+ return page 1).
- **REPORT PAGES** — Exploit report shows raw Markdown (###, ---).
- **REPORT PAGES** — Exploit description cut mid-word with no ellipsis.
- **REPORT PAGES** — Exploit report shows "Status: Inactive" for a GitHub advisory and "Extra: None".
- **REPORT PAGES** — Social Mapper: 3 Content-Security-Policy inline-style errors.
- **REPORT PAGES** — world.json (719 KB) downloaded on every app start.
- **STIX** — General export: Identity named "Password".
- **STIX** — Exploit export: Identity named "with".
- **STIX** — Telegram export: report names contain raw JSON and HTML anchors.
- **STIX** — URL indicators include crawler session IDs (sid=...).
- **STIX** — Social STIX export returns HTTP 500 for profile records without a post link (Pinterest profile owasp).
- **API** — GET /api/takedowns without parameters returns 3.67 MB.
- **API** — GET /api/public/case-shares/{x} without token returns 422 instead of 404.
- **API** — System logs contain raw Telegram credential-channel messages ("mail:pass").
- **API** — robots.txt on the app domain allows all crawlers.
- **PUBLIC WEBSITE** — /contact, /about, /privacy render a 404 page.
- **PUBLIC WEBSITE** — Footer "© 2025"; no legal entity; no SLA.
- **PUBLIC WEBSITE** — Lahore office, +92 phone and St. Petersburg FL office still listed.
- **PUBLIC WEBSITE** — GitHub link points to a private repo (404).
- **PUBLIC WEBSITE** — "Get Access" opens a personal Calendly; Instagram link is a personal account.
- **PUBLIC WEBSITE** — No SLA published (home, about, contact, privacy, pricing).
- **PUBLIC WEBSITE** — No Terms of Service page (/terms shows 404; nothing links to it).
- **PUBLIC WEBSITE** — Homepage and About still claim "UPTIME 99.98%".
- **PUBLIC WEBSITE** — Published SLA is 99.5% monthly uptime; own status page shows 96.7% overall for the last 30 days and Try Orion Intelligence at 99.35% over 90 days.
- **DASHBOARD COUNTERS** — GET /api/insight is 1.84 MB on every homepage load.
- **CRITICAL** — General Intelligence returns CSAM-advertising onion pages (kidflix) for ordinary queries ("hotel", "school", "kids", "family", "video") with Safe Search on; Safe Search only removes content type "adult"; 1,475 such records visible.
- **CRITICAL** — Unified Search (/api/ranked/ai) decrypts stealer passwords for any role with the General module; no Enterprise check (code review).
- **CRITICAL** — Public uptime page: "Some systems Down"; /strategic 2.4%, /defacement 2.8% (90 days); /breach and /consolidated down -- cause was stale uptime monitor assertions (pinned record content / rank_index+\_score fields that changed with the API); APIs return 200+v …
- **CRITICAL** — Search returns HTTP 500 when page is null (breach, general, social, exploit, marketplace).
- **CRITICAL** — POST /api/audit/logs returns HTTP 500 on a malformed daterange.
- **CRITICAL** — Snapshot endpoints return HTTP 500 on a long file name (breach and social) -- \_is_valid_screenshot_filename had no length check; a 250-char name passed validation then Path.is_file() raised OSError(ENAMETOOLONG). …
- **CRITICAL** — Data-intake endpoints /api/index/defacement, /apt, /social, /tracking crashed with unhandled exceptions (20 times).
- **STABILITY** — Browser memory leak: after garbage collection on the same page, JS heap 15 MB to 82 MB and DOM nodes 2,397 to 73,696 over 1,048 page views.
- **STABILITY** — List pages without ?page=1 send the search twice and cancel the first.
- **STABILITY** — Every page load fires POST /api/nexus/chat/clear-session.
- **STABILITY** — Elasticsearch "Result window is too large" on deep pages (page 99999); returns empty result instead of a validation error.
- **STABILITY** — Elasticsearch knn query malformed 9 times (11 Nov).
- **STABILITY** — Every search writes an audit log entry (7,107 entries in 2 days for one user).
- **SEARCH** — General Intelligence shows "15 Results Found" (page size) instead of the total.
- **DATA FRESHNESS** — Telegram: newest message 2026-06-12, newest record created 2026-10-08.
- **DATA FRESHNESS** — Facebook: newest record created 2026-07-18.
- **LEAK DATA** — New leak records still get m_medical_license / m_us_driver_license from random tokens (ARRB Group, sportsmansguide.com, Chaney Monge School).
- **LEAK DATA** — Stale relative times in Social, General and Marketplace record text ("4 minutes ago", "20m ago", "Added 54d ago").
- **LEAK DATA** — No forum record has a snapshot.
- **LEAK DATA** — Test snapshot file 00000000000000000000000000000042.html left in workspace/snapshot.
- **DASHBOARD COUNTERS** — Fixed: /api/insight is now served no-store (nginx-prod.conf).
- **SWARM_URL** — Fixed & prod-verified: prod SWARM_URL is a valid two-URL JSON array.

### Fixes made after the audit (November 19-21) (28)

#### Orion Intelligence

- /api/apk/scan posted to a Micros route that no longer exists and returned 404; it now posts to the file-scan route.
- Opening Manage Profiles or Identity Resolution forced a full page reload and collapsed the Social Intel sidebar group; the page-specific content policy for Manage Profiles is removed and the group stays expanded.
- The takedown URL check used a substring domain map that accepted look-alike hosts; it is removed in favour of the platform URL validator.
- The unique per-user index on social profile management was never awaited, so it was never created; it is now.
- Promoting a pending tenant slug used the encrypted tenant email; it is decrypted first.
- Public case shares had lost comments, tasks, linked cases and the primary entity's authors; they are returned again, limited to whitelisted fields.
- Chat links did not open and case links had lost rel="noopener"; both are restored.
- Logging out reset the app configuration and briefly showed the default palette; logout now only clears storage.
- LinkedIn impersonation takedowns were refused with "not available for this platform" although the dialog offered them; LinkedIn is now accepted.
- Fetch, Edit and Verify opened a capture window in every browser where the extension was signed in; they now target the requesting browser.
- Theme palettes left many screens in the default blue (geo-fencing panels, social search, record cards and about 200 other files); colours now follow the palette.
- The social user graph did not settle after a reload; its stabilisation step is halved.

#### Orion Social

- A stray character in the Quora takedown file was a syntax error that stopped every platform's takedown from loading [uncommitted].
- LinkedIn posting timed out opening the composer, ad monitoring found no ads and hate-speech monitoring read no posts on LinkedIn's new page design; all three are fixed [uncommitted].
- The LinkedIn takedown clicked the site's top navigation and reported success without filing a report; it now follows the report dialog and fails when a step is missing [uncommitted].
- The LinkedIn session check waited 15 seconds with no cookie check and could report a valid session as expired; it now matches the other platforms [uncommitted].

#### Orion Micros

- The ClamAV client kept a dead connection after a restart; it reconnects for each scan.
- APK size checks inflated every archive member into memory; only the manifest, dex, resource and signature members are now inflated (addresses RT-95).
- Scan jobs were finalised twice (by the controller and the runner); only the runner finalises.

#### Orion Crawler and extension

- The scheduler queued URLs that were already waiting, so queues filled with duplicates.
- A browser run with a session file reused an existing context and dropped the session; it now gets a fresh context.
- Page-load retries reported a feeder failure on each attempt; they now log a warning.
- A failed Telegram flush advanced the stored offsets and lost messages; it now returns an error and keeps them.
- 12 leak scripts repaired (Helix waiting room, Rhysida, Nitrogen, Morpheus, Termite and seven others) and 11 forum, news and defacement scripts repaired (RSS, Tor or FlareSolverr fetch changes).
- **Extension:** Google result pages are re-read until results appear; the double vault icon in capture windows is removed; closing a capture no longer opens a minimised window; a sign-in window opened from a capture no longer stays hidden behind it; the address bar no longer covers the top of the site.

#### Orion Dark Nexus

- The topic classifier used a wrong label map for its model; labels now come from the model itself with a 0.5 minimum probability and a 0.2 threshold for Adult [uncommitted].

#### CI

- A cancelled run left a root apt-get holding the package lock, so the next job on that runner failed at once; jobs now wait for it to finish.
- CI cleanup removed the wrong compose project, so run.sh stacks were left running and the next Build failed its NGINX test on a missing certificate; cleanup now removes the trusted-search project.

### Audit entries whose behaviour changed after the audit (14)

The entries stay in the lists above with their original text; this is their state on November 21.

- **RT-95** **APK scans:** addressed by the selective archive read above; the APK upload limit is now 256 MB and other files 10 MB.
- **CLI-52** **File scanner:** the scanner now also accepts .apk files; the 10 MB limit applies to other files.
- **RT-202** **Public case share:** partly reverted. Comments, tasks, linked cases and the primary entity's authors are returned again (whitelisted fields only); closedBy and related-entity authors stay hidden.
- **CLI-6** **External links:** the confirmation dialog was removed; the http/https restriction and noopener remain.
- **CLI-12** **Content policy:** the Manage Profiles policy zone is gone and the default image policy allows any https source.
- **EXT-1**, **EXT-25** **Extension commands:** commands are accepted from every approved Orion site, not only the selected one.
- **EXT-4**, **EXT-13** **Extension presence:** https sibling subdomains of the platform host and any localhost port are trusted again, and an unknown secure site learns the extension is installed (without its version).
- **EXT-16**, **EXT-22** **Capture storage:** the local-storage backup and restore and the storage wipe on release were reverted.
- **EXT-21**, **EXT-34** **Vault autofill in captures:** reverted; the vault is asked with no click and a single matching login fills automatically.
- **EXT-29** **Crawl activity list:** no longer cleared on sign-out.

### Open items — what is left (23)

#### Deployment

- Deploy extension 0.6.21 to production and set EXTENSION_VERSION; the packages are built, signed and bundled but not deployed, so LinkedIn Verify, the sign-in window fix and the address-bar fix are not live.
- Restart Micros and the web service for the APK assessment, and add MOBSF_API_KEY to each server's Orion-Micros .env (compose refuses to start without it).
- Give the six restored collector platforms a CELERY_COUNT value and a queue-map line on the crawler servers; purge the duplicate queue entries already on production.
- Register the Stripe webhook (POST /api/stripe/webhook) in each Stripe account used.

#### Uncommitted work

- **Orion Social:** the LinkedIn automation, LinkedIn takedown and Quora takedown fixes (6 files).
- **Orion Dark Nexus:** 176 files (campaigns, new and retuned specs, classifier fix, tests), plus about 14 one-off kill-chain scripts and program notes that should be moved or removed before commit.
- **Orion-DeepAI-Landing (5 files) and Orion-Intelligence-Solution (1 file):** contact details and the licence holder name.

#### Billing

- A webhook event's ID is saved before it is handled, so if handling fails Stripe's retry is treated as a duplicate and the event is never reprocessed.
- The Stripe API key is a process-wide value set per request although several Stripe accounts can be in use at once.
- The pricing-config endpoint accepts raw JSON with no numeric validation; emails print "$" whatever the configured currency.
- Users disabled when a plan is activated or revoked are not re-enabled automatically, and a failed payment blocks features at once with no grace period.
- The Billing route has no route guard (the page redirects and the API returns 403), and the production connect-src policy does not list api.stripe.com (not verified whether it is needed).
- Subscribe, upgrade, Stripe-key and pricing changes write no audit event.

#### Social automation

- Scheduled persona posts fail on every platform while the image host used by the persona post library (loremflickr.com) returns HTTP 500; the images need re-hosting.
- The LinkedIn impersonation report has not been submitted for real, and the LinkedIn session used for testing was signed out by LinkedIn after repeated automated runs and must be captured again.
- Instagram hashtags, LinkedIn hashtag feeds and search, and Quora search collect nothing without a session file; the four quarantined forum scripts still emit nothing.
- A local "LinkedIn Test" persona and profile, two failed post rows and one real LinkedIn post were left by the November 21 test.

#### Collectors

- 38 stale leak scripts were removed from the production Feeder only; the repository still holds them (164 scripts against 126 in production) and the list of the 38 names was not kept.
- SOCIAL_COLLECTOR_RULES.md still says the forums family has 44 scripts (it has 134).

#### Documentation

- The Identity Resolution manual still describes the swap control, and the page, test and manual still carry the title "Identity Resolution & Impersonation Detection"; Keywords mode, APK Security Assessment and Billing are not in the manuals.

#### Not live-tested

- The automated billing tests run against fixture routes, not Stripe; a full payment has not been confirmed against a live Stripe account in this pass.
- The address-bar push-down was tested in a scripted browser on four sites; the sign-in window focus change was not tested; neither has been confirmed in the installed extension on a user's desktop.

#### Earlier audit gaps

- The 9 entries under "Partially addressed — a gap remains" below are still open.

### Partially addressed — a gap remains (9)

- **RT-55** `MED` — The target name is resolved once at the check and again by Micros, ZAP and the browser with no address check, and scan jobs are checked at create but sent at poll, so a name that changes its answer reaches internal addresses.
- **OTH-55** `MED` — Sandbox workspace size is checked only once a minute and the response is to stop the control plane for everyone, not the offending sandbox, so one run can fill the disk or keep the service down.
- **INF-55** `LOW-MED` — The internet-facing storage service uses host networking and receives the whole .env including the GitHub deployment token, so a flaw in it reaches every loopback-only service on that host.
- **RT-40** `LOW` — Several search inputs have no size limit, request keys reach Elasticsearch as field names, and no search route uses the request limiter.   ⟨note: search should not have size limit⟩
- **EXT-30** `LOW` — 307 of the 1,097 collectors use a plain http site address, so those crawls expose targets to the network and results can be altered in transit.
- **CRITICAL** — Backups: last backup 2026-09-23; 30 stale backup maintenance flags cleared after interrupted backups -- newer backup exists now (2026-11-15 20:51, instant), but /api/admin/backups/status = failed 'Backup worker stopped responding' at 30% -&gt; interrupted-back …
- **STABILITY** — Auto alert scan completed_with_errors 7 times; "Alert category run failed" 7 times -- cause: per-tenant alerts doc (one unbounded embedded array + raw_findings, soft-deletes never pruned) exceeds MongoDB 16MB on engine.save() in upsert_alerts_bulk -&gt; outer …
- **LEAK DATA** — Leak industry field is every sector name joined together on ransomware.live records (31 of latest 300).
- **LEAK DATA** — Leak snapshot section shows "Resource not found" and a console 404 on almost every leak report (3 leak reports have HTML).

### Improvements (3)

- **#43** — Marketplace sources have been increased from 12 at the time of the 5 Nov crawl to 500, an increase of about 4,067% (roughly 42 times), so marketplace results no longer depend on a single crawl. …
- **#44** — Since that test, forum sources have been increased from 60 to more than 150 (up about 150%) and marketplace sources from 12 to 500 (up about 4,067%), so these queries now draw on far more sources. Forum and market queries return only 4–12 hits each.
- **#23** — The AuditTeam crawler has been dead since 12 Sep 2026, so 3 of 30 test victims were missed (Pelican ICT, jltrust.com, hnopvm.org).
