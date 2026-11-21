(orion-core)=

# Orion Core

Orion Core covers the main Orion web application: entry and access, search, indexed investigation modules, live lookups and scans, graph investigation, Social Intelligence, reports, profile and tenant workflows, Orion Vault, and administration.

## About This Guide

Orion is an investigation and monitoring platform that combines indexed intelligence, live lookups, graph exploration, tenant workflows, and platform administration in one interface. Users typically work in one of four ways:

1. Enter the platform through login, signup, verification, onboarding, or shared links.
2. Search indexed data from the main dashboard.
3. Run a targeted lookup or scan against a domain, file, email, IP, username, or other entity.
4. Open a report view to inspect metadata, evidence, and relationships.
5. Manage tenant, user, alert, case, collection, and platform settings based on role permissions.

This part of the manual is organized around those tasks. Orion Mail, Orion Leaks, and DeepIntel have their own parts of the [User Manual](../user_manual.md).

For concise, task-based instructions with navigation and troubleshooting, see the [Orion Help Manual](../help_manual.md).

```{contents}
:local:
:depth: 2
```

## Access and Entry Points

### Signup

Signup creates a new account request. The form validates username format, email format, and password requirements before submitting the registration.

The username must start with a letter and use the supported username pattern. If the value is invalid, Orion suggests a corrected format. Password input shows strength and requirement feedback before submission.

After a successful signup, the user is sent to the welcome flow. The account may still require administrator review, email verification, or tenant onboarding before full dashboard access.

Signup is served only on the root platform domain and on the subdomain of a tenant that an administrator has marked as a primary tenant. A signup completed on a primary tenant subdomain creates a sub-tenant beneath that primary tenant, and its verification email points back at the primary tenant URL. When the primary tenant has consumed its full tenant quota, the `Sign Up` link is hidden on that login page and the signup URL redirects to `/login`.

### Login

The standard entry point is the login screen. Depending on deployment settings, users may also encounter:

- account onboarding
- welcome or notification screens
- password reset flows

Browser sign-in uses an encrypted, HTTP-only session cookie instead of storing the access token in browser local storage. In production, the cookie is also marked `Secure`, uses `SameSite=Lax`, and expires after 30 minutes. Session renewal and authenticated requests use the cookie automatically. Signing out invalidates the server session and removes the authentication cookie.

Unsuccessful login attempts are tracked against the entered email or username. The fifth consecutive failure temporarily pauses login for 1 minute, the next failure pauses it for 10 minutes, and later consecutive failures pause it for 30 minutes. The login screen reports how long to wait. A successful login clears the failure count, and an inactive failure history expires after 30 minutes.

```{figure} ../../screenshots/login-page-20260326.png
:alt: Orion login page
:width: 100%

Login screen used for standard account access.
```

```{admonition} Role-aware experience
:class: note

The sidebar, available modules, and some actions are controlled by role, tenant state, and license assignment. Two users in the same deployment may not see the same menu.
```

### Password Reset

Select `Recover account?` on the login screen to open the recovery page. The page provides two separate tabs:

- `Reset password` sends a reset link to the registered email address.
- `Account recovery` verifies the registered email together with the user's global recovery key before sending the same type of reset link.

The standard reset flow supports two stages:

- requesting a reset link by email
- submitting a new password using a tokenized reset link

The request form validates the email format when submitted. Orion always displays the same success result for a well-formed request, whether or not the email is registered. This prevents the recovery screen from revealing which accounts exist.

Reset links expire after 20 minutes, are restricted to the correct tenant, and can be used only for an active account. The reset token is stored as a hash and is removed after a successful password change. The new-password form includes password-strength guidance and confirmation validation, and the new password cannot match the previous password.

If an administrator requires a password change, a successful login redirects the user to the same new-password screen before normal work continues. Completing the change clears the forced-reset requirement.

#### Recover With a Recovery Key

Use account recovery when you have the global recovery key generated from Account Settings:

1. Select `Recover account?` on the login screen.
2. Select the `Account recovery` tab.
3. Enter the registered email address.
4. Enter the complete 43-character recovery key.
5. Select `Recover account` and check the registered email for a reset link.
6. Open the link, choose a new password, confirm it, and submit the form.

The page validates email and recovery-key formats before submission. For correctly formatted input, Orion returns the same success screen even when the email or recovery key is incorrect. A reset email is sent only when both values match.

:::{admonition} Recovery key and 2FA setup key
:class: important

The global recovery key is different from the authenticator setup secret shown while configuring 2FA. Use the global recovery key on the Account Recovery tab. Keep both values private and store them outside the Orion browser session.
:::

```{figure} ../../screenshots/password-reset-20260326.png
:alt: Password reset request page
:width: 100%

Password reset workflow entry point.
```

### Welcome and Email Verification

The welcome page appears after signup and after tokenized verification links. Without a token, it confirms that registration was submitted and tells the user to wait for administrator approval or email notification.

When opened with a verification token, the page verifies the token and reports one of the following states:

- verification successful
- expired verification link
- invalid verification link
- temporary verification failure

After a successful verification, users can continue to login or onboarding depending on account state.

### Tenant Onboarding

New tenant users may be routed through a multi-step onboarding flow before using the main dashboard. The onboarding wizard includes:

1. company information
2. IOC setup
3. confirmation

During onboarding, users can define monitored IOC values by category before entering the main application.

The tested tenant flow confirms that onboarding is part of a larger tenant lifecycle rather than a standalone form. Covered user-visible behavior includes:

- tenant signup and verification email delivery
- admin-side tenant review and verification changes
- enterprise-license assignment before first tenant login
- onboarding wizard completion
- IOC seeding during onboarding
- tenant sub-user creation immediately after onboarding

### Notifications and Subscription Screens

Notification screens are used for access-level messages such as trial expiration, subscription prompts, or deployment-specific access notices. The payment gateway screen displays trial or subscription messaging when payment or upgrade flow context is needed.

These screens do not expose investigation data. They explain why a user cannot continue directly into the requested workflow and provide a path back to the main application.

### Shared Links

Some workflows can produce share links that open outside the signed-in dashboard:

- shared case links open a scoped case view.
- shared chat links open a scoped AI/chat transcript.

Shared views are narrow by design. They expose only the material attached to that share link and do not grant broader dashboard access.

## Main Application Layout

After authentication, Orion opens inside the `dashboard` workspace.

```{figure} ../../screenshots/homepage-overview-20260326.png
:alt: Orion homepage
:width: 100%

Orion dashboard landing view.
```

The main UI is centered around four areas:

- the left sidebar for navigation
- the global search and module toolbar
- the result or report workspace
- slide-out or inline filter panels

### Left Sidebar

The left sidebar is the primary navigation system. It groups features by investigation area and by operational purpose.

```{figure} ../../screenshots/homepage-overview-20260326.png
:alt: Orion sidebar
:width: 100%

Expanded sidebar with major modules and support links.
```

The sidebar can include:

- user profile and account pages
- indexed search modules
- live scan and API modules
- graph tools
- support links such as `Onion Link`, `Links`, and `Documentation`

### Global Search Area

Most data-driven modules share the same search pattern:

- a search box
- optional advanced filtering
- optional search tools
- an optional right-side filter drawer

```{figure} ../../screenshots/homepage-searchbar-20260326.png
:alt: Global search bar
:width: 100%

Search bar with search, advanced mode, and tools controls.
```

### Result Workspace

The result area changes by module, but commonly includes:

- a result count
- cards or row-based entries
- analytics summaries
- filters
- pagination
- empty, loading, and no-result states

## Global Search Workflow

The search bar is the main entry point for indexed investigation.

### Basic Search

In standard mode, users can enter a free-text query and submit it immediately. Orion then loads results for the current module context.

### Advanced Search Toggle

The `Advance` toggle enables the filter overlay below the search bar. When enabled, Orion exposes indexed filter controls that let users narrow the query more precisely.

### Tools Menu

The `Tools` section provides search behavior controls and, in some contexts, sorting options.

```{figure} ../../screenshots/homepage-searchbar-20260326.png
:alt: Search type controls
:width: 100%

Search entry area with search mode and tools controls.
```

Available search modes in the main result workflow include:

- `Match Semantic`
- `Match any term (OR)`
- `Match individual terms (AND)`
- `Match full query`

These modes affect how broadly or narrowly Orion interprets the query.

### Search Filters

When advanced mode is enabled, users can add indexed filters to refine the result set.

```{figure} ../../screenshots/search-filters-20260326.png
:alt: Search filters
:width: 100%

Filter controls for refining indexed search.
```

Across the application, filter panels typically support:

- dropdown selection
- text input
- date range input
- apply
- reset

### Selected Filter Bar

When entity filters, sidebar filters, or non-default search tools are active, Orion can display a selected-filter bar showing what is currently affecting the result set.

## Homepage

The homepage is the default overview for many users and acts as a search-first dashboard.

The homepage typically includes:

- the global search entry point
- high-level summaries
- statistics or insight cards
- general and leaked index summaries

For some privileged roles, the homepage also includes a draggable insight panel layered over the main search experience. Other users may instead see a simplified search-first landing view or a tenant-home style alert summary, depending on license assignment and whether the account belongs to a default tenant.

### Homepage Summary Areas

- `General Index`: broad indexed content gathered across supported sources.
- `Leaked Index`: sensitive, exposed, or higher-priority findings.
- `Recent or featured results`: direct pivots into current records.
- `Insight blocks`: charts and counts used for quick triage.

```{figure} ../../screenshots/homepage-overview-20260326.png
:alt: Homepage dashboard
:width: 100%

Homepage overview with summary panels and search-first layout.
```

```{figure} ../../screenshots/heatmap-report-20260326.png
:alt: Homepage heatmap country report
:width: 100%

Country-level heatmap report opened directly from the homepage world map.
```

The tested homepage workflow also includes:

- hovering countries to reveal tooltip state
- opening country-level report panels from the heatmap
- closing the report by close control and by overlay
- keeping homepage search and heatmap pivots available in the same workspace

## Analytics and Result Insights

Orion exposes analytics alongside search results to help analysts understand the composition of the returned dataset.

```{figure} ../../screenshots/consolidated-insights-20260326.png
:alt: Keyword insights
:width: 100%

Keyword-level insight and result analysis.
```

```{figure} ../../screenshots/consolidated-results-20260326.png
:alt: General result analytics
:width: 100%

Expanded result insight and breakdown panels.
```

Depending on module and query, analytics can summarize:

- keyword frequency
- category distribution
- result volume
- network or source distribution
- URL and title breakdowns

## Navigation Reference

The exact menu depends on license and permissions, but the Orion UI commonly exposes the following modules.

| Module | Primary purpose | Typical views |
| --- | --- | --- |
| Homepage | Entry point and overview | search, summaries, statistics |
| General Intelligence | Broad indexed intelligence search | All, General, Forums, News, Stolen, Drugs, Hacking, Marketplaces, Cryptocurrency, Leaks |
| Data Breach | Breach records, exposure checks, and leak references | All, Databases, Tracking |
| Marketplace | Structured listings from underground marketplaces | All, listing reports |
| Compromise Monitoring | Website compromise and defacement monitoring | All, Hacked, Phishing, Databases |
| Social | Social and community-source intelligence | All, Telegram, Twitter, Mastodon, Pastebin, Forum, Reddit, Facebook, Instagram, LinkedIn, TikTok, YouTube |
| Exploit | Vulnerability and exploit intelligence | All, CVE, Tools, ZeroDay |
| Actors & Malware | APT actor and malware-family intelligence | All, APT, Malware, Compromised-Actors |
| Consolidated | Combined multi-source investigation | IOCs, Deep Search, Network Intel |
| News Feed | News-style intelligence stream | News, Tracking |
| Stealer Logs | Credential and IOC investigation | IOCs |
| Entity Lookup | Entity-based live lookups | Email Breach, Social Scanner, Wanted List, National Identity, Playstore Scanner, Software Scanner, File Scanner, Text Analysis, Crypto Scanner, DKIM Lookup |
| Web Scans | Live web-target scanning | Basic Scan, Port Scan, Repository Scan, SEO Scan, APK Scan, scan reports |
| Network Intel | Domain, IP, and vulnerability recon | Host Recon, IP Scan, Vulnerability Scan with depth controls |
| Satellite Intel | Geo-fencing, satellite map, facilities, aircraft, and ship tracking | Satellite Map, Threat Lens, Imagery Analysis |
| Social Intel | Username and profile mapping, social monitoring, and impersonation checks | Profile Intelligence, Social Monitoring, Identity Resolution |
| CTI Graph | Cyber relationship mapping | graph filters, Advanced Graph Builder, cluster, document, property pivots |
| Account Settings | Current-user profile, security, and preferences | profile image, theme, 2FA, password, recovery key, licenses |
| Public User Activity | Visible profile activity review | user profile, activity items, thread links |
| Tenant Homepage | Tenant alert and monitoring overview | risk cards, alert categories, export, scan actions |
| Manage IOCs | Tenant monitored-value management | IOC tabs, add, import, remove, clear |
| Monitoring | Operational monitoring container | Log Manager, Auditlog, Event Management |
| Event Management | SIEM-style event search | IOC-tagged search, date filter, event expansion |
| Log Manager | Admin operational log review | type/date filters, file delete, flush logs |
| Case Management | Investigation case tracking | filters, analytics, details, artifacts, integrity verification, tracking board, shares, PDF export |
| AI Workspace | In-app assistant workspace, documented in [DeepIntel](deepintel.md) | Nexus chat, Kill Chain, quick prompts, message actions, streaming controls, shared chat links where enabled |
| Feeder | Feed/source rule operations | rule management and ownership flows where enabled |
| Tenant Settings | Tenant identity, domain, mail, and alert configuration | tenant image, contact data, license and quota summary, custom domain, webmail, network configuration, alert integrations, alert schedule, privacy and access |
| Take Down | Takedown request review | domain and social takedowns, accept, reject, retry, Takedown Mail conversations |
| Orion Vault | Personal encrypted credential store | setup, unlock, credentials, recovery key, vault security settings |
| Users | Tenant user management | add, edit, license assignment, status, delete |
| Tenants | Cross-tenant administration | tenant status, quotas, licenses, alert settings |
| Audit Logs | Administrative history | user and tenant activity records |
| System Settings | Global platform configuration | branding, feature visibility, app metadata |
| Custom Alerts | Manual alert creation | alert type, title, source, URL, IOC |
| Links | Link directory and monitored references | directory listing |
| Onion Link | External onion access | external link |
| Whistle Blowing | Orion Leaks reporting portal, documented in [Orion Leaks](orion_leaks.md) | external link |
| Documentation | Published documentation | external docs |

The following supporting workflows are also part of the product even though they are not normal sidebar modules:

| Workflow | Where users encounter it | Purpose |
| --- | --- | --- |
| Signup | Public entry screen | account request creation with username, email, and password validation |
| Welcome and verification | Registration email and welcome screen | registration status and email verification result |
| Password reset | Reset request and emailed reset link | privacy-preserving reset request and tokenized password update |
| Account recovery | Recovery page opened from login | email and global recovery-key verification followed by an emailed reset link |
| Tenant onboarding | First-run tenant setup | tenant identity, monitored values, and initial configuration |
| Notification | Access, trial, and subscription notices | access-level, trial, or subscription message |
| Payment gateway notice | Trial, payment, or subscription state screen | trial/payment/subscription state notice |
| Case share | Shared case review link | public case review link |
| Chat share | Shared chat transcript link | public shared chat transcript link |
| Scan report | Scan notifications and scan-result links | reopened tracked scan result |
| Discussion workflow | Discussion-style result workflow | supported discussion, chat, and social result paths |
| Social mapper aliases | Social Intel navigation | alternate paths into Social Intel |

## Indexed Investigation Modules

### Consolidated

The consolidated view is Orion's combined investigation workspace. It is designed for users who want one query to drive multiple result channels instead of searching each module separately.

The consolidated workspace can expose three major tabs:

- `IOCs`
- `Deep Search`
- `Network Intel`

Depending on the query and license state, this view can combine:

- grouped indexed results
- stealer-log matches for qualifying queries such as emails or URLs
- embedded network or scan-style pivots

Use consolidated search for first-pass triage when you want breadth before moving into a dedicated module.

```{figure} ../../screenshots/consolidated-results-20260326.png
:alt: Consolidated investigation results
:width: 100%

Combined result workflow used for broad first-pass triage.
```

### General Intelligence

General Intelligence is the primary broad-spectrum indexed search area. Use it when you want to search topics, entities, or keywords across multiple kinds of sources.

Subcategories:

- `All`
- `General`
- `Forums`
- `News`
- `Stolen`
- `Drugs`
- `Hacking`
- `Marketplaces`
- `Cryptocurrency`
- `Leaks`

```{figure} ../../screenshots/general-intelligence-results-20260326.png
:alt: General intelligence results
:width: 100%

General Intelligence result workflow.
```

Typical use cases:

- surveying discussions around a topic
- reviewing leak mentions
- exploring dark-web marketplace activity
- scanning mixed-source intelligence for a keyword

### Data Breach

The Data Breach module is used for known breach data, identity exposure checks, leak references, and breach-related listings gathered from monitored sources.

Subcategories:

- `All`
- `Databases`
- `Tracking`

Use `Databases` when you want structured breach records. Use `Tracking` when checking whether a specific email or identity appears in known breach data. Use breach report fields and URL/link pivots when the key artifact is a leak URL, dump reference, or channel-style source mention.

```{figure} ../../screenshots/data-breach-tracking-20260326.png
:alt: Email breach tracking
:width: 100%

Example of a breach tracking workflow.
```

### Marketplace

The Marketplace module lists individual product and service listings collected from underground marketplaces, most of them on the onion network. Each record is one listing rather than one web page, so it can carry listing details such as the market name, vendor, listing category, price, currency, and availability status.

The sidebar entry is `Marketplace` and appears directly below `Data Breach`. Opening it loads `/dashboard/marketplace/all`. The module has no subcategories, so `All` is the only view.

#### What a Listing Contains

Marketplace records are stored in their own index and can include the following fields:

| Field | Meaning |
| --- | --- |
| Title | Listing title as published on the market |
| URL and base URL | Listing page and the market site it belongs to |
| Content and important content | Listing description and the key extracted text |
| Network | Network layer of the market, normally `onion` |
| Market name | Name of the marketplace |
| Vendor | Seller account that published the listing |
| Category | Listing category assigned by the market |
| Price and currency | Advertised price and the currency it is quoted in |
| Status | Listing state reported by the market |
| First seen and last seen | When the collector first and last observed the listing |
| Date | Listing or publication date |
| Tags and team | Labels and group attribution, when available |
| Screenshot, images, web links, and dump links | Supporting evidence captured with the listing |

Which fields are populated depends on what the source market exposes. A listing is identified by its market site and title, so a relisted item with the same title on the same market updates the existing record instead of creating a new one.

#### Search and Filters

1. Open `Marketplace` from the sidebar.
2. Enter a keyword, product, brand, or entity in the global search box.
3. Narrow the results with the filters, then review the result cards.

The module uses the same filter set as Data Breach:

- `Network Type`: `All`, `Onion`, `I2P`, or `Clearnet`
- `Creation Date Range`: limits results by listing date, update date, or creation date
- `Content Type`: the shared threat content filter

Results are sorted by date. When no date range is set, Orion searches roughly the last 150 days.

Each result card shows the listing title, URL, description, network, published date, and freshness status, with `Open Report` and `Open CTI Graph` actions.

#### Listing Detail View

Select `Open Report` on a card to open the listing at `/dashboard/marketplace/all/<record-id>`. The listing opens in the standard report layout described in [General Report Page](#general-report-page): title, description, web reference, network, published and last-checked dates, content-type tags, screenshot preview, and the expandable [Metadata Panel](#metadata-panel). Report translation, export, sharing, and graph pivots behave as in the other indexed modules.

#### Marketplace Results in Other Views

Marketplace records also appear outside the dedicated module:

- Consolidated search includes a `Marketplace` result group, and its `Platform` filter has a `Marketplace` option.
- Geo Fencing Threat Lens includes `Marketplace` as a category.
- Feeder statistics count marketplace records under `Breached and stolen data`.

#### Marketplace Module Compared with the Marketplaces Filter

General Intelligence also offers a `Marketplaces` subcategory. The two are different:

| | `Marketplace` module | General Intelligence `Marketplaces` |
| --- | --- | --- |
| Data | Structured listings collected by the marketplace collector | Crawled web pages classified as marketplace sites |
| Unit of result | One listing | One page |
| Listing fields | Vendor, market name, category, price, currency, status | Not available |
| Use it for | Finding what is being sold, by whom, and for how much | Finding marketplace sites and general page content about them |

Use the `Marketplace` module when you need listing-level detail. Use the General Intelligence `Marketplaces` view when you want broader coverage of marketplace sites and pages.

#### Access and Licensing

Marketplace has no license of its own. It follows the Data Breach module permission:

- the sidebar entry is shown to administrators and to users whose license includes the Data Breach module, such as `osint_basic`, `osint_advanced`, or `enterprise`
- search is available to the admin, member, analyst, and demo roles when their license includes the Data Breach module
- the listing detail view also opens for users holding the `maintainer` license

A user without the Data Breach module receives a `No license for module: breach` error if they open a marketplace route directly.

### Compromise Monitoring

Compromise Monitoring tracks defacement-style website incidents where sites were altered, hijacked, cloned, or otherwise compromised. Backend routes and some report names still use the `defacement` term, but the current sidebar label is `Compromise Monitoring`.

Subcategories:

- `All`
- `Hacked`
- `Phishing`
- `Databases`

The detail view commonly exposes:

- target URL
- date saved
- attacker or defacer
- team name
- server or IOC context
- breach or source reference
- IP and location

```{figure} ../../screenshots/defacement-report-20260326.png
:alt: Compromise Monitoring report view
:width: 100%

Compromise Monitoring result detail with target and attacker context.
```

### Social

The Social module aggregates intelligence from social and community platforms.

Supported views:

- `All`
- `Telegram`
- `Twitter`
- `Mastodon`
- `Pastebin`
- `Forum`
- `Reddit`
- `Facebook`
- `Instagram`
- `LinkedIn`
- `TikTok`
- `YouTube`

Use this module for:

- early warning and chatter monitoring
- leak discovery
- discussion tracking
- platform-specific searches

```{figure} ../../screenshots/social-report-20260326.png
:alt: Social or feed-style intelligence results
:width: 100%

Example of a stream-oriented social intelligence view.
```

#### Discussion Route

Discussion opens discussion-style chat and social result contexts. It uses the current result container and can open chat, social, general, leak, exploit, and compromise-monitoring reports.

Use the Social module for normal sidebar navigation. Use Discussion when a workflow routes through discussion-style result paths.

### Exploit

Exploit focuses on vulnerability and exploit-related intelligence.

Key views:

- `CVE`
- `Tools`
- `ZeroDay`

This module is useful when starting from:

- a known vulnerability ID
- a product or platform with public exploit coverage
- a threat report mentioning exploit tooling

The E2E workflow covers all tested exploit entry points:

- `All`
- `CVE`
- `Tools`
- `ZeroDay`

```{figure} ../../screenshots/exploit-results-20260326.png
:alt: Exploit module results
:width: 100%

Exploit search workflow across the tested vulnerability and tooling views.
```

### Actors & Malware

Actors & Malware is the APT Intel workspace for actor and malware tracking. It is exposed as a licensed sidebar module when the user's license allows access.

Key views:

- `All`
- `APT`
- `Malware`
- `Compromised-Actors`

Use this module when starting from:

- an APT actor name or alias
- a malware family, signature, or reporter
- a country-linked threat actor question
- a need to compare actor and malware records in one search

Actor and malware results open into detail reports with the same report review, export, and pivot behavior used by other indexed investigation modules.

### News Feed

News Feed is the stream-oriented intelligence area for news-style content and current reporting. It is useful for users who want a curated readout without first building a structured query.

The tested feed workflow covers:

- opening the `News` feed view
- submitting a live query
- opening a report
- reviewing JSON-backed detail inside the report

```{figure} ../../screenshots/feed-report-20260326.png
:alt: Feed report view
:width: 100%

Feed report workflow with structured detail and raw response inspection.
```

### Help & Support

The profile menu exposes a support workflow that is part of the tested navigation model.

Covered user-visible behavior includes:

- opening Help & Support from the profile menu
- filling email, subject, and message fields
- submitting the support request

```{figure} ../../screenshots/support-modal-20260326.png
:alt: Help and support modal
:width: 100%

Support modal used for direct in-app support requests.
```

## Stealer Logs

Stealer Logs is a dedicated credential and IOC investigation workflow for infostealer-derived data.

### Search Modes

The stealer-log search bar supports two operating modes:

- `Basic`
- `Advanced`

### Basic Mode

Basic mode lets users search by a selected tag. Available tags include:

- `All`
- `Domain`
- `Email`
- `Credit Card`
- `IP`

Validation is applied to tag-specific inputs where needed.

### Advanced Filter Builder

Advanced mode exposes a row-based query builder that supports:

- `WHERE`
- `AND`
- `OR`

Each row combines:

- an operator
- a data tag
- a value

This is the preferred mode for precise hunting across large stealer datasets.

### Result Metrics

The Stealer Logs results page surfaces quick metrics such as:

- search elapsed time
- total results
- asset count
- aggregated count

### Supporting Actions

The toolbar can include:

- password scheme view
- domain or subdomain helper
- result download

The password-scheme helper is useful when you want to inspect likely password formats or schema patterns. The domain helper provides a fast pivot into related host or subdomain exploration without leaving the stealer-log workflow.

### Result Review

The results area is designed for:

- large record volumes
- structured credential review
- pagination
- ranked result handling

:::{admonition} Common use case
:class: note

Use Stealer Logs when you already have a domain, email, or IP and need to confirm whether it appears in infostealer-derived material.
:::

```{figure} ../../screenshots/stealer-logs-results-20260326.png
:alt: Credential and stealer-log results
:width: 100%

Structured result review for credential-focused investigations.
```

### Dismissing Results

Stealer-log and credential results can be dismissed so they stop appearing in future investigations. Dismissal is tenant-wide: once a result is dismissed, every user in that tenant stops seeing it, not just the person who dismissed it. A dismissed result can be restored again from the same control.

A `Hide dismissed` toggle controls whether dismissed rows are hidden from the current view or shown with a dismissed marker, so reviewers can still audit what was dismissed when needed.

Dismissing and restoring results requires the `Dismiss Result` user permission. Users without that permission can review results but cannot dismiss or restore them, and the dismiss control is not available to them. The permission is granted per user in tenant user management (see [Tenant Users](#tenant-users)); tenant administrators can dismiss across the tenant.

## Live Lookup and Scan Modules

### Entity Lookup

Entity Lookup is used for targeted live lookups rather than passive indexed browsing. The sidebar label is `Entity Lookup`, and the module groups focused enrichment workflows in one place.

Available lookup types:

- `Email Breach`
- `Social Scanner`
- `Wanted List`
- `National Identity`
- `Playstore Scanner`
- `Software Scanner`
- `File Scanner`
- `Text Analysis`
- `Crypto Scanner`
- `DKIM Lookup`

```{figure} ../../screenshots/entity-api-email-breach-20260326.png
:alt: Entity Lookup view
:width: 100%

Entity Lookup interface for live lookup workflows.
```

#### Common Entity Lookup Use Cases

- breach validation for a single email
- identity enrichment
- app and software lookups
- file analysis
- text analysis for spam or malicious URL detection
- crypto-address context
- email-authentication (DKIM/DMARC/SPF) posture checks
- email forensics and delivery tracing for a received message

### DKIM Lookup

DKIM Lookup is part of Entity Lookup. It inspects email-authentication posture (DKIM, SPF, and DMARC) and can trace how a message was delivered. The page has two tabs: **Domain Lookup** and **Raw Email Forensics**.

#### Domain Lookup

Discovers DKIM selectors for a domain, validates each DKIM DNS record, and runs the related DMARC and SPF checks.

Inputs:

- `Domain` (required), for example `example.com`
- `Selector` (optional)

If a selector is provided, only that selector is checked. If the selector field is left empty, the lookup discovers selectors automatically from public archives and common selector names, then validates each discovered selector one at a time.

For each selector the result shows whether the record was found, the syntax and public-key checks, the key type and size, the source (live DNS or archive), any warnings (such as a weak 1024-bit key or a record seen only in archives), and the raw record. A `Domain Security` panel summarizes the domain's DMARC policy and SPF record. Completed lookups can be exported as a report.

Use Domain Lookup for:

- confirming a domain's DKIM selectors and record validity
- reviewing DMARC policy and SPF publication for a domain
- spotting weak keys or selectors no longer published in live DNS

#### Raw Email Forensics (Email Trace)

Analyzes a single received message from its raw source instead of from a domain. In your email client, open the message, view its original source, and paste the full message — headers **and** body — into the box. A **How to get this?** button at the top-right of the panel shows step-by-step instructions for Gmail, Outlook / Microsoft 365, Apple Mail, Yahoo, and Thunderbird.

For that specific message the result reports:

- **DKIM Signature** — the signing domain and selector, and whether the receiving server recorded a `dkim=pass`.
- **SPF Check** — the SPF result, the sending server's originating IP, and the envelope sender.
- **DMARC Policy** — the DMARC result, the `From` domain, and the published policy (for example `p=quarantine`).
- **Network Hops** — the delivery route rebuilt from the `Received` headers, shown as a timeline (source at the bottom, destination at the top).

Use Raw Email Forensics for:

- checking whether a specific message was authenticated (spoofing and phishing triage)
- tracing where a message actually originated and which servers it passed through
- confirming the SPF, DKIM, and DMARC results for a message you received

The results reflect the authentication verdicts recorded in the message headers and the routing they describe; they are a fast forensic read of the message rather than an independent re-verification of the DKIM signature.

### Text Analysis

Text Analysis is part of Entity Lookup. It is used when the artifact is text rather than a file, domain, username, or IP address.

Use Text Analysis for:

- suspicious message bodies
- text that may contain malicious URLs
- spam or phishing-style content checks
- short copied snippets that do not justify a full file upload

The output depends on the configured analysis service, but the workflow follows the same scan-oriented pattern: provide the input, run the analysis, review the result, and use the result in a broader investigation if needed.

### File Scanner

File Scanner is the upload-based analysis area inside `Entity Lookup`.

#### Main Modes

The workflow supports two related use cases:

- file IOC extraction
- APK analysis

#### Supported Behavior

The File Scanner workflow includes:

- file-type validation
- size validation
- upload and processing progress
- grouped IOC output
- export and print for supported scan types

#### IOC Extraction Output

For file IOC extraction, Orion groups indicators into categories such as URLs, packages, permissions, tampering markers, and other extracted values based on the uploaded content.

```{figure} ../../screenshots/file-scanner-report-20260326.png
:alt: File scanner result
:width: 100%

File-scanner workflow after upload and successful analysis.
```

### Web Scans

Web Scans is the live scanning area for web-facing targets. Depending on the selected scan, the workflow may run through Entity Lookup, Network Intel, or the scan report view used to reopen completed scan jobs.

#### Available Scan Types

- `Basic Scan`
- `Port Scan`
- `Repository Scan`
- `SEO Scan`
- `APK Scan`

#### Standard Workflow

The standard web-scan flow is:

1. enter a target domain or repository-style URL
2. run the scan
3. wait for loading-step progress
4. review the generated report
5. reopen the report from scan-job notifications if needed

#### Report Structure

The resulting report commonly includes:

- a security grade
- host and port
- TLS status
- scan metadata such as `Scanned On` and `Scanned By`
- categorized findings
- evidence or proof blocks
- download and print actions

#### Findings and Error States

Finding sections also show severity and confidence labels, so the report can be used for quick triage as well as export.

Scan failures are handled with retry guidance and error messaging.

```{figure} ../../screenshots/web-scan-report-20260326.png
:alt: Web scan report
:width: 100%

Web scan report with security posture, findings, and metadata.
```

```{figure} ../../screenshots/apk-scan-report-20260326.png
:alt: APK scan result
:width: 100%

APK scan workflow after file upload, analysis, and report generation.
```

### Tracked Scan Jobs

Long-running scan and lookup actions are tracked as scan jobs. This applies to several Entity Lookup, Web Scan, Network Intel, crypto, dynamic social, wanted-list, and national-identity workflows.

Scan jobs can appear in the left home menu and notification surfaces with states such as queued, running, done, or error. Users can reopen completed scan reports, resume incomplete jobs, and poll running jobs without starting the same scan from scratch.

User-visible scan job actions include:

- opening the completed scan report
- returning to an incomplete job
- polling a running job for fresh status
- marking a scan notification as seen
- deleting one completed scan notification
- clearing all completed scan notifications

Duplicate scan handling follows these rules:

- if an identical scan is already running, Orion reuses the existing running job
- if an identical scan completed in the last three days, Orion opens the previous result automatically
- if the last identical completed scan finished more than three days ago, Orion asks whether to use the previous result or run a new scan
- choosing to run a new scan sends the request with `force_new=true`

Notification controls allow terminal scan jobs to be marked seen, deleted individually, or cleared in bulk. Incomplete jobs are prioritized so users can continue active work before reviewing completed scans.

Scan reports open from notifications, result links, or completed scan workflows. Users normally do not need to type a direct address manually.

### Network Intel

Network Intel provides live recon workflows for domains and IPs.

Tabs:

- `Host Recon`
- `IP Scan`
- `Vulnerability Scan`

```{figure} ../../screenshots/network-intel-host-recon-20260326.png
:alt: Network intelligence view
:width: 100%

Network Intel module for recon and vulnerability review.
```

#### Host Recon

Host Recon looks up the DNS records of a domain or host name: addresses, mail servers, name servers, TXT, CNAME, SOA and CAA records, and the SPF and DMARC records. When a host name such as `scanme.nmap.org` is searched, the records of its parent domain are listed as well, and a DMARC policy published on the parent domain is shown with the domain it comes from. Every search is a fresh lookup. Each resolved IP can be opened to see its IP Scan details.

#### IP Scan

IP Scan is reconnaissance for a single IP address. It lists the open ports, the service and software on each port, TLS certificates, HTTP details, and where the address is hosted. The selector next to the search box sets how many of the most common ports are checked: 100, 1,000 or 5,000.

IP Scan does not report CVEs, vulnerabilities or misconfigurations. Use Vulnerability Scan for those.

```{figure} ../../screenshots/network-intel-ip-scan-20260326.png
:alt: Network Intel IP scan
:width: 100%

IP-scan result view with service and infrastructure context for a resolved address.
```

#### Vulnerability Scan

Vulnerability Scan reviews security issues for a supplied target and includes:

- progress feedback
- elapsed time
- downloadable report output
- cancel support during scanning

After a target is resolved, the vulnerability view lists the primary domain and any discovered subdomains as selectable targets. Each target can be scanned with a depth level before opening the target result:

| Depth | Intended use | Tool coverage shown in the UI |
| --- | --- | --- |
| `Low` | fast first-pass review when the analyst needs quick signal | URL probes and heuristic checks |
| `Medium` | broader validation when the target needs passive security review | URL probes, heuristic checks, ZAP passive checks, and CVE lookup |
| `High` | deeper review when the analyst accepts a longer scan | URL probes, ZAP spider, heuristic checks, ZAP passive checks, and CVE lookup |

Changing the depth prompts the user to confirm the scan before the selected target is run. The selected depth is stored per target in the current view, so one target can be scanned at `Low` while another target is reviewed at `Medium` or `High`.

Completed vulnerability results can show severity summary cards, extracted response details, scanned URLs, request metadata, and individual findings. Findings may include title, category, risk, description, affected URL, reference URLs, and evidence snippets when those fields are returned by the scanner.

```{figure} ../../screenshots/network-intel-vulnerability-scan-20260326.png
:alt: Network Intel vulnerability scan
:width: 100%

Vulnerability-scan result view with severity summary and findings.
```

#### Common Toolbar Features

The Network Intel toolbar can include:

- query input
- status indicators
- result count
- elapsed time
- download report
- cancel current run
- optional geo search support for relevant views

Geo support is especially relevant when working from host-oriented results and wanting to pivot from a location or coordinates into nearby IP discovery.

```{figure} ../../screenshots/network-intel-geo-modal-20260326.png
:alt: Network Intel geo modal
:width: 100%

Geo-assisted pivot modal used from network results.
```

### Satellite Intel

Satellite Intel is Orion's geo-fencing map workspace for infrastructure, facilities, transportation tracking, and satellite imagery review. It combines a Leaflet map, indexed map entities, nearby facility discovery, live aircraft and ship overlays, and comparison imagery in one operational view.

Satellite Intel can be opened from the sidebar as `Satellite Intel`. It is also embedded inside the consolidated results `Geo Fencing` tab, where the top toolbar can switch between `Satellite Map` and `Threat Lens`.

```{figure} ../../screenshots/satellite-map-overview-20260326.png
:alt: Satellite Map overview
:width: 100%

Satellite Map overview with indexed map entities, facility filters, search, tracking controls, selection state, and the map renderer.
```

#### Access and Licensing

The sidebar entry is available to admins and users with the `osint_advanced` module. If the module is unavailable, the sidebar entry remains gated by the subscription prompt.

Satellite Intel can be opened from the Geo Fencing sidebar entry or from consolidated geo-fencing flows.

The embedded view exposes the `Satellite Map` and `Threat Lens` toggle. The map view keeps its own panel menu, layer switcher, facility dashboard, imagery-analysis panel, location modal, and tracking overlays.

#### Map Renderer and Layers

The map renderer uses Leaflet. The `Street` layer uses the Carto Voyager tile set, while the `Satellite` layer uses ArcGIS World Imagery.

Map behavior includes:

- world-bounds limiting so the map does not wrap horizontally
- dynamic minimum zoom based on the rendered container
- map movement events that update the active viewport
- feature focusing from search results
- selected-location rendering after a geocode or coordinate lookup
- marker sizing refresh after zoom changes
- sidebars for aircraft and ship details

```{figure} ../../screenshots/satellite-map-satellite-layer-20260326.png
:alt: Satellite Map satellite imagery layer
:width: 100%

Satellite imagery layer selected from the map layer control.
```

#### Indexed Map Entities

On load, the dashboard requests indexed map entities and converts them into map features with name, type, source, coordinates, optional capacity, and a stable feature identifier.

The dashboard can show power and infrastructure facility categories, including:

- hydro
- solar
- wind
- gas
- coal
- oil
- nuclear
- geothermal
- biomass
- waste
- storage
- cogeneration
- petcoke
- wave and tidal
- airport
- port
- warehouse
- industrial
- military
- other

The `All Facilities` panel shows loaded and visible counts. Users can select all categories, clear all categories, or toggle individual categories to control which indexed points render on the map.

#### Search and Selection

The dashboard search box filters loaded map entities and nearby facilities. Selecting a result focuses the map on that feature and updates the `Selection` panel.

The selection panel can show:

- facility or entity name
- normalized type
- source, such as `WRI` or `OSM`
- capacity in megawatts when available
- coordinates in latitude and longitude form

#### Location Search and Nearby Facilities

The `Location` button opens the shared geocode modal. Users can search for a place, enter coordinates, adjust the map coverage delta, and apply the location to the Satellite Map.

```{figure} ../../screenshots/satellite-map-location-modal-20260326.png
:alt: Satellite Map location modal
:width: 100%

Location modal used to scope Satellite Map facilities and tracking overlays.
```

After a location is applied, Satellite Intel:

- focuses the map on the selected coordinates
- records the active viewport
- loads nearby facilities for the selected viewport
- refreshes enabled aircraft and ship tracking against the scoped viewport
- enables the location-target control so the user can return to the selected location

Nearby facilities are normalized into the same map-feature shape used by indexed entities. Point, line, polygon, and multipolygon geometries are converted into renderable coordinates. Facility kinds are normalized into Orion map categories such as airport, port, warehouse, industrial, military, solar, wind, hydro, coal, gas, oil, storage, and other.

```{figure} ../../screenshots/satellite-map-location-facilities-20260326.png
:alt: Satellite Map nearby facilities
:width: 100%

Nearby facilities loaded for a selected location, with facility counts and category breakdowns.
```

#### Aircraft and Ship Tracking

The `Tracking` panel controls live transportation overlays.

Aircraft tracking uses the active map bounds and can include OpenSky credentials when configured.

Ship tracking uses the active map bounds. Bounds are clamped to valid latitude and longitude ranges, and the request can include an AISStream API key when configured.

Tracking behavior includes:

- separate toggles for `Aircraft` and `Ships`
- loading indicators per tracking source
- visible counts in the tracking buttons
- matching aircraft and ship counts in the facilities summary
- marker rendering on the map
- detail sidebars when a tracking marker is selected
- aircraft detail lookup by ICAO
- aircraft track lookup
- ship detail lookup by MMSI
- viewport refreshes for ships after the map moves

If a tracking feed is pending or busy, the polling helper keeps waiting. If a feed returns an error, the dashboard shows the tracking-specific error while preserving the rest of the map context.

```{figure} ../../screenshots/satellite-map-tracking-20260326.png
:alt: Satellite Map aircraft and ship tracking
:width: 100%

Aircraft and ship tracking enabled with counts shown in the dashboard panels.
```

#### Imagery Analysis

The panel menu opens `Imagery Analysis`. This view is used for satellite image comparison and anomaly review at a selected location.

The imagery workflow supports:

- selecting or reusing a location
- choosing an image type from the advanced controls
- choosing a timeline date
- resetting the date to the default
- loading a comparison set
- opening generated images in a lightbox

When `Load comparison` is clicked, the view runs a combined comparison flow. If no explicit month is selected, Orion can also request a year-ago image for comparison. Anomaly analysis runs against the selected imagery set.

The result panel can show:

- number of comparison images loaded
- image labels for each returned month
- anomaly alert level
- NDVI delta score
- scan coordinates
- month count for the anomaly scan
- empty-image and failed-request states

```{figure} ../../screenshots/satellite-map-imagery-analysis-20260326.png
:alt: Satellite Map imagery analysis
:width: 100%

Imagery Analysis panel with comparison output and anomaly summary for the selected map location.
```

#### Empty and Error States

Satellite Intel keeps map and dashboard state visible while individual data sources load or fail.

Common states include:

- the main loading overlay while large map or entity requests are in progress
- `Select location to load facilities` before nearby facility lookup
- `Loading facilities...` while a facility request is running
- `No facilities found` when a scoped lookup returns no renderable records
- request-failed messaging in Imagery Analysis
- aircraft and ship feed warnings beside the affected tracking control

Clearing the selected location resets the focused feature, selected feature, nearby facilities, tracking data, and location overlay while keeping the base indexed map entities available.

### Geo Fencing Threat Lens

Threat Lens is the geo-fencing threat-intelligence workspace. It turns consolidated threat records into a country-oriented map, overlays category relationships as arcs, and runs an IP exposure scan for the active map scope.

Threat Lens can be opened directly from the dashboard sidebar as `Threat Lens`. It is also available inside Satellite Intel as the `Threat Lens` tab, where it shares the geo-fencing map workspace without showing the standalone filter button.

```{figure} ../../screenshots/threat-lens-overview-20260326.png
:alt: Threat Lens overview
:width: 100%

Threat Lens overview with map, country ranking, category layers, live feed, archive, and IP scan status.
```

#### Access and Licensing

The sidebar entry is available to admins and users with the `osint_advanced` module. When the module is not available, the sidebar item remains visible but gated by the subscription prompt.

Threat Lens can be opened as its own geo-fencing workspace or from the Satellite Intel tab switcher.

#### Data Request and Filtering

Threat Lens requests consolidated map, feed, and category data based on the currently selected dashboard filters.

Before the request is sent, empty values, default values, empty arrays, and `all` selections are removed. The keyword field `q` and page field are kept so that an empty search can still load the complete Threat Lens dataset.

The standalone filter drawer uses the Threat Lens filter model:

- network type
- date range
- content type
- platform
- platform result count

Changing filters refreshes the Threat Lens search. In the embedded Satellite Intel tab, the parent geo-fencing shell controls the surrounding map toolbar and opens the same filter behavior from its side panel.

```{figure} ../../screenshots/threat-lens-filters-20260326.png
:alt: Threat Lens filters
:width: 100%

Threat Lens filter drawer for network, date, content, platform, and platform-count filtering.
```

#### Consolidated Category Coverage

The implementation reads these consolidated result categories:

- `Leak`
- `Tracking`
- `News`
- `Exploit`
- `Defacement`
- `Chat`
- `Social`
- `Generic`

Each category has its own map color. Result records are deduplicated by hash, document id, id, URL, title, and creation date. If those fields are missing, the raw document body is used as the fallback identity.

Country labels are extracted from the available country and location fields, including `m_country`, `m_country_name`, `m_location`, `country`, and `location`. Comma, semicolon, and pipe-separated values are split into individual countries. Two and three letter region codes are normalized through browser region display names when possible.

The map data builder then produces:

- total result count
- ranked country counts
- per-category country counts
- document country groups used for arc generation
- feed items sorted by timestamp

#### Search Panel

The search panel supports free-text keyword searches and country pivots.

Search actions:

- type a keyword and press `Enter`
- type a keyword and click `Search`
- click a top highlighted country

When the keyword matches a country known by the map layer, Threat Lens converts the search into a country-filtered request. In that case it sends an entity filter for `m_country`, enables strict matching, disables full search, and focuses the country on the map. For other keywords, the value is sent as `q`.

```{figure} ../../screenshots/threat-lens-search-20260326.png
:alt: Threat Lens search
:width: 100%

Threat Lens keyword search with active keyword state and refreshed country/category context.
```

#### Map, Countries, and Arcs

The map renderer uses ArcGIS SceneView with a global dark basemap. It loads a country feature layer, highlight styling, tooltip handling, arc graphics layers, and IP marker layers.

The country layer provides the selectable geographic surface. Hovering a country shows the country name, total count, and category breakdown. Clicking a country selects it, focuses the map on the country geometry, updates the summary panel, and starts a country-scoped IP exposure scan when boundary data is available.

Threat arcs are generated from records that mention more than one country. The renderer builds animated connections between country pairs, groups them by category color, and rotates visible arcs in batches of up to five. When a country search is active, the map shows only arc connections linked to the selected country.

The renderer also watches zoom and interaction state:

- close zoom switches to a street-oriented night basemap
- map movement pauses arc animation while interacting
- completed navigation can request a new viewport IP scan
- resize handling keeps the scene stable inside dashboard layouts

Documentation capture can use a fallback map state so screenshots remain stable while preserving the same visible panel flow.

#### Summary Panel

The summary panel reports the active Threat Lens state:

- selected country, when one is selected
- current status message
- visible arc count
- per-category selected-country breakdown
- IP scan status, scope, range, and marker count

Both the search panel and summary panel can be collapsed to clear map space.

#### News Feed and Archive

Threat Lens converts result documents into feed cards. Each card can include title, summary, source link, date, category label, category color, and up to four highlights such as platform, risk, channel, attacker, IOC, CVE, or content type.

There are two feed panels:

- `News Feed` shows only `News` category records
- `Archive` shows leak, tracking, exploit, defacement, chat, social, and generic records

Feed controls:

- collapse or expand each feed
- local text search inside the loaded feed records
- range filtering for `1 Day`, `1 Week`, and `All Time`
- auto-scroll while the pointer is away
- temporary pause during hover, wheel, or touch interaction
- safe link opening for HTTP and HTTPS source URLs

The feed range buttons filter data already loaded into the browser. The side filter date range requests refreshed data for the selected time window.

```{figure} ../../screenshots/threat-lens-feeds-20260326.png
:alt: Threat Lens feeds
:width: 100%

Threat Lens feed panels with local archive search and feed range filtering.
```

#### IP Exposure Scan Overlay

Threat Lens automatically uses the Network Intel geo scanner to look for exposed IP-backed camera or IoT records near the active map scope.

Default behavior:

- initial coordinates are `20, 0`
- default radius is `12,000 km`
- default max IP count is `200`
- the summary label is `Global view`

Viewport and country behavior:

- map movement can request a viewport-based scan
- country selection changes the scope to the selected country
- country boundary data is passed to marker rendering when available
- repeated scans with the same scope, center, and radius are deduplicated

The scan posts coordinates, radius, and max-IP count through the Network Intel geo-camera scan flow. Completed results are normalized from returned IP arrays or camera arrays, limited to renderable records, and displayed as map markers. Selecting an IP marker opens the Threat Lens IP detail popup.

The IP scan panel shows:

- running, ready, complete, or error state
- marker count
- scope label
- radius label
- progress/status text
- previous markers kept when a later scan returns no renderable records

#### Empty and Error States

If the Threat Lens data request fails, the map is cleared and the status message explains that the data source could not be loaded. If records load but no country metadata is present, the workspace reports the loaded record count and explains that no country highlights were found.

If records contain countries but no multi-country co-occurrence, the country ranking still appears while the arc count remains zero.

If an IP exposure scan fails, the IP scan panel changes to the error state and preserves the map context.

## Graph Investigation Modules

### CTI Graph

CTI Graph is the relationship-mapping module for cyber threat intelligence pivots.

It opens in its own tabbed workspace and supports multiple sessions.

Key concepts:

- `Cluster` nodes
- `Document` nodes
- `Property` nodes
- grouped nodes
- directional connections

#### Core CTI Features

- session tabs
- sidebar filters
- graph and list views
- node search and highlighting
- physics toggle
- expand or collapse controls
- right-side listings panel
- import and export support
- report export

The listings panel provides a document-oriented summary of the current graph state, while the legend explains node and edge types.

```{figure} ../../screenshots/cti-graph-20260326.png
:alt: CTI graph workspace
:width: 100%

CTI graph workspace with filter controls, graph canvas, listings, and session actions.
```

#### Advanced Graph Builder

The Advanced Graph Builder is used when a single CTI filter is too broad. It opens from the CTI Graph advanced filter control and lets the analyst combine multiple graph fields in one search.

The builder supports:

- up to eight filter rows
- searchable field selection
- `AND` and `OR` joins after the first row
- direct text values for property-style fields
- searchable cluster values for cluster-style fields
- removing individual rows
- clearing the active builder chips after execution

The first row acts as the initial condition. Additional rows refine or broaden the graph query depending on the selected join operator. Use `AND` when all conditions should be present in the graph result, and use `OR` when any of the selected conditions should be enough to bring related nodes into the result.

Typical Advanced Graph Builder workflow:

1. Open `CTI Graph`.
2. Expand the advanced builder control.
3. Choose the first field, such as country, actor, IP, domain, cluster, or another available graph field.
4. Enter a value or choose a cluster value.
5. Add another row when the investigation needs a second condition.
6. Choose `AND` or `OR` for the new row.
7. Execute the search.
8. Review the generated filter chips and graph result.
9. Clear the builder chips when returning to a broader graph view.

The builder is useful for questions such as:

- show graph records tied to a country and a specific infrastructure value
- find relationships that match either of two indicators
- narrow a noisy graph to a cluster plus one supporting property
- prepare a cleaner graph before exporting JSON or a PDF report

The tested CTI workflow also confirms the following operator-visible actions:

- switching filter type to `Cluster`
- applying graph filters
- searching and highlighting matching nodes
- switching between graph and list views
- collapsing and reopening the listings panel
- toggling physics simulation
- opening the Advanced Graph Builder
- adding multiple builder rows
- joining builder rows with `OR`
- executing builder filters and clearing generated filter chips
- creating, renaming, importing, exporting, and closing sessions
- exporting report options such as JSON and graph PDF
- opening canvas context-menu actions

```{figure} ../../screenshots/cti-export-modal-20260326.png
:alt: CTI export modal
:width: 100%

CTI export modal with tested report-export options such as JSON and graph PDF.
```

```{figure} ../../screenshots/cti-context-menu-20260326.png
:alt: CTI context menu
:width: 100%

CTI graph context-menu actions opened directly from the graph canvas.
```

### Social Intel

Social Intelligence has three workspaces. **Profile Intelligence** is the profile-search and investigation workspace at `/dashboard/social-intel`. **Social Monitoring** opens **Manage Profiles** at `/dashboard/manage-profiles`, where you manage captured browser sessions, personas, monitors, and run results. **Identity Resolution** at `/dashboard/identity-resolution` compares two profiles for impersonation.

| Workspace | Use it for |
| --- | --- |
| Profile Intelligence / profile search | Find accounts from a username or image, inspect saved and live profile data, and explore relationships. |
| Social Monitoring / Manage Profiles | Capture platform sessions, create personas and monitors, run posting, ad, hate-speech, or keyword monitoring, and review results. |
| Identity Resolution | Compare a reference profile with a suspected impersonator and review the impersonation-risk and same-person scores with their evidence. |
| Social indexed search | Search already indexed social content. This is separate from profile discovery and monitoring. |

The **Manage Profiles** candidate popup inside profile search selects discovered profiles for an investigation. It is different from the full **Manage Profiles** monitoring page.

(profile-search-profile-intelligence)=
#### Profile Search (Profile Intelligence)

1. Open **Social Intelligence > Profile Intelligence**.
2. Enter a username or handle and select **Search**, or use **Scan by image** to upload a PNG, JPEG, WebP, or GIF.
3. Follow the scan in **Scan History**. Open a completed entry to return to its results; use the available cancel, retry, or delete controls on the relevant entry.
4. Review the discovered platforms. When candidate selection is offered, open the **Manage Profiles** popup, filter platforms or usernames, and select the profiles to investigate.
5. Select a platform to open its profile. Use **Open Profile** to compare the result with the source account.
6. Review **Details**, **Online Presence**, **Stealer Logs**, and the platform's supported content or relationship tabs. A tab is available only when the platform and returned data support it.
7. Use the **Relationship graph** button to explore connections, or return to the profile list for detailed review. Where **Normal** and **Darkweb** source tabs appear, use them to switch result sources.

A matching username is a candidate identity, not proof that two accounts belong to the same person. Compare profile links, names, images, published content, and relationships before treating a match as confirmed.

#### Saved Results, Live Fetching, and Login

A discovery scan can save basic profile details, online-presence matches, and exposure results before a live browser fetch succeeds. These saved results remain visible when available, even if the extension cannot fetch additional data. A failed live fetch does not erase them.

Live fetching needs the Orion browser extension and, for platforms that require authentication, a valid login to that platform in the same browser. When **Login required** appears:

1. Select **Login** to open the platform in a separate tab.
2. Complete the platform's login, then return to Orion.
3. Select **Reload** for the affected section after login has completed.
4. Continue reviewing saved results while live data is unavailable.

The **Details**, **Online Presence**, and **Stealer Logs** panels have their own reload controls. Content and relationship sections expose their own sync state and supported fetch controls. A running indicator means work is still in progress; it does not mean the displayed saved data has already been refreshed. A profile with no saved details or enrichment still shows the fetch prompt or error until data becomes available.

| What you see | What to do |
| --- | --- |
| Extension install or update prompt | Follow the browser-specific instructions, then return to the profile. |
| Login required with saved results below | Log in to the source platform and reload the section; the saved results can still be reviewed. |
| No saved results and a fetch error | Check the extension, platform session, and profile URL, then retry. |
| Empty or unavailable content tab | Check whether that platform supports the section and whether the source returned any data. |

#### Exposure Results and Exports

The dashboard's **Exposure Signals** panel shows stealer-log matches for the investigated identity across domains. Inside an individual platform, **Stealer Logs** narrows those matches to that platform's domain when its host can be resolved. These two views can therefore show different counts.

Where an export menu is available, choose PDF, CSV, or JSON. Stealer-log exports use the credentials report layout: each source record retains its labeled fields instead of being squeezed into one wide table. The selected view determines the records included. Treat a saved exposure record as evidence to review, not as confirmation that a credential still works.

#### Social Intel Layout

The workspace includes:

- a tab bar for multiple social-analysis sessions
- a collapsible left home menu for created scans and saved jobs
- a graph toolbar for search, mode switching, export, and scan actions
- a central graph canvas or list view
- modal workflows for profile management, metadata, aliases, and follower scans

This is not a single-screen graph. It is a multi-state workspace where the user can move among:

- graph view
- list view
- summary popups
- profile-management modals
- follower/following import popups
- metadata search results

#### Core Entry Points

Social Intel supports several starting paths:

- direct username scanning
- image-based and reverse image profile discovery
- manual custom-entity entry
- guided entity lookup submission from the add-entity modal
- reopening previously created scan jobs from the left home menu

This makes Social Intel useful for both:

- known-profile investigations
- unknown-profile discovery from an uploaded image

```{figure} ../../screenshots/social-intel-20260326.png
:alt: Social Intel workspace
:width: 100%

Social Intel graph workspace used for username and relationship mapping.
```

#### Graph and List Views

The graph view is intended for structural relationship analysis. The list view is intended for profile-by-profile inspection and management.

Common view actions include:

- switching between graph and list views
- searching within the graph toolbar
- clearing graph search input
- enabling or disabling graph physics where available
- opening relationship popups directly from graph nodes
- opening list rows to review platform-specific detail

Use graph view when you want to understand how entities connect. Use list view when you want a more structured review of profiles, links, summaries, and platform records.

The graph is a view of the Social Intel dashboard rather than a separate page. The round diagram button beside the gear switches between the profile list and the graph, and lights up while the graph is shown. Everything around it — scan history, scan box, breadcrumb — stays in place.

In graph view:

- clicking a scan in the left history adds that user's node instead of opening the profile
- the `Find a username in this graph...` box lists only users already in the graph, and offers `Add @handle` for one that is not
- each account node carries badges for the relationship sets found on it, such as `Followers` and `Commenters`
- right-clicking an unscanned contact offers `Scan @handle`, which starts a normal scan and refreshes the graph when it finishes
- removing a user is done from its own node panel

The set of users in the graph is saved per account, so reopening Social Intel restores the same relationship picture.

```{figure} ../../screenshots/social-relationship-graph-20260326.png
:alt: Social Intel relationship graph view
:width: 100%

Relationship graph view showing a scanned account, its platform node, and follower and commenter badges.
```

#### Session Management

Use the left **Scan History** to reopen saved investigations and the graph controls to retain the relationship view for your account. Reopening a saved scan is different from capturing a browser login session: browser sessions are managed under **Social Monitoring > Sessions** and are used to authenticate platform requests.

Export the current investigation from the available report controls when you need to share its findings. A report export is not a browser session file or a tenant backup.

#### Add-Entity Workflow

The add-entity modal supports more than one submission mode.

Available tested behavior includes:

- opening an entity type such as `Phone`
- using an API query mode
- validating that the submit button stays disabled until a valid input is present
- switching from API mode to manual mode
- entering a manual value
- submitting the new entity into the social workspace

This matters because Social Intel is not limited to scraped social accounts. It can also be used to place analyst-defined entities into the investigative graph.

#### Image-Based Profile Discovery

The image-based workflow is one of the more advanced Social Intel paths.

It supports:

- uploading an image
- waiting for image recon processing
- opening the manage-profiles modal
- filtering candidate platforms
- reviewing discovered usernames
- opening direct profile links for discovered accounts
- fetching the selected profile into the workspace
- reopening completed profile jobs from the left home menu
- selecting multiple discovered profiles and updating the graph with them

Use this workflow when a screenshot, avatar, or reused profile image is the starting point instead of a known handle.

#### Manage Profiles Modal

The manage-profiles modal is the main control surface for discovered or queued profile candidates.

From this modal, users can:

- filter platforms
- search usernames
- review discovered profile links
- fetch profile data
- select all fetched profiles
- update the graph with the selected profiles
- cancel without applying changes

This modal is central to the Social Intel workflow and should be treated as part of the main graph system, not as a secondary helper.

Profile detail panels can expose platform-specific sections for profile search results, posts, videos, shorts, followers, following, images, online presence, metadata, wanted-list context, forum profile matches, saved profile candidates, and stealer-log matches. Availability depends on the platform and on which data was found for the selected username.

Use the profile detail panels to move from broad identity mapping into source-by-source review. Posts, videos, and shorts help review published content; followers and following show relationship context; images and online presence help validate whether the same identity appears elsewhere; stealer-log and wanted-list sections highlight risk context that may require a case or tenant alert follow-up.

```{figure} ../../screenshots/social-manage-profiles-20260326.png
:alt: Social Intel manage profiles modal
:width: 100%

Manage-profiles modal used to filter, inspect, fetch, and push discovered accounts into the graph.
```

```{figure} ../../screenshots/social-intel-list-view-20260326.png
:alt: Social Intel list view
:width: 100%

Social Intel list-view mode for profile-by-profile review after graph ingestion.
```

#### Browser Extension and Captured Sessions

Live profile fetching runs through the Orion browser extension. Saved recon results can remain available when live fetching fails; profiles with no saved data still need a successful fetch. When it is missing, Social Intel and the Manage Profiles page show an install prompt: Firefox installs the signed build in one click, while the Chrome package is downloaded and loaded manually from `chrome://extensions` with Developer mode on.

```{figure} ../../screenshots/social-extension-install-20260326.png
:alt: Orion extension install prompt
:width: 100%

Install prompt shown while the Orion extension is not available to the browser.
```

The **Sessions** tab on the Social Monitoring / Manage Profiles page lists the platforms returned by the extension and the number of sessions saved for each. From here users can fetch a session for a platform, and expand a platform to verify, re-capture, or delete an individual saved session.

```{figure} ../../screenshots/social-manage-profiles-page-20260326.png
:alt: Manage Profiles captured sessions
:width: 100%

Manage Profiles page listing supported platforms and their captured session counts.
```

#### Summary Popup and Metadata Search

The summary popup provides deeper profile inspection beyond the main graph or list node.

Supported summary actions include:

- opening the summary popup from a list entry
- reviewing all detected platforms for the selected subject
- opening `Profile Metadata Results`
- entering metadata search tokens
- validating no-token error states
- running metadata searches with terms such as leaked email or other keywords
- reviewing returned external links
- pivoting into a selected platform from within the popup
- reopening external profile links

This popup is where profile-level enrichment becomes operationally useful. It combines raw profile context with searchable metadata and quick pivots.

```{figure} ../../screenshots/social-summary-popup-20260326.png
:alt: Social Intel summary popup
:width: 100%

Summary popup used for platform review, enrichment actions, and detailed subject inspection.
```

```{figure} ../../screenshots/social-metadata-results-20260326.png
:alt: Social Intel metadata results
:width: 100%

Metadata-search results inside the Social Intel summary workflow.
```

#### Followers, Following, and Connections

The followers/following workflow is more than a read-only count view.

Covered actions include:

- opening the followers-and-following popup
- switching among `Followers`, `Following`, and `Connections`
- filtering discovered related accounts
- fetching more followers from inside the popup
- selecting discovered related accounts
- confirming selection to import those accounts back into the main workflow
- reopening created follow-based jobs from the left home menu
- selecting all imported results and updating the graph

In practice, this means Social Intel can expand an investigation outward from one profile into a broader relationship set rather than staying limited to the original target.

A `connection` is someone who **engaged** with the profile, not someone who follows it. Orion reads the commenters off each of the profile's own posts and deduplicates them into a single people list, remembering which posts each person commented on. One handle therefore appears once in `Connections` even when it commented on several posts, and the count of those posts is kept with it.

The graph carries the same relation. Commenters hang off the account node as a `Commenters` badge; an individual commenter's edge reads `commented on @handle`, and the edge weight and its node panel show how many distinct posts that person commented on. When a commenter is also a scanned user, the two users are joined through the platform node they share. One that has not been scanned yet can be added to the graph as its own user, or scanned directly from its node.

Each fetch section keeps its own state: a band above the results shows whether the section is up to date and when it was last synced, `Sync all` refetches it, and the section resumes on its own if the page is reloaded while a fetch is still running.

```{figure} ../../screenshots/social-followers-popup-20260326.png
:alt: Social Intel profile fetch tabs
:width: 100%

Profile fetch tabs with the per-section sync band, alongside exposure, wanted-list, and phone-lookup panels.
```

#### Images, Followers, and Re-Scan Controls

Within the summary popup, the suite covers several enrichment actions:

- `Fetch Followers`
- `Fetch Following`
- `Fetch Images`
- `Re-scan profile`

These actions make the popup a live enrichment console rather than only a static summary.

#### Aliases and Context Menus

The graph canvas supports right-click or context-menu style interaction paths.

Covered behavior includes:

- triggering a context menu from the canvas
- opening the `Set Alias` action
- editing an alias value
- saving the alias
- seeing the alias reflected in later list or summary views

Aliases are useful when the analyst wants a cleaner investigation label than the raw discovered username.

#### Relationship Popups

Social Intel also supports relationship-specific popups from graph nodes.

These popups can expose:

- related-account information
- external account links
- quick-close controls

This makes it possible to inspect a connection without leaving the graph canvas.

#### What the Legend Represents

The legend distinguishes visual object types such as:

- user profiles
- platforms
- platform groups
- custom entities
- relationship or connection types

Understanding the legend is important when the graph becomes dense. It tells the user whether they are looking at:

- a discovered profile
- a platform wrapper
- a manually added entity
- or a relationship generated by enrichment

#### Recommended Social Intel Workflow

1. Start with a known username or an uploaded image.
2. Fetch the initial profile set into the workspace.
3. Review the manage-profiles modal and push selected profiles into the graph.
4. Switch between graph and list views as needed.
5. Open the summary popup for metadata and platform review.
6. Fetch followers, following, or images where useful.
7. Rename aliases or add custom entities if the graph needs cleanup.
8. Export the session when the relationship picture is complete.

### Social Monitoring

Open **Social Intelligence > Social Monitoring** (`/dashboard/manage-profiles`). The page heading is **Manage Profiles**. It has three tabs:

| Tab | Purpose |
| --- | --- |
| Sessions | Capture, verify, replace, upload, download, or delete browser sessions for supported platforms. |
| Monitoring | Add and manage monitors: personas, posting and ad-monitoring accounts, and hate-speech or keyword monitors for your own or external profiles. |
| Results | Review running and completed activities, inspect findings, stop active runs, and clear saved results. |

Install or update the extension when prompted. **Monitoring** and **Results** become available once the extension is ready and the platform list has loaded successfully. While the extension is not ready, the page returns to **Sessions**.

#### Supported Platforms

The platform list comes from the extension and is limited to the platforms that support automation: Facebook, Instagram, X, Threads, Reddit, Pinterest, YouTube, TikTok, Quora, Patreon, OK.ru, Hashnode, Behance, and MeWe. X is listed as `x`; older Twitter entries are not offered. Other platforms reported by the extension are not shown on this page. Posting is not available on MeWe and Behance, so their monitors do not show the **Post** control.

(capture-and-maintain-browser-sessions)=

#### Capture and Maintain Browser Sessions

1. In **Sessions**, find the platform and use `Fetch session` while signed in to the intended platform account in that browser. A successful capture is verified automatically.
2. Expand the platform row to inspect its saved sessions, capture time, verification state, and detected username. A session used by a monitor shows `Already in use`; hover over it to see which monitor uses it.
3. Use `Verify` to check a session. A successful check shows the detected account; a failed check shows the error to resolve before monitoring.
4. Use `Edit` to capture a replacement session when the login expires or the wrong account was captured.
5. Use `Upload` on the platform row to import a saved session file (`.zip` or `.json`), or `Download` on a session to export it.

The dot next to each platform and session shows its state: green for verified, red for a failed verification, and grey for a session that has not been checked yet.

Up to **10 sessions per platform** can be saved. When the limit is reached the fetch button reads `Max reached`; delete an unused session before capturing another. Session files contain authentication material; keep downloaded copies private. Deleting a session asks for confirmation. When monitors use it, the confirmation states how many; deleting it removes the session from those monitors and marks them disconnected, so capture or attach a replacement before running them again.

#### The Monitoring Tab

**Monitoring** lists your monitors in two groups:

| Group | Contains |
| --- | --- |
| Private Monitors | Posting accounts, monitors that use your persona's own connected account, and personas that are not yet assigned to an account. |
| External Monitors | Hate-speech and keyword monitors that watch someone else's profile through its URL. |

Each monitor row shows the persona or label name, an activity badge (`Post`, `Ad Monitoring`, `Hate Speech Monitoring`, or `Keywords Monitoring`), `Cron: On` when daily runs are enabled, the connection status, the platform, the attached session, and the monitored profile URL for external monitors. Warning badges appear when something is missing:

- `No session attached`: the monitor cannot run until a session is attached. Use `Attach session` to attach a verified session for that platform that no other monitor with the same activity is using. The button is disabled when no such session exists; capture one in **Sessions** first.
- `No persona assigned`: a posting monitor needs a persona before it can post.

The row controls depend on the activity:

| Control | Shown for | Action |
| --- | --- | --- |
| `Post` | Posting monitors on platforms that support posting | Publishes a post through the connected account. Requires an assigned persona and an attached session. |
| `Ad` | Ad-monitoring monitors | Starts ad detection with the attached session. |
| `Scan` | Hate-speech and keyword monitors | Starts a hate-speech or keyword scan of the monitored profile. |
| Edit (pencil) | Posting and external monitors | Opens the monitor for editing. |
| View details (eye) | Monitors that use your persona's own account for ad, hate-speech, or keyword monitoring | Opens the monitor read-only. These monitors cannot be edited; delete and recreate them to change settings. |
| Delete (bin) | All monitors | Deletes the monitor after confirmation. |

Unassigned personas appear under **Private Monitors** with their age group, gender, location, and interests, and can be edited or deleted. While a scan runs, the monitor row shows the number of running scans and the current step.

#### Add a Monitor

Select `Add a Monitor` to open the monitor drawer. Choose the **Account** first, then the **Activity Type**:

| Account | Meaning | Available activities |
| --- | --- | --- |
| `Persona` | Uses your persona and its connected account. No URL is needed. | `Posting`, `Ad Monitoring`, `Hate Speech Monitoring`, `Keywords Monitoring` |
| `External Account` | Monitors an external or target profile through its URL. | `Hate Speech Monitoring`, `Keywords Monitoring` |

A new monitor starts with `Persona` and `Ad Monitoring` selected. Switching to `External Account` while `Posting` or `Ad Monitoring` is selected changes the activity to `Hate Speech Monitoring`. Switching back to `Persona` clears the profile URL.

Complete the **Account Details**:

1. **Platform**: select a supported platform.
2. **Session**: select a captured session for that platform. Sessions already used by another monitor with the same activity are marked `(Already in use)`, and a warning offers `Go to Sessions` to capture another. When the platform has no sessions, the drawer shows `Go to Sessions` instead of the list.
3. **Profile URL** (`External Account` only): enter the full profile URL of the account to monitor. The URL must belong to the selected platform and point to a profile, not a post, video, or system page; a mismatch shows an explanation under the field.
4. **Label**: a name that identifies the monitor in lists and results.
5. **Target Keywords** (`Keywords Monitoring` only): type a keyword and press Enter or select `Add`. At least one keyword is required. Remove a keyword with its chip's remove control.
6. **Automated Cron Job**: turn on to include this monitor in the scheduled daily run.

With the `Persona` account, also complete the **Persona Details**:

- **Persona Name**, **Age Group**, **Gender**, and **Interests** (select up to three) are required.
- **Country** and **City** are optional.
- **Adult Status** is derived from the age group: `13-17` is shown as Minor, all other groups as Adult.

Select `Save`. Orion creates the persona first and then the monitor with that persona assigned. If a required field is missing, the drawer shows the first problem, for example `Age group is required`, `Label is required`, `Platform and Profile URL are required`, `Select a platform and available session`, or `At least one keyword is required for keywords monitoring`.

```{admonition} One account per persona
:class: note

A persona can be assigned to only one monitor. Saving a second monitor with the same persona is rejected with "This persona is already assigned to another profile". Removing a persona clears its assignment; it does not create a replacement persona.
```

#### Create a Persona Without an Account

To prepare a persona before an account is available, open `Add a Monitor`, keep the `Persona` account, set **Activity Type** to `Posting`, fill in the **Persona Details**, and save without selecting a platform. The persona appears under **Private Monitors** as unassigned. Persona-only saving is available only with the `Posting` activity; other activities require a platform and session.

#### Run Monitors Manually

- **Post** publishes content through the connected account. It is an external posting action, not a read-only scan. It requires an assigned persona, a readable session, a supported platform, and content available for that persona's settings. A profile can have one successful manual post per UTC day.
- **Ad** starts ad detection using the monitor's session. Review the detected ads in **Results**.
- **Scan** on a hate-speech monitor collects the monitored profile's posts and classifies each as **Hate Speech** or **Normal**.
- **Scan** on a keyword monitor collects the monitored profile's posts and marks those that contain any target keyword as **Keyword Matched**. Keyword monitoring does not run the hate-speech classifier.

If the application reports that no post content is available, adjust the persona's age group or interests to a supported content set. If a session cannot be read or has expired, capture it again and reattach it. Manual triggers are refused while the daily scheduler is running; wait until it finishes before retrying.

#### Scheduled Daily Runs

The daily scheduler processes only monitors with **Automated Cron Job** turned on. A monitor is skipped when it has no attached session, no activity, or a session that cannot be read; posting monitors are also skipped when they have no assigned persona. Monitors without the cron job enabled run only when triggered manually.

#### Results

In **Results**:

- Filter by **Account** and choose what to show: `Ads Monitoring`, `Posts`, `Hate Speech Monitoring`, or `Keywords Monitoring`.
- Results are grouped into `Running now`, `Today`, `Last 7 days`, and `Earlier`. Running activities stay visible in every view.
- Each row shows the account, the persona when one is assigned, whether the run was `Manual` or `Automated`, and the time. Running rows show the current step. Unsuccessful runs show `Failed` or `Session Expired` and the reported reason.
- Expand a row to review ads or posts, source links, classifications, and available engagement statistics. Detected ads show whether they were classified as `Scam` or `Not Scam`, and any extracted indicators under `IOCs`.
- In `Keywords Monitoring`, an expanded row lists the posts that matched, with the matched keywords as chips. Use `Show other posts` to also see the posts that did not match.
- `Posts` lists successfully published posts with their links. Failed post attempts appear as rows with the failure reason.
- Use `Stop` to cancel a manual run. `Stop daily run` requests that the active scheduled run and the remaining daily batch stop; it is broader than cancelling one monitor's scan. Wait for the stopping state to finish. Stopping does not remove content already published or undo completed work.
- Use the bin control on a row to remove that record, or `Clear All ...` (or `Clear Profile ...` when an account is selected) to remove all results of the current view after confirmation. Clearing results is separate from deleting a monitor, persona, or session.

If a scheduled batch was stopped, it is recorded as handled for that day; restarting the backend does not restart the same daily batch. Results and running indicators refresh automatically while the page is open.

### Identity Resolution

**Identity Resolution & Impersonation Detection** compares two social media profiles and estimates whether one is impersonating the other, and whether both belong to the same person. Open **Social Intelligence > Identity Resolution** (`/dashboard/identity-resolution`). The entry is listed below **Profile Intelligence** in the Social Intel group, which is available to administrators and to non-demo users whose licenses include the Social Intel module.

The two profiles can be on the same platform or on different platforms. Supported platforms are Instagram, Reddit, YouTube, TikTok, X (Twitter), Facebook, Pinterest, Threads, Quora, Patreon, MeWe, OK.ru, Hashnode, and Behance.

#### Compare Two Profiles

1. Enter the **Reference** profile URL: the authentic or baseline account.
2. Enter the **Candidate** profile URL: the suspected copycat account being investigated.
3. Check the line under each field. A recognised profile shows the platform and handle, for example `Instagram · @brand`. An unusable link shows the reason instead.
4. Select `Compare Profiles`, or press Enter in either field. The button stays disabled until both URLs are valid profile links for two different accounts.

Use the swap control between the fields to exchange the reference and candidate. Profile pictures are always compared as part of the analysis.

The URL check runs before anything is sent and rejects links that do not point to a user profile, for example:

- a post, reel, pin, or video link instead of the profile
- a subreddit or other community page instead of a user
- a system page such as search, explore, or settings
- a domain that is not one of the supported platforms

When both fields point to the same account on the same platform, a warning explains that the comparison requires two distinct accounts.

#### Read the Verdict

The **Verdict** panel shows two scores from 0 to 100:

| Score | Meaning | Levels |
| --- | --- | --- |
| Impersonation Risk | How likely the candidate is imitating the reference. Higher is more concerning. | `Low` below 40, `Moderate` 40–69, `High` 70 and above |
| Same-Person Likelihood | How likely both accounts are run by the same owner, for example an official secondary account. | Same thresholds |

Each score has a short summary. **Key findings** lists the most important observations behind both scores. A low impersonation risk together with a high same-person likelihood usually indicates a legitimate account owned by the same person or organisation.

#### Side-by-side Profiles

**Side-by-side Profiles** places the collected public data of both accounts next to each other:

- profile picture, display name, verified badge, handle, and platform; select the handle to open the profile
- `Private` or a non-active account status when the platform reports one
- followers, following, and post counts, using the platform's own labels such as Subscribers, Pins, or Videos; a count that could not be collected shows its collection status instead
- bio and external links

#### Structured Evidence Matrix

The **Structured Evidence Matrix** lists every signal used in the assessment. Each entry shows the signal name, its category (Identity, Visual, Content, Network, or Activity), the reference and candidate values, the comparison result, and an explanation. Each entry also shows:

- the direction: `Links accounts` when the signal connects the two profiles, `Sets apart` when it indicates they are different, or `Neutral`
- the strength, from weak to definitive, shown as a bar of up to four segments

Signals include platform scope (same or cross-platform), handle similarity and digit padding, display name, bio similarity and cross-references between the bios, external links, profile picture similarity, verification status, follower and following asymmetry, post history, privacy settings, and parody disclaimers. Signals whose data could not be collected for either profile are skipped.

```{admonition} Interpreting the result
:class: note

Scores are probabilistic indicators derived from public profile metadata. They do not constitute legal identity verification. Review the evidence and both profiles before acting, for example before submitting a takedown. When profile pictures cannot be fetched, or fields are missing, the assessment uses fewer signals.
```

## Result and Report Workflows

Most indexed modules eventually lead into a report page. Report pages are one of the most important parts of the product because they consolidate the searchable record, its metadata, and pivot actions.

### Report Toolbar

The shared report header can expose:

- download
- export report
- translation
- AI summary
- share
- open source URL
- open CTI graph

The exact buttons depend on the record and deployment configuration.

When available, this toolbar is the fastest way to export, translate, summarize, share, or pivot the current record into graph analysis.

Some report pages also show feedback controls. These controls let signed-in users mark a record as recommended, trusted, or untrusted. The counters help teams identify records that have already been reviewed and make it easier to spot material that needs confidence review before it is reused in a case, export, or briefing.

### Result Insights Side Panel

In consolidated workflows, Orion also provides a dedicated insights panel beside the main result stream. This side panel can expose:

- keyword insights
- general coverage summaries
- threat-actor search helpers
- unique URL lists
- expandable extracted-data sections

This panel is intended for quick triage and narrowing before opening individual reports.

In practice, it helps answer three questions quickly:

- what themes dominate this result set
- whether actor- or URL-based pivots are available
- which extracted sections are worth opening in full reports

```{figure} ../../screenshots/consolidated-insights-20260326.png
:alt: Result insights side panel
:width: 100%

Result insights side panel with URL and extracted-data pivots.
```

### General Report Page

The general report view commonly includes:

- title
- description or important content
- web reference
- source URL
- published date
- network
- last-checked date
- content-type tags
- freshness status

Some report layouts also expose quick links, downloadable record output, or direct pivot actions to graph and sharing tools from the same header.

```{figure} ../../screenshots/social-report-20260326.png
:alt: Report content view
:width: 100%

Typical report layout with content and structured context.
```

### Metadata Panel

The metadata panel is expandable and lets users browse extracted values by category. Common tabs include:

- content
- section
- organization
- entity or person
- other extracted attributes

This is the main place to inspect structured extraction results from the record.

```{figure} ../../screenshots/report-json-viewer-20260326.png
:alt: Report metadata sections
:width: 100%

Expandable metadata and extracted-section review.
```

### Screenshot and JSON Sections

For relevant breach records, the report may also include:

- screenshot preview
- JSON record viewer
- report mapping

The JSON viewer is useful for raw structured inspection, while report mapping helps users navigate relationships and related record context.

```{figure} ../../screenshots/report-json-viewer-20260326.png
:alt: Report JSON viewer
:width: 100%

JSON inspection view for raw structured report data.
```

### AI Chat and Summary

Report chat and AI summaries are part of DeepIntel. See [AI Chat and Summary](deepintel.md#ai-chat-and-summary).

### Compromise Monitoring Report Page

The Compromise Monitoring report is a streamlined variant focused on target and attacker context. It includes:

- target URL
- saved date
- defacer or IOC type
- team
- source breach reference
- IP
- location
- metadata panel
- JSON viewer

## Links, Support, and External Navigation

### Directory

The `Directory` page presents monitored live services and related records in a browsing-oriented layout. It differs from the normal search-result workflow by focusing on monitored entries and operational visibility rather than keyword-first investigation.

Common behaviors include:

- page-level filtering
- paginated or progressively loaded directory entries
- monitoring-status style browsing
- service and reference review across monitored live entries

```{figure} ../../screenshots/directory-monitoring-20260326.png
:alt: Directory and monitoring view
:width: 100%

Monitoring-oriented directory workflow.
```

### Links

The `Links` sidebar item acts as the user-facing entry into the directory-style workflow above.

### Onion Link

If configured, `Onion Link` opens the deployment’s onion address in a separate tab.

### Whistle Blowing

The `Whistle Blowing` entry opens the Orion Leaks reporting portal. See [Orion Leaks](orion_leaks.md).

### Documentation

The `Documentation` entry opens the published documentation site in a new tab.

## Profile, Tenant, and Alert Workflows

The user profile area at the top of the sidebar contains user-specific and tenant-specific pages.

```{figure} ../../screenshots/account-settings-20260326.png
:alt: Account settings page
:width: 100%

Profile, settings, and administrative workspace.
```

### Account Settings

The account page allows the current user to review and manage:

- profile image
- username
- role
- tenant or location display
- assigned licenses
- two-factor authentication
- password
- global recovery key
- theme preference

The page also shows the currently running platform version. It is focused on the current user rather than the tenant as a whole.

The tested account workflow also includes:

- avatar upload
- theme toggle and persistence
- enabling `2FA`
- logging out and reaching the two-factor challenge screen on next login
- viewing the QR image and OTP input state for 2FA setup/verification
- changing the account password
- generating or replacing a global recovery key

Changing the password, enabling or disabling 2FA, and generating or replacing a recovery key opens a `Confirm your identity` dialog. Enter the current account password to authorize the change. Orion verifies it on the server against the stored password hash; an incorrect password leaves the dialog open and shows an error. Profile, language, theme, and visibility changes do not require this additional confirmation.

#### Generate or Replace a Recovery Key

1. Open `Profile > Account`.
2. In `Recovery Key`, select `Generate / replace recovery key`.
3. Enter the current password in the identity-confirmation dialog.
4. Copy the recovery key from the popup and store it in a password manager or another secure location.
5. Close the popup after saving the key.

The recovery key is 43 characters and is shown only once. Orion stores only its hash. Generating another key immediately replaces the previous key, so the older value can no longer recover the account.

When 2FA is configured, the authenticator secret is encrypted with tenant-scoped encryption before it is retained. Existing unencrypted 2FA secrets are encrypted after successful verification.

```{figure} ../../screenshots/account-settings-20260326.png
:alt: Account settings form
:width: 100%

Current-user profile and account settings form.
```

(delete-your-account)=

#### Delete Your Account

You can permanently delete your own account from `Profile > Account`. The `Delete Account` card appears at the bottom of the page.

```{admonition} Deletion is permanent
:class: warning

Deleting your account cannot be undone. Your login, Orion Mail mailbox, stored mail, and encryption keys are removed. Export or forward anything you need to keep first.
```

##### Who Can Delete Their Account

| Account type | Can self-delete |
| --- | --- |
| Member or analyst without the `maintainer` license | Yes |
| Tenant maintainer (holder of the `maintainer` license) | No. Maintainer accounts are removed together with their tenant. |
| Platform administrator | No. Administrator accounts cannot be deleted from this page. |

The `Delete Account` card is hidden for administrators and maintainers. If a maintainer or administrator account needs to be removed, contact the platform administrator.

##### Steps

1. Open `Profile > Account`.
2. In the `Delete Account` card, select `Delete Account`.
3. In the `Confirm your identity` dialog, enter your application login password in `Current password`. Do not enter your Orion Vault password.
4. Select `Confirm`.

There is no second confirmation step. Selecting `Confirm` with the correct password deletes the account immediately.

If the password is wrong, the dialog stays open and shows `Invalid password`. If Orion Mail cannot be reached, the dialog shows an Orion Mail error and the account is not deleted; try again later.

##### What Gets Deleted

- your user account and login
- your Orion Mail data: mailbox, stored mail, PGP keys, disposable addresses, and mail keys
- your account encryption key
- your profile image

The deletion is recorded in the tenant audit log as `Account deleted by user`.

##### What Happens Next

- You are signed out straight away and returned to the login page.
- The username and password no longer work, and other open sessions for the account stop working.
- The user slot is freed in your tenant's user quota.
- To use Orion again, ask your tenant maintainer to create a new account. A new account does not restore any deleted data.

### Orion Vault

Orion Vault is a personal credential store inside the profile area. Use it to save website names, addresses, usernames, and passwords. Passwords are encrypted in your browser before they are sent to Orion, so the server stores only the encrypted value.

The vault is protected by a vault password that you create the first time you open the page. The vault password is separate from your Orion login password. Some actions ask for one and some ask for the other, and each dialog states which one it needs.

#### Access

Open `Profile > Orion Vault`. The page is available at `/dashboard/profile/orion-vault`.

`Orion Vault` appears in the profile sidebar for every role: administrators, maintainers, analysts, and members. It does not depend on a license. Each vault belongs to one user account. There is no sharing option, and the vault only ever returns the signed-in user's own credentials.

When you open the page, Orion checks the state of your vault and shows one of three screens:

| State | What you see |
| --- | --- |
| No vault yet | `Welcome to Orion Vault` with the `Set Up Data Protection` dialog. |
| Vault exists and is locked | `Orion Vault is Locked` with the `Unlock Protected Data` dialog. |
| Vault is unlocked | The credentials table, the `Lock Vault` and `+ Add Credential` buttons, and the `Vault Security Settings` card. |

```{admonition} Two different passwords
:class: note

The vault password unlocks Orion Vault and is never sent to the server. Some dialogs call it the security passphrase. The `Set Up Data Protection`, `Unlock Protected Data`, and `Data Recovery` dialogs use the label `Security Passphrase`. The `Change Vault Password`, `Regenerate Recovery Key`, and `Delete Orion Vault` dialogs use `Vault Password`. Both labels mean the same password.

Your application login password is the password you use to sign in to Orion. Orion asks for it before showing a stored password and before changing a stored password. These dialogs say "Enter your application login password, not your Orion Vault password".
```

#### Set Up the Vault

1. Open `Profile > Orion Vault`. The `Set Up Data Protection` dialog opens.
2. In `Security Passphrase`, enter a new vault password.
3. Enter the same password in `Confirm Security Passphrase`.
4. Select `Enable Data Protection`. The button shows `Setting up...` while Orion generates your keys.

`Enable Data Protection` stays disabled until the password meets every requirement and both fields match. If the confirmation does not match, the dialog shows `Passwords do not match.`

The vault password must contain:

- at least 8 characters
- at least one lowercase letter
- at least one uppercase letter
- at least one number
- at least one special character

While you type, a strength bar shows weak, medium, or strong, along with the first requirement that is still not met. A password that meets all requirements is rated medium from 10 characters and strong from 12 characters. Vault passwords are case-sensitive.

If you select `Cancel`, close the dialog, or click outside it, Orion does not create a vault and returns you to the profile homepage.

#### Save the Recovery Key

After setup, the vault unlocks and the `Important: Save Your Recovery Key` dialog opens. The dialog reads "This is your Vault Recovery Key. It will only be shown once. Please save it in a secure location."

1. Select `Copy Recovery Key`, or write the key down by hand. When the copy works, the button changes to `Copied ✓` and the message `Recovery Key copied to clipboard` appears.
2. Store the key in a password manager or another secure location outside Orion.
3. Select the `I have saved this code somewhere safe` checkbox.
4. Select `I have saved it`. This button stays disabled until the checkbox is selected.

The recovery key has 24 characters in six groups of four, separated by hyphens. For example, `XXXX-XXXX-XXXX-XXXX-XXXX-XXXX`. It uses the digits 2 to 9 and capital letters, and it never contains 0, 1, I, or O, which are easy to confuse.

If the browser blocks clipboard access, the dialog shows `Unable to copy. Please copy it manually.` Copy the key from the dialog by hand before you continue.

```{admonition} The recovery key is shown only once
:class: warning

Orion does not store the recovery key in a form it can show you again. If you close the dialog, reload the page, or leave before saving the key, it cannot be shown again. If that happens, unlock the vault and use `Regenerate Recovery Key` to create a new one.
```

#### Unlock and Lock the Vault

To unlock the vault:

1. Open `Profile > Orion Vault`. The `Unlock Protected Data` dialog opens.
2. Enter your vault password in `Security Passphrase`. Select `Show` to check what you typed.
3. Select `Unlock`.

The credentials table loads after the vault unlocks. If you select `Cancel` or close the dialog, Orion returns you to the profile homepage.

To lock the vault, select `Lock Vault` at the top of the page. Orion removes the vault key from the browser, hides any revealed passwords, and returns you to the profile homepage.

The vault stays unlocked while you move between pages inside Orion in the same browser tab. It locks when:

- you select `Lock Vault`
- you reload the page or open Orion in a new tab
- you sign out

The vault does not lock automatically after a period of inactivity.

#### Find Saved Credentials

When the vault is unlocked, the page lists your saved credentials in a table with these columns:

| Column | Content |
| --- | --- |
| `Website Name` | The name you gave the credential. |
| `URL` | The website address. Selecting it opens the site in a new tab. If the saved address has no `http://` or `https://`, Orion adds `https://`. |
| `Username` | The saved username or email address. |
| `Password` | Hidden as dots until you select `Show`. |
| `Actions` | `Edit` and `Delete`. |

Use `Search credentials...` to filter the table. The search matches website name, URL, or username, and it ignores upper and lower case.

If the vault is empty, the table shows `No credentials saved yet.` If nothing matches your search, it shows `No credentials found matching your search.`

#### Add a Credential

1. Select `+ Add Credential`. The `Add Credential` panel opens on the right.
2. Enter `Website Name`, for example the service or account name.
3. Enter `Website URL`.
4. Enter `Username / Email`.
5. Enter `Password`. To have Orion create one, click into the password field and select `Suggest strong password`.
6. Select `Save Credential`.

All four fields are required. Orion encrypts the password in your browser before it saves the credential. The panel then closes, and the new row appears in the table.

`Suggest strong password` creates a random 18-character password. It always includes at least one uppercase letter, one lowercase letter, one number, and one special character. The suggested password is shown in the field so you can copy it into the website you are registering with.

`Website URL` accepts addresses with or without `http://` or `https://`. The address can be a domain name, `localhost`, or an IPv4 address, followed by an optional port and path. Any other value shows `Please enter a valid URL`.

Select `Cancel` or click outside the panel to close it without saving.

#### View a Stored Password

1. In the credential row, select `Show` in the `Password` column.
2. In the `Confirm your identity` dialog, enter your application login password in `Current password`.
3. Select `Confirm`.

Orion checks your login password on the server. Your browser then decrypts the stored password and shows it in the row. Select `Hide` to mask it again. Every `Show` asks for your login password again.

Revealed passwords are hidden again when you lock the vault or leave the page.

Orion Vault has no copy button for stored passwords. To copy one, reveal it and select the text.

#### Edit a Credential

1. In the credential row, select `Edit`. The `Edit Credential` panel opens.
2. Change `Website Name` if needed.
3. Select `Save Credential`.

When you edit a credential, you can change only the website name and the password. `Website URL` and `Username / Email` are read-only. To change either one, delete the credential and add it again.

To change the stored password:

1. In the `Edit Credential` panel, select `Change Password`. The panel notes "Changing credential password requires your application login password confirmation."
2. In the `Confirm your identity` dialog, enter your application login password and select `Confirm`.
3. Enter the new password in `New Password`, or select `Suggest strong password`.
4. Select `Save Credential`.

After you confirm your login password, you have five minutes to save the new password. Each confirmation can be used for one save only. If the time runs out, saving fails with `Verification proof has expired`. Close the panel and start again.

#### Delete a Credential

1. In the credential row, select `Delete`.
2. Confirm the browser prompt `Are you sure you want to delete this credential?`

The credential is removed right away. You cannot undo this.

#### Change the Vault Password

You can change the vault password only while the vault is unlocked.

1. In `Vault Security Settings`, select `Change Vault Password`.
2. Enter your current vault password in `Current Vault Password`.
3. Enter the new password in `New Vault Password` and again in `Confirm New Vault Password`. The new password must meet the requirements listed in **Set Up the Vault**.
4. Select `Save Password`.

If the current vault password is wrong, the dialog shows `Incorrect current password.`

After the change, the vault stays unlocked and your saved credentials are still available. Your current recovery key keeps working.

#### Regenerate the Recovery Key

Regenerate the recovery key if you lost the old one or think someone else has seen it.

1. In `Vault Security Settings`, select `Regenerate Recovery Key`.
2. Enter your vault password in `Vault Password`.
3. Select `Regenerate`.
4. Save the new key as described in **Save the Recovery Key**. Then select `I have saved this code somewhere safe` and `I have saved it`.

The new key replaces the old one right away. The old recovery key stops working. If the vault password is wrong, the dialog shows `Incorrect password.`

#### Recover Access With the Recovery Key

Use this if you forgot your vault password.

1. Open `Profile > Orion Vault`.
2. In the `Unlock Protected Data` dialog, select `Forgot Security Passphrase?`. The `Data Recovery` dialog opens.
3. Enter your recovery key in `Recovery Key`. Upper and lower case do not matter, and you can include or leave out the hyphens.
4. Enter a new vault password in `New Security Passphrase` and again in `Confirm New Security Passphrase`. The new password must meet the requirements listed in **Set Up the Vault**.
5. Select `Recover Protected Data`.

If recovery succeeds, the new vault password replaces the old one, the vault unlocks, and your saved credentials load. Your recovery key keeps working. If you think the recovery key has been exposed, regenerate it after recovery.

If the recovery key is not accepted, the dialog shows `Invalid Recovery Key`. Select `Cancel` to return to the `Unlock Protected Data` dialog.

```{admonition} Losing both the password and the recovery key
:class: warning

Orion never receives your vault password or your recovery key. It cannot reset them for you. If you lose both, the stored passwords cannot be decrypted.
```

#### Delete the Vault

Deleting the vault removes every credential in it and your data protection key. You cannot undo this.

1. Unlock the vault.
2. In `Vault Security Settings`, select `Delete Vault`.
3. In the `Delete Orion Vault` dialog, enter your vault password in `Vault Password`.
4. Select `Delete Vault`.

Orion returns you to the profile homepage. The next time you open `Profile > Orion Vault`, the `Set Up Data Protection` dialog opens and you can create a new, empty vault.

If the vault password is wrong, the dialog shows `Incorrect Vault Password`. If the server cannot complete the deletion, the dialog shows `Unable to delete Vault. Please try again.`

(password-autofill-on-other-websites)=

#### Password Autofill on Other Websites

The Orion browser extension can use your Orion Vault on other websites. When it is on, the extension places a small Orion icon (labelled `Autofill with Orion`) inside recognized sign-in and sign-up fields. From that icon you can fill a saved login on a sign-in form, or, on a sign-up form, generate a strong password and save a new login (see [Create a New Login on a Sign-Up Form](#create-a-new-login-on-a-sign-up-form)). Website autofill uses the same encrypted vault as the Orion Vault page: passwords are encrypted in your browser, decrypted only while the vault is unlocked, and never filled or saved while it is locked.

Website autofill is **off by default**.

:::{admonition} Prerequisites
:class: note

The Orion icon and all website autofill appear only when every one of these is true:

- The Orion extension is installed, and the `Passwords` toggle in the extension popup is on.
- You are signed in to Orion Intelligence in the same browser.
- Your Orion Vault is unlocked.
:::

**Turning autofill on or off.** Open the extension popup. In the top-right, next to the extension status, is a `Passwords` toggle. Switch it on to enable website autofill, off to disable it. The toggle starts off for every new install.

- While off, the extension adds no icon to any field and performs no fill or save. Nothing about the Vault is shown on web pages.
- While on, the icon appears in recognized username and password fields, and the fill and save flows become available.

Turning the toggle off takes effect immediately on open tabs: any icon already shown is removed and any open panel closes. A page that was already open before you installed or updated the extension keeps the previous behaviour until you reload that page.

**Where the setting is stored.** The on/off choice is saved in the extension's own private storage inside your browser. It is remembered across browser restarts and applies to this browser profile. It is not a website cookie and cannot be read by the websites you visit. Only the on/off value is stored there; no vault data, credentials, or keys are kept in it.

**Filling a saved login.** On a sign-in form, select the Orion icon in the password or username field. The panel lists the logins saved in your Vault for that website, matched by website address. Select one to fill the username and password. If the Vault is locked or you are signed out, the panel shows `Unlock in Orion` or a sign-in prompt instead of the list.

(create-a-new-login-on-a-sign-up-form)=

#### Create a New Login on a Sign-Up Form

On a sign-up (registration) form, the extension can generate a strong password, save it to your Orion Vault as a new login, and fill the form for you. This needs the same prerequisites as autofill above: the `Passwords` toggle on, signed in to Orion in the same browser, and the Vault unlocked.

**How a sign-up form is recognized.** The extension offers this only on the form's main new-password field, and only when the form looks like account creation. It treats a form as sign-up when any of the following is true, and the form is not a sign-in, password-reset, or change-password form:

- A heading or button on the form says sign up, register, or create account.
- The page marks the password field as a new password.
- The form has a second confirm-password field.
- The form asks for registration details such as first name, surname, given or family name, date of birth, or gender.

This covers most registration forms, including those (such as Facebook) that have no clear sign-up heading, because they ask for a name and date of birth. Sign-in, password-reset, and change-password forms are deliberately excluded, so the create flow never appears on them.

**Creating and saving the login.**

1. On the sign-up form, select the Orion icon in the password field. The `Create a login` panel opens and checks the Vault.
2. If you are signed in and the Vault is unlocked, the panel shows a `Username or email for this account` box and a `Generate password & save` button. The box is prefilled from the page's username or email field when one is already filled in.
3. Enter the username or email you want for the new account, if it is not already filled.
4. Select `Generate password & save`. Orion generates a strong password, saves it to your Vault as a new credential for this website, and fills the username field and every password field on the form, including a confirm-password field. The button then shows `Saved to Orion Vault`.

The new credential is stored exactly like one added on the Orion Vault page: the website address and username are saved in the clear, and the password is encrypted in your browser before upload. You can view, edit, or delete it later from `Profile > Orion Vault`.

**Notes.**

- A username or email is required. If the box is empty, the panel asks you to enter one before it will save.
- The generated password is up to 20 characters and respects the form's own length limit, but is never shorter than 8.
- If the form has no detectable username field, only the password fields are filled; enter the username yourself.
- Submitting the form and completing any later steps, such as email or phone verification, is still up to you. The extension only prepares and saves the login.

**When the panel cannot save.** If the Vault is not ready, the `Create a login` panel shows a message and the matching action instead of the form:

| Message | What it means and what to do |
| --- | --- |
| `Orion Vault is locked.` with `Unlock in Orion` | Unlock the Vault in Orion, then open the panel again. |
| `Sign in to Orion to save logins.` with `Sign in to Orion` | The extension is not signed in. Sign in, then retry. |
| `Sign in to Orion Intelligence with the same account, then click the icon again.` | The open Orion tab is signed out, or is a different account than the extension. |
| `That username is already saved for this site.` | A login with that username already exists for this website. Fill it with autofill, or choose a different username. |
| `Enter a username or email first.` / `Enter a valid username to continue.` | Enter a username before saving. |
| `Orion is unavailable. Open Orion Intelligence and try again.` with `Retry` / `Open Orion` | Orion is not open or did not respond. Open Orion and retry. |
| `Could not save the login (…). Please try again.` | The save failed. The code in brackets identifies the cause; retry, and report the code if it persists. |

#### What the Server Can and Cannot See

| Data | How Orion stores it |
| --- | --- |
| Website name, URL, and username | Readable on the server. These fields are not encrypted. |
| Credential password | Encrypted in your browser with AES-256-GCM before upload. The server stores only the encrypted value. |
| Vault password | Never sent to the server. Your browser derives keys from it. The server keeps only a one-way hash of a check value, which it uses to confirm that you entered the right password. |
| Recovery key | Never sent to the server. It is handled the same way as the vault password. |
| Vault encryption key | Stored only in encrypted form. It can be unlocked only with your vault password or your recovery key. |

In practice, this means:

- No one with access to the Orion server or database can read your stored passwords, including administrators.
- Website names, URLs, and usernames are not protected the same way. Do not put secrets in those fields.
- Your browser decrypts passwords only while the vault is unlocked. The vault key is kept in browser memory and is never written to browser storage.
- Orion asks for your login password before it shows or replaces a stored password. This adds a check if someone else uses your session while the vault is unlocked.

#### Limits and Troubleshooting

Incorrect vault passwords and recovery keys are limited per account. After five incorrect attempts, Orion blocks unlocking for 15 minutes. Attempts count across unlocking, recovery, changing the vault password, regenerating the recovery key, and deleting the vault.

Incorrect login passwords in the `Confirm your identity` dialog count toward the same limit as sign-in attempts. After five incorrect entries, Orion blocks further attempts for a short time.

| Message | Where it appears | What it means and what to do |
| --- | --- | --- |
| `Passwords do not match.` | Setup, change password, recovery | The two password fields differ. Re-enter the confirmation. |
| `That passphrase is not correct` | `Unlock Protected Data` | The vault password is wrong. Check upper and lower case, or use `Forgot Security Passphrase?`. |
| `Too many incorrect attempts. Key unlock blocked for N more minute(s).` | `Unlock Protected Data` | Five incorrect attempts in a row. Wait for the time shown, then try again. |
| `Invalid Recovery Key` | `Data Recovery` | The recovery key was not accepted. Check the key and try again. |
| `Incorrect current password.` | `Change Vault Password` | The current vault password was not accepted. |
| `Incorrect password.` | `Regenerate Recovery Key` | The vault password was not accepted. |
| `Incorrect Vault Password` | `Delete Orion Vault` | The vault password was not accepted. The vault was not deleted. |
| `Invalid password` | `Confirm your identity` | The application login password is wrong. Enter the password you use to sign in to Orion, not the vault password. |
| `Too many login attempts. Try again in N seconds.` | `Confirm your identity` | Too many incorrect login-password entries. Wait for the time shown. |
| `Website Name is required`, `Website URL is required`, `Username / Email is required`, `Password is required` | `Add Credential`, `Edit Credential` | A required field is empty. |
| `Please fill all required fields.` | `Add Credential`, `Edit Credential` | One or more required fields are empty, or contain only spaces. |
| `Please enter a valid URL` | `Add Credential` | The website address is not in a supported format. |
| `Verification proof has expired` | `Edit Credential` | More than five minutes passed after you confirmed your login password. Close the panel, then select `Change Password` again. |
| `Vault is locked. Cannot encrypt password.` | `Add Credential`, `Edit Credential` | The vault key is no longer in the browser. Close the panel, unlock the vault, and try again. |
| `Unable to copy. Please copy it manually.` | Recovery key dialog | The browser blocked clipboard access. Copy the key by hand. |

### Public User Activity

User activity pages open from profile, report, comment, or interaction links when profile visibility allows it.

The page can show:

- the user's profile image
- visible activity items
- report or thread links for activity entries
- an unavailable or private-profile state when visibility is disabled

Tenant profile visibility and the user's own profile preference can hide public activity from other users.

### AI Workspace

The AI Workspace, including Nexus chat and the Kill Chain view, is documented in [DeepIntel](deepintel.md#ai-workspace).

### Tenant Homepage

For tenant users, the profile homepage may function as a tenant intelligence and alert workspace instead of a simple profile landing page.

Depending on license and role, this page can include:

- homepage search
- alert export
- scan-all or flush-all actions
- risk summary cards
- category alert cards
- monitored IOC counts
- alert scanner settings

In some deployments, this page behaves differently by role:

- maintainers or higher-license users may receive the full alert-and-action workspace
- analysts may see a simpler search-first homepage variant
- some users may see an insights-only fallback instead of tenant alert controls

The summary area commonly displays:

- critical alerts
- high-risk alerts
- medium-risk alerts
- low-risk alerts

Category cards provide quick access to alert-specific drill-down reports.

Maintainer-level users can manage alert scanner settings when the workflow is enabled. The scanner settings page lets the tenant enable or disable allowed alert categories for future tenant scans. Saving updates the tenant's allowed alert categories.

Scheduled alert scans use the tenant's monitored IOC values, allowed alert categories, and alert run-time configuration. The run time controls when the platform should perform the tenant's recurring alert scan, while scan-all remains the manual path for immediate follow-up.

The profile area also supports alert-focused workflows such as:

- category-specific alert reports
- custom alert creation where enabled
- category-level alert scanner settings where enabled

#### Custom Alerts

Custom Alerts allow a permitted user to create or edit an alert record manually. The form captures alert type, status, title, description, source, reference URL, and one IOC bucket.

Supported alert types are:

- `general`
- `breach`
- `exploit`
- `social`
- `defacement`

The alert form validates title, description, source, and URL before saving. The reference URL must start with `http://` or `https://`. The IOC selector uses the deployment's supported entity list, and the saved alert receives license visibility based on the alert type.

#### Alert Scanner Settings

Alert scanner settings control which alert categories are allowed to run for the tenant. They are useful when a tenant wants monitoring but does not want every possible scanner category to execute.

Typical scanner categories can include:

- general intelligence
- breach and credential exposure
- compromise monitoring
- social and discussion sources
- exploit intelligence
- stealer logs
- scanning-backed categories such as email breach, social scanner, software scanner, repository scan, SEO scan, playstore scan, advanced scan, and vulnerability scan

The exact list depends on the deployment and tenant configuration. Disabled categories are not a data deletion control; they affect future alert scanning and visibility behavior.

#### Alert Report Drilldown

Category alert reports are opened from `Profile > Homepage` or from alert drilldowns. The report page is used to review category-specific alert records, inspect risk context, export findings, and move from an alert into a deeper investigation.

Alert detail drawers can show the alert risk, title, description, URL, category, source, matched entity, result date, content type, password when present, and raw findings when the source record includes additional evidence. Use the drawer to verify the underlying finding before exporting, creating a custom alert, or opening a case workflow.

Admins with case-management visibility can also open tenant alert views inside case management. That workflow is used when tenant alerts need administrative review alongside cases and tracked investigation work.

### Take Down

The `Take Down` page is the review workspace for abuse and takedown evidence requests. It appears in the profile area as `Takedown Requests` for takedown reviewers and opens at `/dashboard/profile/take-down`. The page heading reads `Take Down` with the description "Review takedown evidence requests and approve or deny abuse reports."

Orion supports two kinds of takedown:

- **Domain / Web takedowns.** Orion captures evidence for a target website, finds its public abuse contact, and creates a request that a reviewer accepts or rejects. Accepting sends the abuse email through Orion Mail. Replies from the abuse contact appear on the `Takedown Mail` page.
- **Social takedowns.** Orion reports an impersonating social media profile. For supported platforms it submits the report automatically. For all other platforms it shows the official reporting portal and any known abuse email.

Both kinds start from the same takedown popup, which has two tabs: `Domain / Web` and `Social Takedown`.

#### Access and Roles

| Action | Who can do it |
| --- | --- |
| Open the takedown popup from a Compromise Monitoring report | Any user who can open the report. Creating a domain request requires the admin, member, or analyst role and Compromise Monitoring (defacement) module access. Users with the maintainer license do not need the module. |
| Submit an automated social takedown | Users with the admin, member, or analyst role and Social Mapper module access. Admins and users with the maintainer license do not need the module. |
| Open the `Take Down` and `Takedown Mail` pages | Administrators of the root (default) tenant, and users with the maintainer license in a primary tenant. Other users who open these routes are sent back to the profile homepage. |
| Accept, reject, or retry a request; read and reply to takedown mail | The reviewer of the tenant that owns the request (see below). |

Each request is assigned to an owning tenant when it is created:

- A request from a primary tenant is owned by that primary tenant.
- A request from a sub-tenant is owned by its parent primary tenant.
- All other requests are owned by the root tenant.

Primary-tenant maintainers see and decide the requests owned by their primary tenant, which includes requests from its sub-tenants. Root administrators see requests from every tenant. They can only accept, reject, or retry requests owned by the root tenant. For requests owned by a primary tenant they see the row and the `View` action only.

```{admonition} Access and scope
:class: note

Reviewers act only on requests owned by their own tenant. A root administrator can see a request raised by a sub-tenant but cannot approve it; the sub-tenant's primary maintainer approves it. See [Primary Tenants and Sub-Tenants](#primary-tenants-and-sub-tenants).
```

#### Starting a Domain or Web Takedown

Domain takedowns start from one of two places:

- `Initiate Takedown` on a Compromise Monitoring report. The report's target URL is used.
- `Report` on the `Take Down` page. You enter a target URL yourself.

To start a takedown from a report:

1. Open the Compromise Monitoring report and click `Initiate Takedown`.
2. The popup opens on the `Domain / Web` tab with the title `Takedown Request` and the note "Verify abuse contact evidence before creating an admin review entry."
3. Optionally enter a `Custom Email Note (Optional)`. This note will be included in the abuse report email sent to the provider.
4. Click `Report takedown`.
5. Orion shows "Capturing evidence and creating the admin review request..." while it captures a screenshot and page source and looks up the abuse contact. The button reads `Creating request` during this time.
6. When capture finishes, the popup shows the request status, for example `Takedown in progress`, the abuse email found, and "Evidence has been saved for admin review."
7. Click `Close`.

If the report URL belongs to a supported social platform, the popup opens on the `Social Takedown` tab instead, with the platform selected and the URL prefilled. See [Social Takedown](#social-takedown).

To report a URL manually from the `Take Down` page:

1. Click `Report`.
2. Enter the `Target URL`, for example `https://example.com/path`. A URL without a scheme is treated as `https://`.
3. Optionally enter a `Custom Email Note (Optional)`.
4. Click `Report takedown`, or press Enter in the URL field. The button is disabled until a URL is entered.

The new request is added to the list after it is created.

You can close the popup while evidence is being captured. Capture continues and the button keeps showing `Creating request`. If capture has not finished after 10 minutes, the request is marked as failed with the reason "Evidence capture timed out".

When the abuse contact cannot be found, or evidence capture fails, the popup shows `Takedown request not created` and "No evidence entry was saved", followed by the reason, for example "No public abuse contact was found for this site." The request is still recorded with the `failed` status. It appears on the `Take Down` page, where a reviewer can retry or reject it.

Some providers use a web form for abuse reports. When the captured evidence says so, the popup shows "This platform uses a dedicated portal for abuse reporting." and an `Open Official Reporting Form` link.

Only one domain request can exist for each domain within a tenant. Once a request exists, the report button shows the current status label and is disabled. Trying to create a second request for the same domain returns "A takedown request already exists for this domain".

The takedown button changes to show the request state:

| Button label | Tooltip |
| --- | --- |
| `Initiate Takedown` | `Open Takedown Evidence Panel` |
| `Creating request` | `Creating takedown request` |
| `Capturing evidence` | `Takedown is unavailable` |
| `Takedown in progress` | `Takedown request is under review` |
| `Takedown reported` | `Takedown has been reported` |
| `Takedown denied` | `Takedown request was denied` |
| `Takedown failed` | `Takedown is unavailable` |

(social-takedown)=

#### Social Takedown

The `Social Takedown` tab reports social media accounts that impersonate a person or brand. Its title is `Profile Takedown` with the note "Report impersonating social accounts directly or via official portals."

Choose the platform from the `Platform` dropdown. The dropdown shows the selected platform with an `Automated` or `Manual` tag. Inside the dropdown:

- Use `Search 260+ platforms...` to search by platform name, domain, or category.
- Use `All`, `Auto`, or `Manual` to filter the list.
- Results are grouped under `Automated` and `Manual`. "No platforms found" appears when nothing matches.

Facebook is selected by default.

##### Automated Platforms

Orion submits the report automatically for nine platforms: Facebook, Instagram, Threads, TikTok, YouTube, Reddit, Quora, Pinterest, and Patreon. It uses browser automation to fill in each platform's official report form.

`Impersonation Profile URL` is required for every automated platform. Orion checks that the URL matches the selected platform:

- The host must belong to the platform, for example `facebook.com`. Threads accepts `threads.net` and `threads.com`, YouTube accepts `youtube.com` and `youtu.be`, and Pinterest accepts `pinterest.` domains and `pin.it`.
- The URL must include a profile path, or an `id` query parameter.
- Reddit URLs must use `/user/` or `/u/`. Quora URLs must use `/profile/`. TikTok URLs must use `/@username`.

An invalid URL shows "Please enter a valid profile URL." and `Launch Automated Takedown` stays disabled.

The other fields depend on the platform:

| Field | Platforms | Required |
| --- | --- | --- |
| `Impostor Handle or Name` | All except Reddit, YouTube, and Quora | Yes |
| `Email or Phone on impostor profile (Optional)` | Facebook | No |
| `Active Session` | Reddit, YouTube | Yes |
| `Reporter Legal Name` | All except Reddit, YouTube, and TikTok | Yes |
| `Contact Email` | All except Reddit, YouTube, and TikTok | Yes |
| `Government Photo ID` | Facebook, Instagram, Threads | Yes |
| `Proof Attachment (Optional)` | Pinterest | No |
| `Relationship to Patreon` (`Neither`, `Patron`, `Creator`) | Patreon | Yes |
| `Target` (`Me or my child`, `A celebrity`, `A business`) | Pinterest | Yes |
| `Pin or Board URL` | Pinterest | No |
| `Subject Line` | Patreon, Pinterest | Yes |
| `Violation Details` | All | Yes, except optional for Reddit, YouTube, and Quora |

Notes on these fields:

- `Reporter Legal Name` is prefilled with the tenant name, or your username if there is no tenant name.
- `Contact Email` is prefilled with the tenant's Orion Mail report mailbox. If there is none, it uses your account email or the tenant contact email instead. An `Orion Mail` tag appears next to the field when the address is an Orion Mail address, so platform replies reach Orion Mail.
- `Government Photo ID` and `Proof Attachment (Optional)` accept image files. Click `Attach Photo ID` to choose a file. The button then shows the file name. Click `Remove` to clear it.
- `Active Session` lists the sessions saved for Reddit or YouTube, shown as `@username` or `Session #` followed by the first characters of the session ID. These platforms submit from a signed-in account, so capture a session first. See [Capture and Maintain Browser Sessions](#capture-and-maintain-browser-sessions).
- For Reddit, YouTube, TikTok, and Quora, Orion fills in missing details before submitting. It takes the impostor name from the profile URL, the reporter name and email from your account, and a default description of the report.

To submit an automated social takedown:

1. Open the takedown popup and select the `Social Takedown` tab.
2. Choose an automated platform.
3. Fill in the required fields for that platform.
4. Click `Launch Automated Takedown`.
5. The button reads `Automating...`. A progress panel titled `Automating takedown runner...` shows a percentage and the current step: initializing browser automation, navigating to the platform support portal, filling the form, submitting the report, and waiting for confirmation. The steps advance on a timer while the request runs; they do not report live progress from the platform.
6. When the platform confirms, a `Takedown Submitted Successfully` card shows the result message. If the platform returned a ticket number, it is shown as `Ticket: #` followed by the number. A success notification also appears, the form is cleared, and the footer button changes to `Close`.
7. If the submission fails, a `Submission Failed` card shows the reason and an error notification appears. Correct the details and click `Launch Automated Takedown` again.

Use the close icon on a result card to dismiss it. Changing the platform also clears the result.

Social takedowns are submitted directly to the platform. They do not create an entry on the `Take Down` page and do not need reviewer approval.

##### Manual Platforms

All other platforms are manual. Orion has a reporting dossier for 251 of them, grouped into categories such as Social & Microblogging, Messaging & Community, Video & Streaming, and Developer & Tech. When you select a manual platform, Orion shows a card with:

- the platform name and an `Open Report Form` link to the platform's official reporting portal
- notes on how that platform handles impersonation reports, when available
- an `Abuse Email:` address with a `Copy Email` button (`Copied!` confirms the copy), when the platform publishes one

`Launch Automated Takedown` is not shown for manual platforms. You submit the report yourself on the platform's portal.

#### Reviewing Takedown Requests

The `Take Down` page lists domain takedown requests, newest first, with up to 100 rows per page. The table columns are `ID` (row number), `Created`, `Target` (domain and full URL), `User` (username and user ID), `Status`, and `Action`.

The page supports:

- searching by target, abuse email, user, or report identifier with `Search target, email, user, report` (press Enter to search)
- the `Filter` panel, with a date range and a `Status` filter for `All`, `Pending`, `Accepted`, `Denied`, or `Failed`
- pagination
- `View`, to open the request report
- `Accept`, `Reject`, and `Retry`, depending on status
- `Report`, to create a request manually from a target URL
- the `Takedown Mail` button, to open takedown mail conversations

Requests that are still capturing evidence are not listed. When no requests match, the page shows "No takedown requests found for the selected filters." If the list cannot load, it shows "Unable to load takedown requests."

Click `View` to open the request report. It shows:

- the target and current status
- `Abuse contact`
- `Channel` (`Email` or `Web form`)
- `Requested by`
- `Created`
- `Reported`, once the abuse email has been sent
- `Target URL`

It also shows any `Failure reason`, `Rejection reason`, or `Analyst note` (the custom email note). Under `Captured evidence` you see the captured screenshot, which you can `Expand` or `Collapse`. If no screenshot was captured, the report shows the capture error or "No screenshot was captured for this site."

#### Request Statuses

Request statuses have the same meaning on reports and on the review page:

| Stored status | Label | Meaning |
| --- | --- | --- |
| `capturing` | `Capturing evidence` | Orion is capturing evidence and looking up the abuse contact. The request is not yet listed for review. |
| `pending` | `Takedown in progress` | Evidence and an abuse contact were captured, and the request is waiting for a reviewer. |
| `accepted` | `Takedown reported` | A reviewer accepted the request and the abuse email was sent. |
| `denied` | `Takedown denied` | A reviewer rejected the request with a reason. |
| `failed` | `Takedown failed` | Evidence capture failed, no abuse contact was found, or capture timed out. The reason is shown under the status badge. |

#### Accepting, Rejecting, and Retrying

The actions available on each row depend on its status:

| Status | Actions |
| --- | --- |
| `pending` | `Accept`, `Reject`, `View` |
| `failed` | `Retry`, `Reject`, `View` |
| `accepted` | `View` |
| `denied` | `View` |

Only one row action can run at a time.

**Accept.** Accepting a pending request emails the captured abuse contact through Orion Mail, from the owning tenant's mail account. The subject is `Urgent: Takedown Request for <domain> [TD-<request id>]`. The message asks the provider to suspend the domain and refers to the attached screenshot and HTML evidence. Any custom email note is included as an "Additional Analyst Note". The request then moves to `Takedown reported` and the source report shows the same label. Accepting requires an activated tenant mail account; see [Tenant Mail Account Activation](#tenant-mail-account-activation).

**Reject.** Rejecting opens `Reject takedown request`, which shows the target and asks for a `Rejection reason`. The prompt reads "Provide the reason admins will see in the request history." The reason is required and can be up to 500 characters. A reason made only of spaces shows "Reason cannot be blank." Click `Reject request` to confirm; the button reads `Rejecting...` while it saves. The request moves to `Takedown denied` and the reason appears in the request report. Accepted requests cannot be rejected.

**Retry.** Retrying a failed request clears the old evidence and captures fresh evidence, keeping the original custom note. The button reads `Retrying` while capture runs. When capture succeeds the request returns to `Takedown in progress`; otherwise it stays `Takedown failed` with the new reason.

(tenant-mail-account-activation)=

#### Tenant Mail Account Activation

Takedown emails are sent from the reviewing tenant's Orion Mail report mailbox. Before accepting requests, the tenant mail account must be activated and its passwords and keys configured.

When the mail account is not activated:

- The `Takedown Mail` button on the `Take Down` page shows `Mailbox Needs Activation`.
- `Accept` is disabled, and its tooltip reads "Tenant mail account must be activated first".
- If an accept is attempted, the page shows "Please activate your tenant mail account and configure passwords/keys before accepting takedown requests."

If Orion cannot check the mail account status, it treats the account as not activated. While the account is not activated, Orion checks the status again whenever you return to the browser tab, so the warning clears after activation without a page reload.

```{admonition} Activate before reviewing
:class: note

Rejecting and retrying requests still work while the mail account is not activated. Only accepting, which sends the abuse email, is blocked.
```

#### Takedown Mail Conversations

The `Takedown Mail` page holds replies from abuse contacts to the takedown emails your tenant has sent. Open it with the `Takedown Mail` button on the `Take Down` page, or at `/dashboard/profile/take-down-mail`. The button shows the number of unread replies, up to `99+`.

The page heading is `Takedown Mail`, with the description "Replies from abuse contacts for your takedown requests." Use the back arrow to return to the `Take Down` page.

The inbox lists one row per conversation, newest first. Each row shows:

- the other party's address, or `Unknown sender`
- the subject
- a preview of the latest message, starting with `You: ` when you sent it
- the date

Conversations with unread replies are highlighted and marked with a dot. Opening the `Takedown Mail` page marks all takedown mail as read and resets the unread count. When there is no mail yet, the page shows "No takedown mail yet" and "Replies from abuse contacts will appear here."

To read and reply to a conversation:

1. Click a conversation row. The latest message opens with its subject, a `Sent` or `Inbox` label, the sender, the date, the recipient, and the message body.
2. If the conversation has more than one message, the `Conversation` section lists the others with the message count. Click a message to read it.
3. Click `Reply`. A reply box opens, addressed to the other party.
4. Type your reply. To add files, click `Attach`; you can attach several files and remove each one before sending.
5. Click `Send`, or press Ctrl+Enter (Cmd+Enter on macOS). The button reads `Sending...` while the message goes out. The subject is prefixed with `Re: ` if it is not already a reply.
6. After sending, the conversation reloads with your reply included.
7. Click `Inbox` to return to the conversation list.

If a reply cannot be sent, the reply box shows the error, for example "Unable to send reply."

#### Messages and Errors

| Message | Where it appears | What it means |
| --- | --- | --- |
| `Takedown request not created` / "No evidence entry was saved" | Domain tab | Evidence capture failed or no abuse contact was found. The reason follows. |
| "No public abuse contact was found for this site." | Domain tab, request failure reason | The target site does not publish an abuse email. |
| "Evidence capture timed out" | Request failure reason | Evidence capture took longer than 10 minutes. |
| "A takedown request already exists for this domain" | Domain tab | Your tenant already has a request for this domain. |
| "Target URL is required." | Domain tab, manual mode | No target URL was entered. |
| "Please enter a valid profile URL." | Social tab | The profile URL does not match the selected platform. |
| "The selected session could not be found." | Social tab | The chosen Reddit or YouTube session no longer exists. |
| "The selected session is unreadable. Please recapture the session." | Social tab | The saved session cannot be used; capture it again. |
| "Reddit takedown requires an authenticated session." / "YouTube takedown requires an authenticated session." | Social tab | No session was selected for Reddit or YouTube. |
| `Submission Failed` | Social tab | The platform did not confirm the report. The reason follows. |
| "Tenant mail account must be activated first" | `Accept` tooltip | The tenant mail account is not activated. |
| "Tenant mail account is not activated. Please activate your tenant mail account and configure passwords/keys before accepting takedown requests." | `Take Down` page | An accept was attempted before mail activation. |
| "No abuse email found in captured evidence" | `Take Down` page | The request has no abuse contact to email. |
| "Denied takedown requests cannot be accepted" | `Take Down` page | The request was already rejected. |
| "Accepted takedown requests cannot be denied" | `Take Down` page | The request was already accepted. |
| "Root admin access is required" | `Take Down` and `Takedown Mail` pages | Your account is not a takedown reviewer for this tenant. |
| "Unable to accept takedown request." / "Unable to reject takedown request." | `Take Down` page | The action failed for another reason. |

### Manage IOCs

The IOC management page allows tenants to maintain the set of monitored values used in searches and alerting.

Capabilities include:

- IOC category search
- horizontal category browsing
- adding IOC values
- IOC import from CSV
- CSV upload action for bulk IOC upload
- downloading the CSV template
- removing IOC values
- clearing all IOC values

This page is especially important for tenant-driven monitoring workflows.

CSV imports must use an exact `key,value` header. Files must be CSV format, no larger than 1 MB, and each key must match a supported IOC entity key. Duplicate values are ignored during import.

Some tenants can be marked for Privileged IOC handling. When that protection applies, users without the required permission can review IOC values but cannot add, remove, clear, or upload IOC values for that tenant. The page shows a permission warning when IOC editing is disabled.

Example CSV structure:

```text
key,value
domain,example.com
email,analyst@example.com
ip,203.0.113.10
url,https://example.com/login
```

After import, review the visible IOC counts before running tenant alert scans. Invalid keys or oversized files should be corrected before retrying the import.

The tested tenant IOC workflow includes:

- opening the IOC page from the tenant profile area
- switching across IOC category tabs
- adding values in multiple categories
- importing values from a CSV template
- adding monitored email values for downstream alerting
- returning to the tenant homepage and triggering follow-up scanning actions

### Statistics

The `Statistics` page in the profile area reuses the insight-oriented summary experience for users who want a visual overview without returning to the main homepage.

### Profile Consolidated View

The profile area also contains a consolidated-search view. Functionally, it behaves like the main consolidated workspace but sits within profile and tenant-oriented workflows.

### Monitoring

Monitoring is an operational profile workspace that groups monitoring-related tabs. Depending on role and tenant configuration, the visible tabs can include:

- `Log Manager`
- `Auditlog`
- `Event Management`

Admins can see Log Manager. Admins and maintainers can see Auditlog. Event Management appears only for admins or maintainers when the tenant has event management enabled.

### Event Management

Event Management is the SIEM-style event search workspace. It is available from the profile area or the Monitoring tab when enabled.

Capabilities include:

- searching SIEM events
- using IOC-style search tags such as all, domain, email, and IP
- validating domain, email, and IP input
- filtering by date range
- paginating large event result sets
- expanding a result to inspect raw event fields and extracted IOCs

### Log Manager

Log Manager is an admin-only operational log viewer.

Capabilities include:

- filtering logs by type: `INFO`, `WARNING`, or `ERROR`
- filtering by available log date
- paginating log entries
- deleting an individual log file
- flushing all logs after confirmation
- reviewing file size and log metadata

Use Log Manager for operational troubleshooting, not analyst investigation.

### Feeder

Feeder is the source/rule intake workspace for users with feeder access.

The Feeder workspace includes:

- a rule catalog dropdown
- grouped Social Media rules for supported social platforms
- an `Add` tab for uploading parser files or saving URL values
- a `View` tab for uploaded scripts
- a `Values` tab for stored rule values where supported
- search, sorting, pagination, preview, enable/disable, delete, and clear controls
- owner transfer for admins

Upload rules depend on the selected rule type:

- Python parser uploads must use `.py` files no larger than 1 MB.
- Shared session uploads must use `.zip` files.
- Value-backed rules accept newline-separated URL values.
- Shared rules require the parser file before adding values.

Feeder data is used by collection workflows. It should be managed carefully because enabling, disabling, clearing, or deleting entries affects future ingestion behavior.

### Case Management

Case Management is the investigation workspace for turning alerts, findings, and analyst leads into tracked cases. It is available from the profile area when the user has the required case-management access.

Common entry points include:

- `Profile > Case Management`
- `Profile > Case Management > Tracking Board`
- `Profile > Case Management > Case Details`
- shared case links
- admin tenant alert views inside case management

Case visibility and allowed actions depend on role:

| Role or access pattern | Typical capabilities |
| --- | --- |
| Admin | create, assign, update, archive, unarchive, close, share, export, and review tenant alert case flows |
| Maintainer/member with access | create, assign where permitted, update, archive, share, export, and close eligible cases |
| Analyst with case-management permission | view assigned or permitted work, update allowed case fields and task state, review evidence, and contribute comments |
| Shared-link viewer | view only the shared case material allowed by the generated share link |

#### Case List, Filters, And Analytics

The Case Management landing page has three working modes where permissions allow them:

- `Case List` for day-to-day triage and opening cases
- `Analytics` for summary charts and workload review
- `Alerts` for admin tenant-alert review inside the case-management context

The case filter row applies to the list and analytics modes. It supports:

- case-list scope selection for open or archived cases where the user can manage cases
- free-text search across case ID, title, and description
- status filtering
- severity filtering
- priority filtering
- searchable case-type filtering
- sort order selection

The same filter state is used by the list and analytics panel. For example, if the user searches for a case ID and switches to `Analytics`, the analytics counts and charts reflect the filtered set rather than silently reverting to all cases.

The analytics panel shows:

- visible cases compared with total cases
- active case count
- critical-severity case count
- high-priority case count
- unassigned case count
- open-task count
- artifact count
- average case age and stale case count
- status distribution
- severity distribution
- priority distribution
- case-type distribution
- intake-source distribution
- task-status distribution
- analyst workload
- `Needs Attention` cases

Use analytics when a lead or administrator needs to understand workload, stale investigations, unassigned work, or high-risk cases before assigning analysts or moving cases through the tracking board.

```{figure} ../../screenshots/case-management-add-20260326.png
:alt: Add case drawer
:width: 100%

Case creation drawer with the core case fields and primary entity form.
```

When adding a case, users define:

- case title and investigation description
- case type and intake source
- status, severity, and priority
- tags for triage and reporting
- primary entity, such as a person, organization, email, domain, IP, URL, account, credential, or infrastructure indicator

Recommended case creation flow:

1. Create the case with a clear title, severity, priority, and intake source.
2. Add a primary entity before adding secondary evidence.
3. Attach artifacts or report references while the source context is still fresh.
4. Assign analysts or tasks if follow-up work is needed.
5. Use the tracking board for status movement rather than editing status informally.

The case details page keeps the case record organized into independent sections. Each section has its own add or edit action, and side drawers are used for focused data entry.

```{figure} ../../screenshots/case-management-view-20260326.png
:alt: Case detail view
:width: 100%

Case detail view with closure, case metadata, entity context, evidence, and analyst workflow sections.
```

The main case details section shows the title, description, case ID, type, intake source, status, severity, priority, tags, assigned analysts, PDF export, and share-link actions.

PDF export is intended for handoff and reporting. Share links are intended for controlled review of a case without giving the recipient broader application access. Revoke case shares when external review is complete.

Primary Entity stores the main subject of the investigation. Related Entities are additional people, domains, accounts, assets, indicators, sources, or actors connected to the case.

The case details page also exposes a case-level Nexus assistant where enabled. This assistant receives the current case details as its working context, so it is useful for summarizing case state, asking what evidence is already attached, preparing handoff notes, or identifying likely next steps from the visible case record. It is separate from public case shares; shared-link viewers do not receive broader application access through the assistant.

#### Case Artifacts

Artifacts store evidence and supporting material. Common artifact types include screenshots, uploaded files, URL captures, raw alerts, log excerpts, email headers, chat transcripts, linked reports, and generic evidence.

Artifact records include:

- title
- type
- source
- captured date
- description
- optional URL for URL-capture artifacts
- optional files for screenshot and file artifacts
- optional linked report metadata for report artifacts

Artifact cards show the title, type, source, captured date, description, URL actions, linked-report actions, and file actions where relevant.

##### Linked Report Artifacts

When the artifact type is `Report`, the user can attach an existing Orion report instead of only typing a free-form artifact description.

The linked-report workflow is:

1. Add or edit an artifact.
2. Set artifact type to `Report`.
3. Choose a report source.
4. Search for a report title.
5. Select a result from the report dropdown.
6. Save the artifact.
7. Use `View Report` from the artifact card when the linked report needs to be reopened.

Supported report sources are:

- `General Intelligence`
- `Data Breach`
- `Compromise Monitoring`
- `Social`
- `Feed`
- `Exploit`
- `Stealer Logs`

If no result matches the search term, the dropdown shows the empty state. Clearing the selected report removes the linked report ID and title from the artifact before saving.

##### Artifact Files And Integrity

Screenshot and file artifacts support multiple uploaded files. Screenshot artifacts accept PNG files. File artifacts accept PDF, JPG, PNG, TXT, or DOCX files. The current UI allows up to five files per artifact.

Saved artifact files show the file name and an integrity badge. Users with case-management authority can:

- download a file
- verify a file's integrity
- delete a file from an open case

Integrity verification compares the stored file against the case artifact record and updates the badge to `Verified` or `Integrity Failed`. Failed integrity disables download for that file so users do not rely on evidence that no longer matches its stored integrity record.

Analyst-style users without the required management permission can review artifact files, but they cannot run artifact integrity verification.

Tasks track follow-up work for analysts. A task can hold status, priority, assignee, due date, description, and links to relevant entities or artifacts.

Linked Cases connect the current case to other case records. Links can mark duplicates, parent or child cases, follow-ups, escalations, shared actors, shared victims, shared infrastructure, or general related cases.

Comments provide the analyst discussion thread for the case. Use comments for review notes, handoff context, evidence interpretation, or follow-up decisions. Comment authors can be opened through the user sidebar where supported.

Closure records the final outcome. It includes the closure reason, summary, resolution notes, who closed the case, and the close time. Closing a case is the point where the investigation outcome becomes part of the case report and exported PDF.

#### Case Status Lifecycle

Case status changes follow a fixed tracking-board flow:

1. `new`
2. `intake_review`
3. `under_investigation`
4. `evidence_collection`
5. `verification`
6. `regulatory_action`
7. `legal_review`
8. `resolved`
9. `closed`

Status changes must be made from the tracking board, require a reason, and can move only one step forward or backward. A case cannot be moved back to `new`, and a closed case cannot be moved to another status.

Each board move stores the submitted reason in the case status history. Use concise reasons that explain why the case moved, because this history supports handoff, review, export preparation, and later audit of the investigation timeline.

Closure is handled separately from board movement. A case can be closed only from the case details closure section, only after it reaches `resolved`, and only by admins, maintainers, or the case creator.

Archived cases are read-only for update-style actions. Analyst access is intentionally narrower than admin or maintainer access: analysts can view assigned work and update their allowed task state, but case assignment, case status movement, and closure remain restricted.

#### Admin Tenant Alerts In Case Management

Administrators can review tenant alert categories from the case-management area when the workflow is enabled. This view is useful when an alert needs to become a case, be compared with existing cases, or be reviewed across tenants.

Use this path when:

- a tenant alert requires administrative triage
- a category alert needs to be linked to active case work
- a default or administrative tenant needs to inspect alerts for another tenant
- alert review and investigation status need to be handled together

### Tenant Settings

Tenant Settings holds your tenant's identity, contact details, custom domain, webmail domain, outgoing mail server, alert integrations, alert schedule, and privacy options. The page title is `Tenant Data`. Sections appear in this order:

1. `Profile`
2. `Contacts, Users & Address`
3. `Custom Domain`
4. `Webmail Configuration`
5. `Network Configuration`
6. `Alert Integrations`
7. `Alert Schedule`
8. `Privacy & Access`

Each section has its own `Save changes` button. A button stays disabled until you change something in that section, and saving one section does not save the others.

```{admonition} Who sees which sections
:class: note

The header, `Profile`, and `Alert Integrations` are shown to every user who can open the page. All other sections are shown only to tenant users (not platform administrators) whose tenant does not hold the `free` license. `Custom Domain` is shown only on a primary tenant. Saving tenant details requires the maintainer license, and the root/default tenant cannot be changed from this page.
```

Alert scanner categories are not set on this page. Use `Alert scanner settings` on the Tenant Homepage instead.

#### Tenant Data header and Profile

The header shows the tenant image and a `Licenses assigned` line listing the tenant's licenses, or `No licenses assigned`.

To change the tenant image:

1. Click the round image. `Change` appears when you hover over it.
2. Choose an image file. It uploads straight away; you do not need to save.
3. To go back to the default image, click the `✕` button on the image.

The `Profile` section summarizes the tenant in four read-only tiles:

| Tile | Shows |
| --- | --- |
| `Tenant Name` | The tenant name, or `Unnamed Tenant` |
| `Location` | City/state and country, or `Location will appear here` |
| `Assigned Quota` | The number of users assigned to the tenant |
| `Licenses Count` | The number of licenses the tenant holds |

```{figure} ../../screenshots/tenant-settings-20260326.png
:alt: Tenant settings page
:width: 100%

Tenant settings and tenant-level license summary.
```

#### Contacts, Users & Address

This section has the tenant's contact details.

| Field | Editable | Notes |
| --- | --- | --- |
| `Email Address` | No | The email address of the signed-in account |
| `Phone` | Yes | Digits only, with an optional leading `+` |
| `Assigned User Quota` | No | Set by an administrator or the primary tenant |
| `Country` | Yes | Letters, spaces, and hyphens only |
| `City/State` | Yes | Letters, spaces, and hyphens only |

Characters that are not allowed are removed as you type. Click `Save changes` to store the phone, country, and city/state.

#### Custom Domain

A custom domain lets your users open the workspace at your own hostname, for example `app.acme.com`, instead of the tenant's standard Orion address. Only a primary tenant can set a custom domain. Sub-tenants do not see this section, and the backend rejects a custom domain for any tenant that is not primary.

The badge next to the section title shows the current state:

| Badge | Meaning |
| --- | --- |
| `Not configured` | No domain has been entered |
| `Unverified` | A domain is entered but has not passed verification, or you have edited it and not saved yet |
| `Verified` | The saved domain passed verification |

To set up a custom domain:

1. Enter the hostname in `Domain Hostname`, for example `app.acme.com`. The value is stored in lowercase, and any `https://`, port, or trailing dot is removed.
2. Read the records under `Domain DNS Setup` and add them at your DNS provider. Keep them DNS only, not proxied.
3. Click `Save changes`. Saving clears any earlier verification and turns off custom webmail, then runs verification straight away.
4. If verification fails, read the message under `Verification failed`, fix the DNS records, wait for them to propagate, and click `Verify Domain`.

The records shown depend on the hostname you enter:

| Hostname entered | Record label | Type | Name / Host | Target / Value |
| --- | --- | --- | --- | --- |
| Subdomain, such as `app.acme.com` | `Primary Domain (CNAME) · required` | CNAME | `app.acme.com` | The platform's custom-domain target (`domains.orionintelligence.org` unless your deployment sets another) |
| Subdomain, such as `app.acme.com` | `Sub-tenants Wildcard (CNAME) · optional` | CNAME | `*.app.acme.com` | The same target |
| Root domain, such as `acme.com` | `Primary Domain (A) · required` | A | `acme.com` | `144.76.157.34` |

The `Verify Domain` button appears once a domain is saved and has no unsaved edits. It reads `Re-verify Domain` after a successful verification, and `Verifying...` while the check runs. Verification checks that:

- the hostname resolves in DNS
- it points to this platform, either by IP address or through a CNAME to the platform target
- it does not also point to other addresses, for example through registrar forwarding or parking
- when the deployment has a custom-domain target, a wildcard record for sub-tenants (`*.<your domain>`) also points to the platform

The hostname must also meet these rules:

- it has at least a name and a top-level domain of two or more letters
- each label is 1–63 letters, digits, or hyphens and does not start or end with a hyphen
- it is not an IP address, is not the platform's own domain, and is not a subdomain of the platform's tenant base domain
- it is not already used by another tenant, and is not a subdomain of another tenant's verified custom domain

When verification succeeds, the badge changes to `Verified` and a link to the domain appears next to `Domain Hostname`.

To remove the custom domain, clear `Domain Hostname` and click `Save changes`. The page confirms with `Custom domain removed.`

Once verified, the custom domain is used as follows:

- The custom domain opens your primary tenant. `www.<your domain>` opens the same tenant.
- With the wildcard record in place, `<sub-tenant slug>.<your domain>` opens that sub-tenant.
- Links that Orion generates for your tenant use the verified custom domain.
- HTTPS certificates are issued automatically on the first visit, only for verified custom domains and their mail hosts.
- The tenant's standard Orion address keeps working.
- Users sign in on their own tenant's address. An account from one tenant cannot sign in through another tenant's domain.

#### Webmail Configuration

This section decides which domain your Orion Mail mailboxes use. By default mailboxes use `mail.orionintelligence.org`. With a verified custom domain, a primary tenant can switch to a mail domain of its own.

The badge next to the section title shows the current state:

| Badge | Meaning |
| --- | --- |
| `Using Default Orion Mail` | Mailboxes use `mail.orionintelligence.org` |
| `Custom Domain Active` | Custom webmail is on and the custom domain is verified |
| `Configured by Primary Organization (Read Only)` | Shown to sub-tenants, which take this setting from their primary tenant |

The mail domain is always built on the root of your custom domain, not on the full hostname. Orion takes the last two labels of the custom domain as the root.

| Custom domain | Primary tenant mail domain | Sub-tenant mail domain |
| --- | --- | --- |
| `app.acme.com` | `mail.acme.com` | `<sub-tenant slug>mail.acme.com` |
| `acme.com` | `mail.acme.com` | `<sub-tenant slug>mail.acme.com` |

To turn on custom webmail as a primary tenant:

1. Set up and verify your custom domain first. Until it is verified and saved, the toggle is disabled and the page shows `Configure and verify your custom domain above to enable custom webmail addresses.`
2. Add the records listed under `Mail DNS Setup` at your DNS provider.
3. Click `Use Custom Domain for Webmail`. The text under it changes to `Mailboxes are created as user@mail.<root domain>`.
4. Click `Save changes`. The badge changes to `Custom Domain Active`.

`Mail DNS Setup` appears once a domain is entered in `Custom Domain`. For `app.acme.com` it lists:

| Record label | Type | Name / Host | Target / Value | Priority |
| --- | --- | --- | --- | --- |
| `Mail Exchange (MX)` | MX | `mail.acme.com` | `mail.orionintelligence.org` | 10 |
| `Sender Policy Framework (SPF)` | TXT | `mail.acme.com` | `v=spf1 include:mail.orionintelligence.org ~all` | |
| `Webmail (A)` | A | `mail.acme.com` | `144.76.157.34` | |

Orion does not check these mail records when you save. Add them before you turn the setting on, or mail for the new domain may not be delivered.

To go back to the default domain, click the toggle off and click `Save changes`. Changing or removing the custom domain also turns custom webmail off.

Sub-tenants cannot change this setting. A sub-tenant's mailboxes use its primary tenant's mail domain, with the sub-tenant slug in front, while the primary tenant has custom webmail on.

For the mailbox itself, see the Orion Mail chapter.

#### Network Configuration

`Network Configuration` holds the tenant's outgoing mail account, described on the page as `Mail delivery settings.` If the tenant has no saved account, Orion uses the platform's mail settings.

| Field | Meaning |
| --- | --- |
| `ACCOUNT MAIL` | The sending email address |
| `ACCOUNT MAIL PASSWORD` | The password for that account |
| `ACCOUNT SMTP SERVER` | The SMTP server hostname |
| `ACCOUNT SMTP PORT` | The SMTP server port |

To save the mail account:

1. Fill in all four fields. The password field is empty each time the page loads, so enter the password again whenever you save.
2. Click `Save changes`. Orion sends a test email, `SMTP configuration test`, to `ACCOUNT MAIL` with the entered settings and saves only if the test succeeds. On production deployments the connection uses SSL.
3. If the test fails, the page shows `Mail configuration is not working`.

Click `Verify Configuration` to send the same test email with the saved settings. The result is `Mail configuration is working` or `Mail configuration is not working`. Unsaved changes are not tested.

#### Alert Integrations

`Alert Integrations` forwards tenant alert notifications to Slack and Jira. Both providers are listed. Each row shows `Connected` or `Not connected`. Once connected, Slack shows the workspace and channel and Jira shows the site.

Expand a row to see its OAuth fields and connection notes:

- `SLACK CLIENT ID` and `SLACK CLIENT SECRET`, with the `Slack OAuth redirect URI for tenant connections`
- `JIRA CLIENT ID` and `JIRA CLIENT SECRET`, with the `Jira OAuth callback URI for tenant connections`

The redirect URI is your current address followed by `/api/alert-connectors/slack/callback` or `/api/alert-connectors/jira/callback`. Register it in the Slack or Atlassian app before connecting. The client secret is never shown after it is saved; the field shows `Configured` instead.

To connect a provider:

1. Expand the provider row, enter the client ID and client secret, and click `Save changes`.
2. Click `Connect` on the provider row. It opens the provider's authorization page in a new tab.
3. Approve access. The provider returns you to Tenant Settings.
4. To authorize again, for example to pick a different Slack channel, click `Reconnect`.

`Connect` is disabled until OAuth credentials are saved for that provider. If the saved settings cannot be loaded or saved, the section shows `Alert integration configuration is invalid`.

```{figure} ../../screenshots/tenant-alert-integrations-slack-20260326.png
:alt: Tenant Slack alert integration
:width: 100%

Tenant alert webhook integrations showing Slack as the only available configured connector.
```

#### Alert Schedule

`Alert Schedule` sets when automated alert scans run for the tenant.

1. Click the time field. It reads `Use default schedule` when no time is set.
2. Choose an hour (00–23) and a minute (in 5-minute steps).
3. Click `Save changes` in `Privacy & Access`. This section has no save button of its own.

To go back to the default schedule, click `×` in the time field and save `Privacy & Access`. The default schedule runs daily at 02:00. Both the default and custom times are in the platform's alert time zone, Australia/Sydney.

#### Privacy & Access

`Privacy & Access` controls profile visibility, alert visibility, and access to Event Management. Click a toggle card to switch it, then click `Save changes`.

| Toggle | Default | When on | When off |
| --- | --- | --- | --- |
| `Allow User Profile Visibility` | On | `Users can manage their profile visibility` | `User profile visibility is disabled for this tenant`; other users who open a profile from this tenant see `Profile hidden by tenant` |
| `Allow Event Management` | Off | `Event Management is enabled for this tenant`; Event Management appears in Monitoring for admins and maintainers | `Event Management is disabled for this tenant` |
| `Allow Admin Alert Visibility` | On | `Tenant alerts are visible to admin` | `Tenant alerts are hidden from admin` |
| `Allow Parent Tenant Alert Visibility` | On | `Tenant alerts are visible to parent tenant` | `Tenant alerts are hidden from parent tenant` |

Only sub-tenants see `Allow Parent Tenant Alert Visibility`. It sits next to the admin visibility toggle, and the two are independent: hiding alerts from administrators does not hide them from the primary tenant, and hiding them from the primary tenant does not hide them from administrators. Turning either toggle off also removes the tenant from the alert-access lists of users on the side that lost visibility.

## User and Tenant Administration

### Tenant Users

The `Users` view is the main tenant user-management page.

It supports:

- viewing users in a table or mobile card layout
- adding a user
- expanding a user row for details
- changing status
- editing assigned licenses
- editing user permissions where available
- limiting alert-administration visibility to all tenants or selected tenants where enabled
- deleting a user

Displayed information commonly includes:

- username
- email
- role
- status
- subscription
- licenses
- permissions
- alert access scope where enabled

The page also respects quota-based restrictions.

Per-user permissions are assigned here when adding or editing a user. Assignable permissions include `Case Management`, `Orion Mail`, and `Dismiss Result`. The `Dismiss Result` permission controls who can dismiss or restore stealer-log and credential results for the tenant (see [Dismissing Results](#dismissing-results)); users without it can view results but cannot dismiss them.

The broader tested user-management lifecycle also covers:

- creating multiple users with different roles and license mixes
- verifying role- and license-based sidebar visibility after login
- triggering subscription or paywall behavior for limited-license users
- showing near-expiry trial state messaging where applicable

```{figure} ../../screenshots/tenant-users-20260326.png
:alt: Tenant users page
:width: 100%

Tenant user-management view with quotas, roles, and licenses.
```

### Tenant Administration

The `Tenants` view is used by higher-privilege roles to manage tenant records across the platform.

It supports:

- reviewing tenant information
- expanding a tenant for detail and editing
- changing verification state
- changing quota
- marking a tenant as a primary tenant and setting the tenant quota that limits how many sub-tenants it may hold
- reviewing and editing sub-tenants as the maintainer of a primary tenant
- changing status
- updating tenant licenses
- setting tenant alert run time in `HH:mm` 24-hour format
- controlling whether tenant alerts are visible to admins
- configuring allowed alert scanner categories
- enabling Privileged IOC handling where required
- assigning which tenants a user can view in tenant-alert administration workflows
- deleting a tenant and its associated users and keys after confirmation

Displayed fields include:

- company name
- country
- subscription
- verification state
- user quota
- primary tenant state and tenant quota
- the owning primary tenant on sub-tenant records
- status
- license assignments
- alert visibility and alert run-time settings where enabled
- privileged IOC state where enabled

Admin tenant-alert views can summarize alerts across visible tenants, filter or search tenants from a multi-select dropdown, open category-specific alert drilldowns, review risk summary cards, and export a tenant's alerts as a PDF report.

Administrators delete top-level tenants, and the maintainer of a primary tenant deletes its own sub-tenants. The root/default tenant cannot be deleted, and an administrator cannot delete or edit a sub-tenant directly; those records are managed through their primary tenant.

To delete a tenant:

1. Sign in as an administrator, or as the maintainer of the primary tenant that owns the sub-tenant, and open `Tenants`.
2. Find the tenant and select its red delete button.
3. Review the confirmation message and select `Yes, Confirm`.
4. Confirm that the tenant disappears from the list.

This permanently removes the tenant and its associated users and keys. Deleting a primary tenant also removes every sub-tenant beneath it, together with their users and keys.

(primary-tenants-and-sub-tenants)=
#### Primary Tenants and Sub-Tenants

**Secondary tenant** and **sub-tenant** mean the same thing: a tenant owned by a primary tenant. The hierarchy has one parent-child level; a sub-tenant is not another independent primary tenant.

| Tenant type | Who manages it | Relationship |
| --- | --- | --- |
| Root/default tenant | Platform administrators | Platform administration and top-level provisioning. |
| Standalone tenant | Its maintainer for permitted operations; administrators for platform-controlled settings | No parent and no sub-tenants. |
| Primary tenant | Its maintainer, within administrator-assigned limits | Owns and manages its sub-tenants. |
| Secondary / sub-tenant | Its own maintainer for local users and settings; its primary maintainer for tenant administration | Belongs to one primary tenant. |

##### Create a Primary Tenant

1. Register a top-level tenant through signup on the root platform domain and complete its verification and onboarding.
2. Sign in as a platform administrator and open **Tenants**.
3. Expand the tenant, enable **Primary Tenant**, and set **Tenant Quota** and **User Quota**.
4. Review its status, verification, and licenses, then save.
5. Give the primary maintainer the tenant's dedicated URL for onboarding its sub-tenants.

Promotion raises quotas to the primary defaults when they are lower: **15 users** and **5 sub-tenants**. Non-primary tenants accept a user quota of up to **8** from this page. Review the saved values rather than assuming the pre-promotion limits remain unchanged.

##### Create a Secondary / Sub-Tenant

1. Open the **primary tenant's URL** and select **Sign Up**. Signup on the root platform domain creates a top-level tenant instead.
2. Complete the signup details for the new organization and follow the verification email, which points back to the primary tenant URL.
3. As the primary maintainer, open **Tenants** and review the new sub-tenant's verification, status, licenses, and user quota.
4. Allocate the sub-tenant's permitted quota and licenses, then save.
5. Use the sub-tenant's dedicated URL to sign in as its maintainer, finish onboarding, and add its users in **Tenant Users**.

If the primary tenant has reached its tenant quota, **Sign Up** is hidden and signup is refused until a sub-tenant is removed or the quota is increased. **Add User** in Tenant Users creates an account in an existing tenant; it does not create a secondary tenant.

##### Allocate Quotas and Licenses

The primary user quota covers its own counted users **plus the user quotas reserved for its sub-tenants**. Maintainer accounts are excluded from user counts. For example, with a primary quota of 15, four counted primary users and sub-tenant reservations of 3 and 5 consume 12 slots, leaving 3 available. Each sub-tenant is also limited by its own reservation.

A sub-tenant created through signup starts with a user quota of **1** if there is room in the pool, or **0** otherwise. Freeing a reserved slot may require lowering the sub-tenant's allocation; deleting a user alone does not reduce that reservation.

The primary maintainer can:

- review and administer its own sub-tenants in **Tenants**;
- change verification, status, and user quota within the available pool;
- assign licenses from the set held by the primary tenant;
- enable Privileged IOC or the AI endpoint only when the primary already has that capability;
- require a password reset for a sub-tenant maintainer;
- review sub-tenant alerts when parent visibility is enabled;
- review takedown requests owned by its tenant family.

Administrators manage sub-tenants through the primary tenant rather than editing or deleting them directly. Turning off **Primary Tenant** disables its sub-tenants. Reducing the primary's licenses also trims its sub-tenants' license sets. Deleting a primary tenant deletes its sub-tenants and their users and keys as well.

##### When Access Is Temporarily Disabled

A missing, unverified, disabled, or no-longer-primary parent can block its sub-tenants. Exceeding the primary's user or tenant quota can also block associated sub-tenant accounts. The responsible operator sees the relevant quota warning and must reduce usage or reserved quotas, remove tenants where appropriate, or obtain an increased allocation.

When a signed-in account is blocked, the **Account temporarily disabled** screen directs the user to their network administrator and provides **Log out**. Some quota or account-verification failures can instead prevent sign-in; pass that message to the responsible administrator. The screen does not disclose the underlying administrative reason. A sub-tenant member's subscription checks also follow the primary tenant's subscription state.

For the corresponding backup permissions, see [Backup and Restore](#backup-and-restore).

#### Dedicated Tenant Subdomains and White-Labeling

Each tenant uses a dedicated subdomain, such as `<tenant-slug>.<platform-domain>` (`<tenant-slug>.localhost` in local environments). Tenant accounts sign in through that tenant URL, keeping authentication isolated from the main platform domain. The platform resolves the tenant from the request host on every call, so the branding, public configuration, signup availability, and login screen a visitor sees are the ones belonging to that subdomain. A host that matches no tenant is rejected.

Branding is tenant-scoped, so the application name, favicon, light and dark logos, and public header links can be customized without changing other tenants.

Who can change branding:

- an administrator can change every platform setting for the tenant being edited
- a user holding the `maintainer` license can change the branding of its own tenant only, limited to the application name, the onion address, and the public data-source, adversary, and pricing links
- settings such as version, allowed language, admin-panel visibility, backup schedule, and platform mail credentials stay administrator-only
- a maintainer cannot edit another tenant's settings, including the settings of its own sub-tenants

Brand assets:

- three images are white-labeled per tenant: the primary logo used as the favicon, the wide light logo, and the wide dark logo
- uploads are limited to image files of at most 100 KB
- each tenant's uploads are stored against that tenant, so one tenant's logo never appears on another
- removing a custom asset restores the stock image shipped with the platform
- a tenant that has not uploaded an asset automatically falls back to the stock image
- replacing an asset takes effect immediately, because asset URLs are versioned

New tenants start from an inherited brand. When a tenant is created, its settings and uploaded logos are copied from a source tenant: the parent tenant for a sub-tenant, and the root tenant for a top-level tenant. A sub-tenant therefore opens already wearing the white-label identity of its primary tenant, and can then diverge from it. Version, language, admin-panel, and onion values are not inherited, and the AI endpoint starts disabled until it is enabled for that tenant.

The tenant image shown on the tenant overview is separate from these brand assets and is uploaded from `Tenant Settings`.

```{figure} ../../screenshots/system-settings-20260326.png
:alt: Tenant white-label branding settings
:width: 100%

Brand assets and application identity controls used for tenant-level white-labeling.
```

```{figure} ../../screenshots/tenant-administration-20260326.png
:alt: Tenant administration page
:width: 100%

Administrative tenant-management table used for verification, licensing, and quota updates.
```

### Audit Logs

Audit Logs provide a searchable activity trail across user and tenant actions.

```{admonition} Audit data shown
:class: note

The audit log list typically shows a timestamp, actor, tenant, and event description for each recorded entry.
```

The audit-log page supports:

- export
- filtering
- pagination
- desktop and mobile layouts

```{figure} ../../screenshots/audit-logs-20260326.png
:alt: Audit log page
:width: 100%

Audit log workspace with filters and export actions.
```

## System Administration

### System Settings

System Settings is the primary platform-level configuration page.

It includes two main groups:

- asset and branding configuration
- application and service configuration

#### Asset Management

Administrators can manage brand and UI images such as:

- primary logo
- wide light logo
- wide dark logo
- authentication dashboard icon

On a tenant subdomain the same page manages that tenant's own assets, so a maintainer uses it to white-label its tenant. See `Dedicated Tenant Subdomains and White-Labeling` under [Tenant Administration](#tenant-administration) for the editing rules, size limits, and inheritance behavior.

#### Configuration

Editable platform settings can include:

- application name
- language
- onion address
- AI endpoint visibility
- admin panel visibility
- documentation, pricing, data-source, and adversary public URL settings
- account email and SMTP settings for platform mail delivery
- version and platform metadata
- logo and authentication dashboard image URLs

#### Alert Webhook Integrations

Administrators configure platform OAuth credentials for alert webhook integrations from System Settings. System Settings stores the Slack and Jira app credentials and redirect URI notes only; tenants connect their own webhook destinations from Tenant Settings.

#### Backup

The Backup card controls whether Orion Intelligence creates backups on its own schedule.

When Scheduled Backup is enabled, the platform creates a backup automatically every 3 days. The toggle saves immediately; there is no separate save action for it.

Only the **2 most recent system backups** are retained. After a new backup completes successfully, older backups beyond that limit are removed. Scheduled and manually created backups share this limit, so scheduled backups can eventually displace older manual recovery points. Download any recovery point you need to keep before it leaves the retained set.

```{figure} ../../screenshots/alert-integrations-system-slack-config-20260326.png
:alt: System Slack alert integration configuration
:width: 100%

System alert webhook integration settings for configuring Slack OAuth credentials.
```

```{figure} ../../screenshots/system-settings-20260326.png
:alt: Administrative and system settings workspace
:width: 100%

Administrative settings and platform-management view.
```

(tenant-onion-addresses)=

#### Tenant Onion Addresses

Orion can publish a tenant on the Tor network under its own onion (`.onion`) address. Users with Tor Browser can then reach the tenant's login page and workspace without going through the tenant's normal web domain.

##### Who Can Assign an Onion Address

| User | Where the button appears | Tenant affected |
| --- | --- | --- |
| Administrator of the root (default) tenant | `System Settings > Platform Settings > Configuration` | Root tenant |
| Maintainer of a primary tenant | `System Settings > Branding > Tenant identity` | That primary tenant |

The `Assign Tor address` button is hidden for everyone else, including maintainers of sub-tenants and of tenants that are not primary. The account must also be active.

##### Assign or Replace the Address

1. Open `Profile > System Settings`.
2. Open the tab that contains `Onion Address` (see the table above).
3. Select `Assign Tor address` next to the `Onion Address` field.
4. Wait while the icon spins. On success Orion shows `Tor address assigned` and fills the field with the new `http://<address>.onion` URL.

The address takes effect as soon as it is assigned. You do not need to select `Save changes` to activate it.

Selecting `Assign Tor address` again creates a brand-new address and retires the old one. The previous `.onion` URL stops working, so share the new URL with your users.

```{admonition} Replacing an address breaks old links
:class: warning

Each assignment generates a new random onion address. Bookmarks and shared links that use the previous address stop working.
```

##### How the Address Is Used

- The tenant's onion URL is shown to its users as `Onion Link` in the sidebar, below `Help & Support`.
- Requests that arrive on the onion address open the owning tenant, with that tenant's branding and login page. Users still sign in to their own tenant only.
- Sub-tenants of a primary tenant do not need their own address. Each sub-tenant is automatically reachable at `http://<sub-tenant-slug>.<primary-onion-address>.onion`, and that URL is shown as its `Onion Link`.
- An assigned address takes priority over any value typed into `Onion Address`. A typed value is used only when no address has been assigned. Typed values must be a 56-character v3 onion address, otherwise saving shows `Invalid onion address`.
- Tenants with no assigned or typed address inherit the root tenant's saved onion value, shown as the placeholder `Inherit from root tenant`.

Orion keeps the onion service's private key encrypted with the tenant's key. Every 60 seconds the platform checks Tor and republishes any assigned address that is missing, for example after the Tor service restarts. It also removes addresses that no longer belong to any tenant. A newly assigned address can take a few minutes to become reachable across the Tor network.

##### Status and Errors

| Message | Meaning | What to do |
| --- | --- | --- |
| `Tor address assigned` | The address was created and saved | Share the new `Onion Link` with users |
| `Tor service is unavailable` | Orion could not reach or authenticate to the Tor service | Ask the platform operator to check the Tor service, then try again |
| `Only the root admin or a primary tenant maintainer can assign an onion address` | Your account is not allowed to assign an address for this tenant | Ask the root administrator or the primary tenant's maintainer |
| `Tenant not found` | Your tenant could not be resolved | Contact the platform administrator |
| `Unable to assign a Tor address` | Another, unexpected error occurred | Try again; if it continues, contact the platform administrator |

### Backup and Restore

Open **Backup & Restore** from the profile/administration area (`/dashboard/profile/backup-restore`). The available controls depend on your role and tenant. Administrators manage system backups; eligible tenant maintainers download, restore, or import their permitted tenant data.

#### How Backups Work

Orion creates a **system backup**, then stores tenant-specific portions inside it. These portions make it possible to recover an organization without restoring the whole platform. A tenant export is extracted from a stored backup: downloading or exporting does **not** create a fresh snapshot.

| Operation | What it produces or changes |
| --- | --- |
| Instant Backup | An administrator creates a new system recovery point. |
| Scheduled Backup | The platform creates a system recovery point automatically on the configured three-day loop when enabled. |
| Download on an administrator's backup list | Downloads the selected whole-system backup for administrative recovery. |
| Download on a maintainer's backup list | Downloads that tenant's portion of the selected recovery point. A primary portion includes its backed-up sub-tenants. |
| Export on a tenant row | Downloads the latest available backup portion containing that tenant. |
| Restore | Replaces the selected system or tenant scope with an existing stored recovery point. |
| Import | Restores an authorized tenant export ZIP uploaded from your computer. |

The system backup contains MongoDB data, ArangoDB graph data, Elasticsearch data, application logs, resource files, session resources, and tenant recovery portions. It is an application backup, not a complete operating-system image. Infrastructure snapshots, deployment configuration, and server encryption-key recovery need to be managed separately by the platform operator.

A tenant portion contains its tenant-owned database records, users and related records, tenant-scoped search data where supported, and associated files. It does not give a maintainer the platform's shared search corpus or unrelated tenants' data. A primary portion contains the sub-tenant portions present at the time of that backup.

The list identifies each backup by name, creation date, and type: **auto** or **instant**. The current retention limit is **2 system backups total**, shared by both types. A new successful backup removes older entries beyond that limit.

#### Who Can Use Which Backup

| Caller | Available scope |
| --- | --- |
| Standalone tenant maintainer | Download, restore, and import its own tenant. |
| Primary tenant maintainer | Its own tenant and associated sub-tenants; restoring its primary backup includes the sub-tenants stored in that backup. |
| Secondary / sub-tenant maintainer | No independent backup controls; request recovery or export through the primary maintainer. |
| Platform/root administrator | System backup management and authorized tenant recovery. Tenant export/import of a secondary is performed through its primary bundle. |

Tenant maintainers do not have **Instant Backup**, system-backup deletion, or the schedule toggle. They use recovery points created by administrators or the schedule. A tenant created after the most recent backup will not appear in that recovery point; ask the administrator for a new backup before trying to export it.

#### Create a System Backup

1. Sign in as an authorized administrator and open **Backup & Restore**.
2. Select **Instant Backup** and review the confirmation. When two backups already exist, the confirmation explains that the oldest will be removed after a new successful backup.
3. Confirm and watch the progress percentage and current stage.
4. Wait for the completion message and refreshed list, then check the creation date.
5. Download the new backup if you need a retained copy outside the server's two-backup rotation.

To enable recurring backups, open **System Settings > Backup** and turn on **Scheduled Backup**. The toggle saves immediately. The background loop checks the setting and runs at three-day intervals; enabling the toggle is not a substitute for **Instant Backup** when you need a current recovery point now.

Only one backup, restore, or import job can run at a time. If a job is already running, wait for its final status before submitting another operation. A started upload or a progress bar is not confirmation that a recoverable backup or restore has completed.

#### Export a Tenant

1. Open **Tenants** and select **Export** on a tenant you are allowed to manage. A standalone maintainer can use **Download** on its own Backup & Restore list instead.
2. Review the recovery date in the export confirmation. The tenant-row export uses the latest stored backup containing the tenant, and warns when it is more than 24 hours old.
3. Confirm and save the ZIP. Keep its original filename to retain the timestamp used by import confirmations.
4. Extract a copy locally and open **index.html** to review the contents summary. Keep the original ZIP intact for a future import.

**The backup date is the recovery point.** For example, a backup taken on Monday and exported on Thursday still restores Monday's data. Changes made after Monday are not included. Ask an administrator for an Instant Backup first if those later changes must be preserved.

#### Read the Export's index.html Statistics

A tenant export ZIP contains a folder with these files:

```text
tenant-export/
  index.html   — readable statistics and recovery-point summary
  tenant.enc   — encrypted tenant data used by Import
```

The outer folder name varies with the backup and tenant. Open `index.html` in a browser after extraction; it is a static summary that does not need a running Orion server or decrypt the payload.

The summary shows:

- **Backup**: the source backup's name.
- **Backup taken**: the time of the stored recovery point.
- **Exported**: when this download was prepared.
- **Tenants**: how many tenant portions are included.
- A section for each tenant, identified by slug or ID; child tenants carry a **secondary** badge.
- Per-tenant **Users**, **Documents**, **Search documents**, and **Files** totals.
- Database collection and search-index tables with their document counts.

These are backup statistics, not live counts or a browser for individual users, passwords, messages, or records. The **Users** total describes stored accounts, not remaining license/quota capacity. The encrypted `tenant.enc` contains the actual restore data. Editing the HTML summary does not edit or restore that data.

Tenant imports accept the encrypted export produced by a compatible Orion server. Modified payloads and older plain-data ZIPs are rejected. An export from another installation requires compatible server encryption-key configuration; coordinate migration with the platform administrator rather than assuming any Orion installation can import it. The administrator's whole-system ZIP is a different format and is not accepted by the tenant **Import** action.

#### Read the System Backup's Statistics

An administrator's whole-system download also includes **index.html**, but uses the broader backup overview rather than the two-file tenant-export layout. Extract the ZIP and open the top-level report locally to review its embedded backup summary.

- **Overview** summarizes saved items, information types, categories, organizations, and listed files.
- **Account details**, **Connections**, and **Findings** summarize the MongoDB, ArangoDB, and Elasticsearch portions respectively.
- **Organizations** shows each included tenant's saved items, people, files, and findings; selecting an organization opens its category summary.
- Use **Search this view** to filter the current table and **Hide empty** to hide categories with no saved items.
- **Files** lists files represented by the report. Counts come from the backup summary, not a fresh live-system query.

The local report can show embedded statistics without reading neighboring data files. Record-level browsing requires the report's data-file loading mode and access to the accompanying files; do not publish an administrative backup folder just to enable browsing. The tenant export's **index.html** remains a static statistics-only report with its actual data in **tenant.enc**.

#### Restore From a Stored Backup

1. Open the correct Backup & Restore scope: your organization for tenant recovery, or the administrator view for a whole-system restore.
2. Review the selected backup's creation date and scope before choosing **Restore**.
3. Read the confirmation carefully. A restore replaces current in-scope data; later changes may be lost. It is not an append or merge operation.
4. Confirm and follow the job's progress until it reports success or failure.
5. Reopen the affected organization and check its users, records, and files after completion.

A tenant restore temporarily places the affected tenant and included sub-tenants in maintenance while data is replaced. Unrelated tenants remain available. A whole-system restore places the site in maintenance.

For an existing tenant, current administrator-controlled tenant settings such as licenses, quotas, status, and parent relationships are retained rather than blindly reverting to older exported values. Records now owned by another tenant, or conflicting with its records, are left out; the completion message reports skipped records when applicable. Ask the administrator to inspect the job logs if that message appears.

#### Import an Exported Tenant ZIP

1. Open **Backup & Restore** as an authorized maintainer or administrator and choose **Import**.
2. Select the original tenant export ZIP, not `index.html`, `tenant.enc` by itself, or a whole-system backup.
3. Review the confirmation's recovery date and affected scope. If the file was renamed and its date cannot be determined, inspect `index.html` before proceeding.
4. Confirm the replacement. A primary export can restore the primary and its included sub-tenants together.
5. Wait for upload, validation, and the restore job to finish, then verify the result in the application.

The server checks ownership before replacement and verifies the tenant relationships in included child portions. A file belonging to an unrelated tenant is not an alternative way to access or overwrite it.

#### Failure and Recovery Messages

The tenant restore process first creates a rollback snapshot. If replacement or validation fails, it attempts to restore the previous state and reports that the operation was rolled back. An interrupted tenant restore is also checked for recovery when the service starts. If automatic rollback fails, the job reports that operator intervention is required; contact the platform administrator instead of repeatedly importing the same archive.

| Message or situation | Next step |
| --- | --- |
| No backups available / no backup contains this tenant | Ask an administrator to complete a system backup after the tenant was created. |
| Another backup or restore is running | Wait for the active job's final status. |
| Export belongs to another tenant | Use the correct tenant account or ask the owning primary/root administrator. |
| Sub-tenant backup is part of the primary backup | Ask the primary maintainer to export or restore the permitted scope. |
| File is not a tenant export / rejected payload | Use the original supported export ZIP and confirm server compatibility. |
| Archive too large / insufficient space | Ask the platform operator to check upload limits and available recovery storage. |
| Restore rolled back or requires manual intervention | Read the final job message and contact the administrator if access or recovery remains blocked. |

For signup and quota setup, see [Primary Tenants and Sub-Tenants](#primary-tenants-and-sub-tenants).

## Detailed UI Coverage Appendix

This appendix documents exact user-visible behaviors verified during documentation coverage. It is intended to close the gap between a feature overview and the concrete interactions that an operator, tenant user, or administrator can perform in the current product.

### Authentication and Session Lifecycle

The tested authentication lifecycle includes:

- loading the login page from the public entry point
- signing in as an administrator
- keeping the browser access token in an encrypted HTTP-only cookie rather than local storage
- applying progressive delays after consecutive unsuccessful login attempts
- opening the profile menu and signing out
- requesting a password-reset email
- receiving the same reset-request result regardless of whether an account exists
- requesting recovery with a registered email and global recovery key
- opening a tokenized reset-password screen
- validating that the new password cannot match the old password
- applying a new password successfully
- signing in again with the updated password
- encountering a two-factor prompt after enabling `2FA`
- viewing the 2FA QR image and OTP input state
- requiring the current password before password, 2FA, or recovery-key changes

### Sidebar and Global Navigation States

The automation covers both content navigation and structural sidebar behavior.

Supported navigation behaviors include:

- expanding and collapsing sidebar groups
- collapsing the whole sidebar
- re-expanding the whole sidebar
- visiting all major search, scan, graph, tenant, and admin groups that are available to the current role
- opening external or support-oriented entries from the main navigation where configured

For user documentation purposes, that means the sidebar is not only a static menu. It is expected to support:

- nested group expansion
- condensed and expanded display modes
- role-based visibility
- license-based visibility

The profile menu is also part of this navigation model. Tested behavior includes:

- opening the profile menu
- reaching Help & Support from the profile menu
- signing out from the profile menu

### Homepage, Heatmap, and Support Interactions

The homepage is validated as more than a search landing page. The automated flow covers:

- world heatmap rendering
- tooltip visibility on country hover
- tooltip hide behavior on pointer leave
- opening a country-level report from the map
- closing the country report with the close button
- closing the same report by clicking the overlay
- fallback behavior when heatmap data or world data changes

The support workflow is also covered directly from the profile menu:

- opening the Help & Support modal
- filling email, subject, and message fields
- submitting the support request

### Search Behavior and Result Expectations

Documentation coverage validates that indexed modules are not only searchable but also return stable, inspectable result structures.

Covered search behavior includes:

- general keyword searching
- module-specific searching
- result opening from cards and table rows
- returning from a report to the original listing
- opening reports in both modal-style and page-style layouts
- validating first-result content against fixtures in key modules

The search-result verification suite explicitly checks stable first-result expectations for:

- `General Intelligence`
- `Data Breach`
- `Compromise Monitoring`
- `Social`
- `Exploit`
- `Feed`

This means the manual should treat these modules as search-first experiences with expected, stable result-card or row-based layouts, not as experimental views.

### Indexed Module and Tab Coverage

The suite covers more module variations than the earlier manual described explicitly.

`General Intelligence` coverage includes:

- `All`
- `General`
- `Forums`
- `News`
- `Stolen`
- `Drugs`
- `Hacking`
- `Marketplaces`
- `Cryptocurrency`
- `Leaks`

`Data Breach` coverage includes:

- `All`
- `Databases`
- `Tracking`
- leak URL and dump-reference fields inside breach reports

`Compromise Monitoring` coverage includes:

- `All`
- `Hacked`
- `Phishing`
- `Databases`

`Social` coverage includes:

- `All`
- `Telegram`
- `Twitter`
- `Mastodon`
- `Pastebin`
- `Forum`
- `Reddit`

`Exploit` coverage includes:

- `All`
- `CVE`
- `Tools`
- `ZeroDay`

`Feed` coverage includes:

- `News`

`Stealer Logs` coverage includes:

- `IOCS`

### Report Opening, JSON Review, and Chat Workflows

Report handling is one of the most deeply exercised areas of the suite.

Covered behaviors include:

- opening the first available report from multiple modules
- verifying that a report can open as a page or modal, depending on module layout
- opening JSON-backed report viewers
- closing modal reports with escape
- opening chat from a report
- sending a chat message
- verifying that a chat response area renders messages

The manual should therefore treat chat and JSON review as first-class report features, not optional side notes.

### Search Tools and Advanced Filters

The suite covers two layers of filtering:

- toolbar-level search tools
- sidebar filter drawers

Toolbar-level coverage includes:

- toggling `Advance`
- opening `Tools`
- changing result sort order
- switching search behavior between semantic, OR, AND, and full-query modes
- clearing entity-filter selections

Sidebar-filter coverage includes:

- network filtering
- safe-search filtering
- content-type filtering
- date-range filtering
- reset
- apply
- auto-apply and manual-apply variations

The tests also verify these filters across multiple modules, including:

- `General Intelligence`
- `Data Breach`
- `Compromise Monitoring`
- `Social`
- `Exploit`
- `Feed`

Advanced resilient filter validation also scans report detail and metadata after filtering, which means filtering is expected to affect downstream report inspection, not just the list page.

For users, that means the filtering model should be understood as end-to-end rather than cosmetic. The tested behavior confirms:

- search-tool mode changes affect the actual returned result set
- sort order changes are preserved into refreshed searches
- side filters can be applied repeatedly across different modules
- date filters support both matching and intentionally empty result windows
- filtered state is expected to remain meaningful when opening report detail and metadata panels

### Pagination, Load More, and Result Expansion

The suite validates navigation through large result sets rather than assuming a single-page result view.

Covered pagination and expansion behaviors include:

- next-page navigation in `General Intelligence`
- next-page navigation in `Data Breach`
- next-page navigation in `Compromise Monitoring`
- next-page navigation in `Social`
- next-page navigation in `Exploit`
- next-page navigation in `Feed`
- directory pagination
- directory page-number navigation
- directory lazy expansion by scrolling to the bottom
- stealer-log row expansion
- IOC row expansion in consolidated tables
- consolidated `See More` and `See Less` toggles where present

This matters operationally because the interface is tested as a browsing workspace, not only a single-query landing page. Users should expect:

- multi-page navigation in indexed modules
- progressive loading where directory-style surfaces support it
- expandable rows and cards in result-heavy modules
- persistence of the browsing context while moving in and out of details

### Stealer Logs: Full Tested Behaviors

In addition to the broader description above, the stealer-log suite covers:

- tag-based basic searching
- advanced row-based condition building
- validation of empty or invalid search states
- result download initiation
- password-scheme modal opening
- password-length and character-class filtering
- helper-driven pivots from results
- expansion of matched credential rows
- review of email and telemetry fields inside expanded rows

This means Stealer Logs should be understood as a full hunting workspace with both simple and compound-query modes.

### Consolidated: Full Tested Behaviors

The consolidated area is one of the deepest tested surfaces in the application.

Covered behaviors include:

- opening `Deep Search`
- opening `IOCs`
- using the profile-scoped consolidated view
- searching from the homepage into consolidated
- reviewing defacement-style threat cards inside deep search
- expanding and collapsing grouped threat cards
- inspecting keyword and coverage insight sections
- expanding all insight sections
- searching inside the threat-actor insight panel
- testing no-match behavior inside insight search
- opening report details from consolidated results and returning
- filtering consolidated results by network
- validating that filtered result cards reflect the chosen network
- opening the domain-scanner modal
- running subdomain scans
- running IP lookup when available
- running wayback-style scans when available
- closing the domain-scanner modal
- opening IOC tables for stealer and threat entries
- expanding the first several IOC rows
- switching IOC search terms and validating both non-empty and empty states
- downloading IOC results
- applying password-scheme filters from the consolidated IOC context
- applying date filters that produce both non-empty and empty results

The consolidated right-side insight panel should therefore be considered part of the documented workflow, not an ancillary convenience.

### CTI Graph: Full Tested Behaviors

The CTI suite covers substantially more than opening the graph.

Covered CTI behaviors include:

- switching graph filter type to `Cluster`
- applying CTI filters
- opening the Advanced Graph Builder
- adding multiple advanced-builder rows
- selecting searchable advanced-builder fields
- joining builder rows with `OR`
- executing advanced-builder filters
- clearing generated builder filter chips
- searching the graph toolbar
- validating highlighted results
- opening export-report modals
- switching between graph and list views
- collapsing and expanding the listings panel
- toggling physics simulation
- creating a new CTI session
- renaming a CTI session
- exporting the current session through the `Export Current Session` action
- importing a session from JSON
- closing a session tab
- selecting export format options such as JSON and graph PDF
- opening a context menu from the graph canvas

There is also component-level branch coverage for:

- graph-change handling
- empty category handling
- rotated category sets
- report retrieval by country

Those coverage points confirm fallback and re-render behavior in the current UI.

### Social Intel: Full Tested Behaviors

The social graph area is also extensively exercised.

Covered behaviors include:

- scanning a username
- switching between graph and list views
- clearing graph search
- creating and renaming a social session
- exporting a social report
- opening the add-entity modal
- validating disabled and enabled submit states
- submitting both guided lookup and manual entity entries
- triggering a graph context-menu path
- opening image-based profile search
- uploading an image for recon
- reviewing the manage-profiles modal
- filtering discovered platforms
- fetching profiles
- selecting all discovered profiles and pushing them into the graph
- opening summary popups
- searching profile metadata with tokens
- opening external profile links
- fetching followers
- fetching following
- fetching images
- rescanning a profile
- reopening manage-profiles and cancelling
- opening follower/following scan popups
- switching among followers, following, and connections tabs
- selecting discovered related accounts
- confirming follower/following imports
- opening relationship popups from graph nodes
- opening related account links
- setting an alias through the context menu

This is one of the richest modules in the product and should be documented as a multi-step graph, list, and modal workflow rather than only as a graph view.

### Entity Lookup and Scan Modules: Full Tested Behaviors

Documentation coverage includes every documented live lookup workflow currently present in the main product:

- `Email Breach`
- `Social Scanner`
- `Wanted List`
- `National Identity`
- `Playstore Scanner`
- `Software Scanner`
- `File Scanner`
- `Crypto Scanner`

It also covers the web-scan routes:

- `Basic Scan`
- `Port Scan`
- `Repository Scan`
- `SEO Scan`
- `APK Scan`

Specific validated actions include:

- submitting text lookups
- submitting file uploads
- showing success badges
- downloading reports
- printing reports
- resetting file-upload flows with `Analyze Another File`
- re-uploading and re-running the same scanner after reset

The tested scan and lookup journeys are therefore more specific than a single generic “scan” action. They include:

- email-driven breach validation
- social handle lookups
- wanted-person lookups
- national identity checks
- Playstore package lookups
- software-name searches
- file-upload IOC extraction
- cryptocurrency address or hash lookups
- web-target scans for basic, port, repository, SEO, and APK workflows

### Network Intel: Full Tested Behaviors

The Network Intel suite covers:

- host recon search
- IP scan search
- vulnerability scan search
- target selection inside vulnerability scanning
- depth-aware vulnerability scanning controls
- detail row expansion and collapse
- downloading reports from each main network-intel tab
- export-trigger validation

The Geo IoT modal is also covered end to end, including:

- opening the modal
- closing with the close control
- closing with the cancel control
- switching between map mode and manual mode
- zooming in and out on the map
- editing coordinates manually
- editing radius
- editing max-IP count
- switching back to map mode
- starting a geo scan
- reusing the selected coordinates as the active network-intel query

### Satellite Map: Full Tested Behaviors

The Satellite Map documentation flow covers the embedded Geo Fencing map workspace inside consolidated results.

Covered behaviors include:

- loading the Satellite Map from the dashboard
- loading indexed map entities
- rendering the Leaflet map before screenshots are captured
- selecting all loaded map-entity categories
- showing loaded and visible entity counts
- switching from the street map layer to the satellite imagery layer
- searching loaded map entities from the dashboard panel
- selecting a search result and updating the selection panel
- opening the geocode location modal
- applying coordinates from the location modal
- loading nearby facilities
- showing nearby facility counts and type breakdowns
- enabling aircraft tracking
- enabling ship tracking
- showing aircraft and ship counts in the tracking and facilities panels
- opening the panel menu
- switching to `Imagery Analysis`
- loading comparison imagery from the satellite imagery flow
- running anomaly analysis
- rendering comparison and anomaly output before capture

### Threat Lens: Full Tested Behaviors

The Threat Lens documentation flow covers the standalone Threat Lens workspace.

Covered behaviors include:

- loading the Threat Lens page from the dashboard
- rendering the map fallback during documentation capture
- loading consolidated Threat Lens data
- ranking top highlighted countries from consolidated country metadata
- rendering category-layer rows for leak, tracking, news, exploit, defacement, chat, social, and generic records
- rendering live news feed records
- rendering archive feed records
- running the default IP exposure scan through the Network Intel geo scanner
- showing IP scan scope, radius, status, and marker count
- searching Threat Lens with a keyword
- showing the active keyword state
- applying local archive-feed search
- switching feed range filters
- opening and capturing the Threat Lens filter drawer

### Directory: Full Tested Behaviors

The directory workflow is covered as an operational browsing surface rather than a search-first module.

Covered behaviors include:

- initial page load
- table and empty-state validation
- progressive loading by scrolling
- pagination to page two and back to page one
- filtering by network
- filtering by index
- filtering by content type
- applying and clearing date ranges
- full filter reset

### Account Settings, Preferences, and Reset Journey

The suite covers more account behavior than the current summary described.

Covered account behaviors include:

- avatar upload
- theme toggle
- two-factor toggle
- current-password confirmation for sensitive changes
- one-time recovery-key generation and replacement
- post-update persistence
- returning to login after logout
- viewing the 2FA challenge screen
- requesting password reset from login
- switching between Reset password and Account recovery without retaining fields or errors
- validating email and recovery-key formats before submission
- returning a generic result for validly formatted reset and recovery requests
- reading the reset email flow
- submitting an invalid reused password
- submitting a valid new password
- logging in again with the updated password

### User Management, License Visibility, and Subscription States

The user-management suite covers both admin and non-admin behavior.

Covered behaviors include:

- creating multiple users with different roles
- assigning licenses during creation
- logging in as those users
- verifying sidebar visibility based on assigned licenses
- verifying that some users see only indexed modules
- verifying that some users also see breach, social, exploit, feed, stealer-log, or scanner modules
- updating account preferences as a non-admin user
- triggering the stealer-logs subscription or paywall flow for a demo user
- showing a near-expiry trial banner for a member user
- deleting managed users until only protected system users remain

This means license-aware UI visibility and paywall/subscription behavior are part of the documented product behavior.

In practical terms, the tested product states include:

- users whose sidebar is limited to core indexed modules only
- users who gain additional breach, social, exploit, feed, stealer-log, or scanner visibility through license assignment
- users whose role grants scanner and entity-API access
- demo or limited users who are redirected into subscription/paywall flows instead of full module access
- expiring users who receive warning banners before access changes

### Tenant Provisioning and Tenant Operations

The tenant suite covers the full tenant lifecycle, including both admin-side and tenant-side workflows.

Covered provisioning and onboarding behaviors include:

- tenant signup
- email verification
- admin review of tenants
- tenant verification state changes
- enterprise-license assignment
- tenant onboarding wizard completion
- tenant IOC initialization during onboarding
- creating a tenant sub-user
- editing tenant user quota

Covered tenant-home behaviors include:

- tenant homepage navigation
- alert export
- notification sidebar opening
- opening alert details from notifications
- exporting alert reports from multiple alert contexts
- opening category alert cards
- creating a custom alert
- date filtering for tenant alerts
- flushing all alerts after confirmation through the `Flush All` workflow

The tenant-alert workflow therefore includes both content review and alert-maintenance controls, not only passive monitoring.

### Audit Logs and Administrative Operations

Administrative audit coverage includes:

- opening the audit-log page
- exporting audit records
- applying a date range that intentionally yields no rows
- resetting filters to return to populated records
- using the audit-log page in both tenant-management and standalone admin contexts

### System Settings and Error States

System Settings coverage includes both successful edits and validation failures.

Covered behaviors include:

- opening the system settings page
- entering edit mode
- changing the application name
- editing external URLs such as data sources, adversaries, and pricing
- saving the updated configuration
- attempting to upload an oversized authentication-dashboard icon
- showing the `File too large` validation error for files above `1 MB`

This should be documented explicitly because it is one of the tested administrative guardrails in the platform.

### Case Management: Full Tested Behaviors

The case-management suite covers the active investigation workflow from creation through evidence handling, status movement, closure, sharing, and archive review.

Covered behaviors include:

- opening Case Management from the profile area
- creating a case with core fields and a primary entity
- assigning an analyst when an eligible analyst is available
- opening the created case detail page
- filtering the case list by case ID or text
- filtering by status, severity, priority, and case type
- changing sort order
- switching between open and archived case lists
- switching between `Case List` and `Analytics`
- rendering the analytics panel
- editing case details
- editing the primary entity
- adding and editing related entities
- adding file artifacts
- uploading artifact files
- verifying artifact-file integrity
- showing verified file integrity state
- editing artifact metadata
- adding linked-report artifacts through report source and search controls
- adding raw-alert artifacts
- deleting artifact files
- adding and editing tasks
- adding and editing linked cases
- adding comments
- exporting a case PDF
- creating a public case share link
- revoking case share links
- moving a case through the tracking board with required reasons
- closing a resolved case
- enforcing read-only controls after closure
- archiving a closed case and reviewing it from the archived list

This means Case Management should be documented as a complete investigation workspace rather than only a form for saving case metadata.

### Chatbot and Report Conversation Flow

The tested chatbot and report conversation behaviors are listed in [DeepIntel](deepintel.md#chatbot-and-report-conversation-flow).

## Practical Workflows

### Workflow 1: Broad Investigation

1. Start in `Homepage` or `General Intelligence`.
2. Enter a keyword or topic.
3. Use `Advance` and sidebar filters to narrow the results.
4. Switch search mode if the results are too broad or too narrow.
5. Open a report for the most relevant record.
6. Review metadata and open CTI Graph if a relationship pivot is needed.

### Workflow 2: Identity Exposure Check

1. Open `Data Breach` or `Entity Lookup`.
2. Search for an email or identity value.
3. Review breach details or live lookup results.
4. Use Stealer Logs if deeper credential evidence is required.

### Workflow 3: Infrastructure Review

1. Open `Network Intel` or `Web Scans`.
2. Enter a domain or IP.
3. Run the appropriate recon or scan view.
4. For vulnerability scans, choose the target and scan depth that match the investigation need.
5. Review the report, severity, scanned URLs, request details, and evidence.
6. Export the report if it needs to be shared externally.

### Workflow 4: Profile Mapping

1. Open `Social Intel`.
2. Scan a username.
3. Review the graph or list view.
4. Open profile summaries and metadata popups.
5. Add custom entities or manage connections if needed.

### Workflow 5: Tenant Monitoring

1. Configure IOC values in `Profile > IOC`.
2. Review alert summaries from the tenant homepage.
3. Open category alert reports for the highest-risk items.
4. Export alerts when sharing findings with teammates.
5. Adjust `Profile > Alert Scanners` if future scans should include or exclude specific categories.
6. Create or link a case when an alert needs ownership, tasking, or closure tracking.

### Workflow 6: Actor Or Malware Investigation

1. Open `Actors & Malware`.
2. Search by actor name, alias, malware family, country, signature, reporter, or related keyword.
3. Switch between `APT`, `Malware`, and `Compromised-Actors` when the result type is unclear.
4. Open the actor or malware report.
5. Pivot to related indicators, infrastructure, reports, or graph views when relationship context is needed.

### Workflow 7: Geo-Fencing Review

1. Open `Geo Fencing` or `Satellite Intel`.
2. Search for a place or enter coordinates.
3. Review indexed facilities and nearby facility results.
4. Enable aircraft or ship tracking if transportation context matters.
5. Use `Imagery Analysis` for comparison or anomaly review.
6. Switch to `Threat Lens` when country-level threat records or map arcs are more relevant than facilities.

### Workflow 8: Long-Running Scan Review

1. Start the scan from `Entity Lookup`, `Web Scans`, `Network Intel`, or a geo camera workflow.
2. Leave the page only after the scan job has been created.
3. Use the notification or scan-job surface to reopen incomplete or completed scans.
4. Reuse recent completed results when the application offers them.
5. Choose a new scan only when a fresh result is required.

### Workflow 9: Case Handoff

1. Create a case from `Profile > Case-Management`.
2. Add the primary entity and the first set of artifacts.
3. Attach uploaded files, linked reports, or raw alerts as separate artifacts.
4. Verify uploaded artifact-file integrity before handoff where the action is available.
5. Assign analysts or tasks.
6. Use the case filters and analytics view to monitor open, stale, high-risk, or unassigned work.
7. Move the case through the tracking board as work progresses.
8. Close the case only after it reaches `resolved`.
9. Export a PDF or generate a share link for review, then revoke the share when review ends.

## Notes and Limitations

:::{admonition} Feature availability
:class: important

If a module described in this manual is not visible in your sidebar, the most common reasons are role restrictions, license restrictions, or deployment-level configuration toggles.
:::

:::{admonition} External modules
:class: note

Some sidebar items open new tabs or external services rather than rendering inside the main Orion workspace. `CTI Graph`, `Social Intel`, `Onion Link`, `Whistle Blowing`, and `Documentation` may behave this way depending on feature and deployment setup.
:::

:::{admonition} Recommended starting point
:class: tip

New users should begin with `Homepage`, `General Intelligence`, `Data Breach`, and `Stealer Logs` before moving into graph tools, tenant administration, or system administration.
:::
