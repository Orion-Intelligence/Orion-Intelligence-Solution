(help-manual)=

# Platform Help Manual

:::{admonition} Scope
:class: tip

This task-oriented manual provides concise help for the user-visible features of Orion Intelligence. Each feature includes navigation, a quick answer, operating steps, and troubleshooting guidance.

The intended audience includes application users, support personnel, analysts, tenant maintainers, and administrators. Backend details are included only when they materially affect user-visible behavior.
:::

Use this manual when you need to complete a specific task or resolve a common interface problem. For complete feature descriptions and broader workflows, see the [User Manual](./user_manual.md). For implementation and maintenance information, see the [Developer Documentation](./developer_documentation.md).

```{contents}
:local:
:depth: 2
```

## Login And 2FA

### Overview

Orion account access, login 2FA, email/password sign-in, authenticator code verification, demo login, verification-pending accounts, forced password reset redirects, progressive login delays, HTTP-only browser sessions, and authenticated session entry.

**Search terms:** login, sign in, open account, access Orion, 2FA login, verify 2FA, demo login.

### Navigation

`/login`.

### Quick answer

To sign in, open `/login`, enter email and password, select Sign In, then enter the 2FA code if the 2FA screen appears.

### Steps

  1. Open `/login`.
  2. Enter Email.
  3. Enter Password.
  4. Select Sign In.
  5. If 2FA appears, enter the 6-digit authenticator code.
  6. If demo access is visible, select the demo option.

### Troubleshooting

For verification-pending accounts, use Resend mail. If forced reset is active, the app redirects to the reset page. If 2FA fails, confirm the latest authenticator code and time sync. After five consecutive unsuccessful login attempts, wait for the delay shown on screen before trying again. Later consecutive failures increase the delay from 1 minute to 10 minutes and then 30 minutes. A successful login or 30 minutes without another failure resets the failure history.

## Signup And Email Verification

### Overview

Public registration, new tenant account creation, username entry, company email entry, password strength, welcome token links, resend verification mail, expired verification links, and first account activation.

**Search terms:** signup, register, create account, new tenant account, verification email, welcome token, resend verification.

### Navigation

`/signup`, `/welcome`, `/welcome/:token`.

### Quick answer

To create an account, open `/signup`, fill username, company email, and password, submit the form, then open the verification email and follow the welcome link.

### Steps

  1. Open `/signup`.
  2. Enter Username.
  3. Enter Company Mail.
  4. Enter Password and satisfy the strength meter.
  5. Select Sign Up.
  6. Open the verification email.
  7. Follow `/welcome/:token`.
  8. Return to login after verification.

### Troubleshooting

If the username already exists, use suggested alternatives. If verification mail is missing, use resend verification. If token is expired/invalid, request a new verification email.

## Password Reset, Account Recovery, And Forced Password Change

### Overview

Forgotten password recovery, global recovery keys, forced first-login password updates, reset email links, reset token pages, new password confirmation, old-password reuse validation, and return-to-login behavior.

**Search terms:** forgot password, reset password, recover account, recovery key, forced password reset, change password, reset token, update password.

### Navigation

`/reset`, `/reset/:token`.

### Quick answer

Select `Recover account?` on the login screen. Use `Reset password` to request a link by email, or use `Account recovery` with the registered email and 43-character global recovery key. Open the email link, enter and confirm a new password, then log in again.

### Steps

  1. Select `Recover account?` on `/login`, or open `/reset`.
  2. Leave `Reset password` selected and enter the registered email, or select `Account recovery` and enter both the email and global recovery key.
  3. Submit the form and open the reset email if the supplied account details are valid.
  4. Open `/reset/:token` within 20 minutes.
  5. Enter New Password.
  6. Enter Confirm Password.
  7. Submit the reset form.
  8. Return to `/login`. A forced password change returns the signed-in user to the application instead.

### Troubleshooting

Email and recovery-key format errors are shown before submission. For correctly formatted input, the same success screen appears even if the account or recovery details are wrong; this is intentional and prevents account discovery. If no email arrives, check the details and spam folder, then retry. Reset links expire after 20 minutes. The new password cannot match the old one. A newly generated recovery key replaces the old key.

## Tenant Onboarding

### Overview

First-time tenant setup, company information, initial monitored IOC seeding, onboarding completion, dashboard access gating, tenant verification, license assignment, and setup-loop troubleshooting.

**Search terms:** onboarding, first setup, tenant setup, company setup, add IOCs during setup.

### Navigation

`/onboarding`.

### Quick answer

Complete onboarding by entering company information, adding optional monitored IOCs, confirming the setup, and continuing to the dashboard.

### Steps

  1. Open `/onboarding` after login.
  2. Enter company or tenant information.
  3. Continue to IOC setup.
  4. Choose IOC categories.
  5. Add IOC values.
  6. Review confirmation.
  7. Complete onboarding.
  8. Continue to dashboard.

### Troubleshooting

If onboarding loops or blocks dashboard access, confirm tenant verification, account status, license assignment, and required setup fields.

## Dashboard Shell And Sidebar

### Overview

Authenticated app navigation, left sidebar modules, collapsed/mobile menu, profile menu, notification bell, default dashboard redirects, role-based visibility, license-based menu access, and dashboard layout behavior.

**Search terms:** dashboard, sidebar, menu, navigation, missing menu item, collapsed sidebar, dashboard layout.

### Navigation

`/dashboard`.

### Quick answer

Use the left sidebar to open modules; if a feature is missing, check role, license, onboarding, tenant state, system settings, and whether the sidebar is collapsed.

### Steps

  1. Sign in.
  2. Open `/dashboard`.
  3. Expand the sidebar if collapsed.
  4. Select a module group.
  5. Open the desired module.
  6. Use the profile menu for account/admin pages.
  7. Use notifications for alerts and payment/status screens.

### Troubleshooting

Missing menu items are usually caused by role/license restrictions, disabled tenant, incomplete onboarding, collapsed/mobile sidebar, or system feature toggles.

## Global Search

### Overview

Shared free-text search across indexed intelligence modules, query submission, loading state, result counts, result cards, result rows, pagination, no-result states, broad result sets, and report opening.

**Search terms:** search, basic search, global search, query, find result, result count, no result, search old data.

### Navigation

dashboard search, `/dashboard/profile/consolidated/all`, or any indexed module.

### Quick answer

Enter a query in the search input, submit it, then inspect the returned result cards or rows.

### Steps

  1. Open the target module.
  2. Enter a query in the search input.
  3. Select the search icon or press Enter where supported.
  4. Wait for loading to finish.
  5. Review result count and cards/rows.
  6. Open a result to inspect the report.
  7. Use pagination or Load More if available.

### Troubleshooting

If results are too broad, use Advanced entity filters, Tools full-query/AND mode, and Date Range. If results appear before a query, the screen may be showing default discovery records.

## Advanced Entity Filters

### Overview

Structured indicator filtering, entity category selection, filter value chips, exact match, partial match, suggestions, selected-filter counts, email filters, domain filters, IP filters, URL filters, CVE filters, phone filters, credit card filters, crypto filters, person filters, organization filters, and social-profile filters.

**Search terms:** advanced filter, entity filter, exact email search, filter by domain, filter by IP, CVE filter, selected filters.

### Navigation

search screen > Advanced.

### Quick answer

Open Advanced, choose the entity category, enter the value, add it, select Exact Match or Partial Match, and run the search.

### Steps

  1. Open a search module.
  2. Select Advanced.
  3. Search or select an entity category.
  4. Enter the entity value.
  5. Select plus or press Enter.
  6. Choose Exact Match for precise values or Partial Match for broader values.
  7. Run the search.
  8. Remove chips or Clear Selection when needed.

### Troubleshooting

Invalid values can fail validation or return no results. Exact Match can miss differently formatted data; Partial Match can return broader results.

## Side Filter Drawer And Date Range

### Overview

Slide-out module filters, start date, end date, creation date, event date, network type, safe search, content type, source, status, index type, platform, active filter chips, old records, random-looking records, and time-bounded result narrowing.

**Search terms:** date range, old results, older data, random results, filter drawer, side filter, reset filters, no query results.

### Navigation

module screen > Filter.

### Quick answer

Open Filter, select start date and end date, select Apply, then confirm the date chip or selected filter count is visible.

### Steps

  1. Open the module.
  2. Select Filter.
  3. Choose needed dropdown filters.
  4. Open the date picker.
  5. Select start date.
  6. Select end date.
  7. Select Apply.
  8. Use Reset to clear drawer filters.

### Troubleshooting

Older data appears when no date range is active or the range is wide. Results can look random before a query because discovery/default records are loaded. Select both dates before applying.

## Search Tools And Sorting

### Overview

Query matching modes, semantic search, any-term OR matching, individual-term AND matching, full-query AND matching, strict search, broad search, exact phrase-style search, newest-first sorting, and oldest-first sorting.

**Search terms:** semantic search, exact search, AND search, OR search, full query, newest first, oldest first, search mode.

### Navigation

search screen > Tools.

### Quick answer

Open Tools and choose semantic, OR, AND, full query, newest first, or oldest first depending on the search goal.

### Steps

  1. Open a search module.
  2. Enter a query.
  3. Open Tools.
  4. Choose the matching mode.
  5. Choose sort order if available.
  6. Run the search again.

### Troubleshooting

Semantic can return related low-precision results. Use full query or AND with entity filters for strict matching.

## Report Views, Evidence, Feedback, And Comments

### Overview

Opened result reports, evidence pages, metadata review, screenshots, extracted entities, raw JSON, comments, recommend feedback, trust feedback, untrust feedback, copy actions, export actions, STIX output, downloads, and report chat controls.

**Search terms:** open report, inspect result, report details, feedback, comment, trust, untrust, recommend, export report, raw JSON.

### Navigation

click result card/row or route such as `/dashboard/breach/:category/:m_hash`.

### Quick answer

Open a result, review report evidence and metadata, then use feedback, comments, copy, export, or AI controls where available.

### Steps

  1. Run a search.
  2. Select a result card or row.
  3. Review title, content, source/reference, date, and extracted entities.
  4. Open raw JSON or screenshots if available.
  5. Use copy controls for indicators.
  6. Add Recommended, Trust, or Untrust feedback if needed.
  7. Add a comment if allowed.
  8. Export or download report where available.

### Troubleshooting

Some report types open full pages while others open modals/overlays. Commenting can be rate-limited. Profile visibility depends on user and tenant privacy settings.

## Homepage, Insights, Statistics, And Heatmap

### Overview

Dashboard overview content, homepage search, insight cards, latest documents, global summaries, tenant alert cards, world heatmap, country report panels, profile statistics, metrics, trend summaries, and role-specific homepage widgets.

**Search terms:** homepage, dashboard overview, statistics, heatmap, country report, insight cards, latest documents.

### Navigation

`/dashboard/home`, `/dashboard/profile/homepage`, `/dashboard/profile/statistics`.

### Quick answer

Use Homepage for high-level search, alert/insight overview, heatmap exploration, and quick pivots into results.

### Steps

  1. Open Dashboard or Profile > Homepage.
  2. Review summaries, insight cards, or tenant alert cards.
  3. Use the homepage search if needed.
  4. Hover or select a country on the heatmap where available.
  5. Open country/report panel.
  6. Close the panel or pivot into a module.
  7. Open Profile > Statistics for metrics.

### Troubleshooting

Homepage content depends on role, license, tenant type, and default/global tenant membership.

## General Intelligence

### Overview

Broad mixed-source intelligence discovery across open web, dark web, forums, news, stolen data references, drugs, hacking, marketplaces, cryptocurrency, leaks, topic keywords, organizations, products, actors, events, network filters, safe search, and content-type filters.

**Search terms:** general intelligence, broad search, forums, news, stolen, hacking, marketplaces, cryptocurrency, leaks.

### Navigation

sidebar > General Intelligence or `/dashboard/strategic/all`.

### Quick answer

Open General Intelligence, choose a category such as All or Forums, search the query, then filter and open reports.

### Steps

  1. Open General Intelligence.
  2. Choose All or a category.
  3. Enter query.
  4. Apply Advanced filters if needed.
  5. Apply Date Range or Network Type.
  6. Open a result.
  7. Review/export report.

### Troubleshooting

Exact values work best with entity filters and the narrowest matching module for the indicator type.

## Data Breach

### Overview

Stored breach datasets, database leaks, tracking records, identity exposure, leaked account references, email exposure, company exposure, person exposure, breach metadata, database categories, tracking categories, recent-only filters, and exact entity matching.

**Search terms:** data breach, breach records, databases, tracking, leaked email, exposed account, breach exposure.

### Navigation

sidebar > Data Breach or `/dashboard/breach/all`.

### Quick answer

Open Data Breach, choose All/Databases/Tracking, search the email or entity, apply filters, then open the breach report.

### Steps

  1. Open Data Breach.
  2. Choose All, Databases, or Tracking.
  3. Enter email, domain, person, company, or keyword.
  4. Add Email/Domain entity filter for exact searching.
  5. Apply Date Range if needed.
  6. Open a result.
  7. Review breach evidence.

### Troubleshooting

If the user specifically needs credential rows, use Stealer Logs or Consolidated IOCs. If no result appears, broaden filters or use Email Breach live lookup.

## Marketplace

### Overview

Structured listings collected from underground marketplaces: listing title and description, market name, vendor, category, price and currency, status, first and last seen dates, network, and supporting evidence. Separate from the General Intelligence `Marketplaces` filter, which returns crawled marketplace pages.

**Search terms:** marketplace, dark web market, listings, vendor, price, onion market, marketplace listing.

### Navigation

Sidebar > `Marketplace` (`/dashboard/marketplace/all`), listed below Data Breach.

### Quick answer

Open `Marketplace`, search for a product, brand, or entity, narrow the results with the filters, and select `Open Report` to view a listing.

### Steps

  1. Open `Marketplace` from the sidebar.
  2. Enter a keyword in the global search box.
  3. Use `Network Type` and `Creation Date Range` to narrow the results.
  4. Select `Open Report` to open a listing, or `Open CTI Graph` to pivot.

### Troubleshooting

- Entry missing or "No license for module: breach": Marketplace follows the Data Breach module license.
- Few results: without a date range, Orion searches roughly the last 150 days.

Full instructions are in [Marketplace](user_manual/orion_core.md#marketplace).

## Compromise Monitoring

### Overview

Website compromise incidents, hacked pages, phishing sites, defacer names, attacker teams, compromised databases, target URLs, domains, server evidence, IP evidence, location evidence, saved dates, and defacement reports.

**Search terms:** defacement, hacked site, phishing site, compromised website, defacer, hacked databases.

### Navigation

sidebar > Compromise Monitoring or `/dashboard/defacement/all`.

### Quick answer

Open Compromise Monitoring, choose All/Hacked/Phishing/Databases, search the target or attacker, and open the incident report.

### Steps

  1. Open Compromise Monitoring.
  2. Choose All, Hacked, Phishing, or Databases.
  3. Enter target URL, domain, defacer, team, IP, or keyword.
  4. Apply Date Range if needed.
  5. Open a matching row/card.
  6. Review target, attacker, date, server, IP, and evidence.
  7. Export or copy indicators where available.

### Troubleshooting

If a report opens as an overlay or JSON-like view, close it to return to the result list. Date range affects the result list and opened report context.

## Social Indexed Search

### Overview

Stored social posts, collected discussion records, Telegram content, Twitter content, Mastodon content, Pastebin content, forum content, Reddit content, chatter monitoring, social leak references, platform tabs, social reports, and chat reports.

**Search terms:** social search, Telegram search, Twitter search, Reddit search, forum search, Pastebin search, social posts.

### Navigation

sidebar > Social or `/dashboard/social/all`.

### Quick answer

Open Social, choose a platform tab, search the query, apply filters, then open the social/chat report.

### Steps

  1. Open Social.
  2. Choose All or a platform.
  3. Enter query.
  4. Apply platform, network, content type, or date filters.
  5. Open a social/chat result.
  6. Review message/post context and metadata.
  7. Export or open related social/chat report actions where available.

### Troubleshooting

Social search retrieves stored platform content and report records.

## Exploit

### Overview

Vulnerability intelligence, exploit intelligence, CVE identifiers, CWE weakness context, exploit tools, zero-day references, vulnerable products, proof-of-concept discussion, exploit names, exploit reports, and vulnerability report review.

**Search terms:** exploit, CVE, tools, zeroday, zero day, vulnerability, exploit report.

### Navigation

sidebar > Exploit or `/dashboard/exploit/all`.

### Quick answer

Open Exploit, choose All/CVE/Tools/ZeroDay, search the vulnerability or tool, then inspect the exploit report.

### Steps

  1. Open Exploit.
  2. Choose All, CVE, Tools, or ZeroDay.
  3. Enter CVE, product, tool, or keyword.
  4. Apply entity filters for CVE/CWE if needed.
  5. Apply Date Range if needed.
  6. Open result.
  7. Review exploit context and indicators.

### Troubleshooting

If a CVE search returns broad semantic results, use exact CVE entity filter or full-query matching.

## Feed

### Overview

News-style intelligence reading, current reporting, feed search, news search, recent intelligence articles, feed reports, source URLs, report metadata, current coverage, and reading-oriented workflows.

**Search terms:** feed, news, current reporting, intelligence news, feed report.

### Navigation

sidebar > Feed > News or `/dashboard/feed/news`.

### Quick answer

Open Feed > News, search or browse current items, then open the feed report.

### Steps

  1. Open Feed.
  2. Select News.
  3. Enter query or browse results.
  4. Apply filters if needed.
  5. Open a report.
  6. Review structured detail and raw data where available.

### Troubleshooting

If feed is too broad, use date range and content filters.

## Actors And Malware

### Overview

APT intelligence, malware-family reports, compromised actor records, actor names, campaigns, malware families, TTP context, victim and region metadata, actor detail reports, malware detail reports, APT filters, malware filters, country filters, source filters, tag filters, and investigation pivots from actor or malware result cards.

**Search terms:** actors and malware, APT intel, threat actor, malware family, actor report, malware report, compromised actors, campaign intelligence.

### Navigation

sidebar > Actors & Malware or `/dashboard/apt-intel/all`.

### Quick answer

Open Actors & Malware, search the actor, campaign, malware family, or keyword, choose APT/Malware/Compromised-Actors when needed, then open the matching report.

### Steps

  1. Open Actors & Malware.
  2. Choose All, APT, Malware, or Compromised-Actors.
  3. Enter actor, malware, campaign, sector, country, or keyword.
  4. Apply filters such as country, source, family, attacker, team, platform, tags, or date range.
  5. Run the search.
  6. Open an actor or malware report.
  7. Review entities, metadata, evidence, and export options.

### Troubleshooting

Actor and malware names can have aliases. Broaden the query or switch between APT, Malware, and Compromised-Actors when a name does not appear in the expected tab.

## Stealer Logs

### Overview

Infostealer-derived credential records, stolen password rows, credential logs, domain searches, username searches, IP address searches, channel searches, file name searches, email searches, credit card searches, advanced WHERE builder rows, AND/OR conditions, expandable assets, recent log filters, and stealer exports.

**Search terms:** stealer logs, stealerlog, stolen credentials, infostealer, leaked password, credential logs, stealer IOC.

### Navigation

sidebar > Stealer Logs > IOCs or `/dashboard/stealerlogs/iocs`.

### Quick answer

Open Stealer Logs > IOCs, select a tag or advanced filters, enter the value, execute search, expand rows, and apply Date Range for recent-only results.

### Steps

  1. Open Stealer Logs > IOCs.
  2. Choose basic tag such as Email, Domain, Username, IP Address, File Name, Channel, or Credit Card.
  3. Enter the value.
  4. Execute search.
  5. Expand rows for detailed credential/log fields.
  6. Apply Date Range when needed.
  7. Use export/download if available.
  8. Use advanced builder for multiple conditions.

### Troubleshooting

Older logs appear when Date Range is inactive. Stealer Logs is subscription/license gated. Full credential-log review supports stricter filtering than preview surfaces.

## Consolidated Workspace

### Overview

Cross-module triage, IOC rows, Deep Search, Network Intelligence pivots, grouped result sections, Social groups, Tracking groups, News groups, Leak groups, Stealers groups, Threats groups, insight panels, scanner shortcuts, expandable IOC rows, and broad query comparison.

**Search terms:** consolidated, deep search, IOC tab, network intelligence tab, cross-module search, grouped results, threats table.

### Navigation

`/dashboard/consolidated/all` or `/dashboard/profile/consolidated/all`.

### Quick answer

Open Consolidated, choose IOCs/Deep Search/Network Intelligence, enter the query, then inspect grouped sections or expandable IOC rows.

### Steps

  1. Open Consolidated.
  2. Choose IOCs, Deep Search, or Network Intelligence.
  3. Enter query.
  4. Apply filters or search tools.
  5. Review grouped result sections.
  6. Expand IOC rows in Stealers or Threats.
  7. Open reports or run scanner pivots.
  8. Export IOC rows where available.

### Troubleshooting

One section can have results while another has none. Date filters can remove IOC rows. Invalid email/tag values can trigger validation or empty states.

## Directory

### Overview

Monitored source catalogs, crawled links, source URLs, onion sources, clearnet sources, source coverage lists, network layer, index type, content type, last update metadata, source pagination, source filters, and directory browsing.

**Search terms:** directory, links, monitored sources, source list, onion sources, clearnet sources, index type.

### Navigation

sidebar > Directory or `/dashboard/directory`.

### Quick answer

Open Directory, apply network/index/content/date filters, and review monitored source records.

### Steps

  1. Open Directory.
  2. Search or browse sources.
  3. Open Filter.
  4. Choose Network Type.
  5. Choose Index Type.
  6. Choose Content Type.
  7. Apply Date Range if needed.
  8. Use pagination/load more.

### Troubleshooting

Directory searches source metadata catalogs. Indexed intelligence modules search collected content.

## Entity API - Email Breach

### Overview

Live email exposure lookup, username exposure lookup, targeted breach lookup, one-value search, structured API output, exposure status, breach fields, downloadable lookup reports, and Search APIs email-breach results.

**Search terms:** email breach, email exposure, user email lookup, check email breach, breached email.

### Navigation

`/dashboard/api/email-breach`.

### Quick answer

Open Search APIs > Email Breach, enter username/email, select Search, then review exposure results.

### Steps

  1. Open Email Breach.
  2. Enter Username or Email.
  3. Select Search.
  4. Wait for success/loading to finish.
  5. Expand result rows/cards where available.
  6. Download report if needed.

### Troubleshooting

If Search is disabled, the input is missing or invalid. If success has little data, the source had limited matching exposure.

## Entity API - Social Scanner

### Overview

Live username lookup, handle search, account identifier search, social platform presence, public profile footprints, platform matches, social profile discovery, and quick social enrichment.

**Search terms:** social scanner, username lookup, check username, social profile lookup, platform presence.

### Navigation

`/dashboard/api/social-scanner`.

### Quick answer

Open Social Scanner, enter a username, search, and review platform/profile results.

### Steps

  1. Open Search APIs > Social Scanner.
  2. Enter Username.
  3. Select Search.
  4. Wait for result.
  5. Review returned platforms/profiles.
  6. Copy or pivot values into Social Intel if needed.

### Troubleshooting

Social Scanner is a direct platform-presence lookup. Relationship mapping, profile expansion, and leak pivots are handled in the dedicated social relationship workspace.

## Entity API - Wanted List

### Overview

Live wanted-person lookup, watchlist records, sanctions-style records, law-enforcement-style records, person name search, alias search, identity risk checks, and structured wanted-list API results.

**Search terms:** wanted list, wanted person, watchlist, sanctions, person alias, search person.

### Navigation

`/dashboard/api/wanted-list`.

### Quick answer

Open Wanted List, enter the person name or alias, search, and review matching wanted/watchlist records.

### Steps

  1. Open Wanted List.
  2. Enter Person Name or Alias.
  3. Select Search.
  4. Wait for result.
  5. Expand or inspect records.
  6. Download report if available.

### Troubleshooting

If no records appear, broaden the name/alias or verify spelling.

## Entity API - National Identity

### Overview

Licensed identity lookup, CNIC-style values, mobile number lookup, phone identity records, family-number style values, national identity datasets, sensitive identity results, and structured national-identity API output.

**Search terms:** national identity, CNIC, mobile number, phone identity, identity lookup.

### Navigation

`/dashboard/api/national-identity`.

### Quick answer

Open National Identity, enter CNIC or mobile number, search, and review identity records.

### Steps

  1. Open National Identity.
  2. Enter CNIC or Mobile Number.
  3. Select Search.
  4. Wait for result.
  5. Review returned fields.
  6. Download report if available.

### Troubleshooting

Invalid format can disable Search or return no result. Visibility depends on license and deployment.

## Entity API - Playstore Scanner

### Overview

Google Play URL lookup, Android package name lookup, Play Store package search, cracked app references, redistributed mobile apps, suspicious Play Store artifacts, package intelligence, and app exposure results.

**Search terms:** Playstore scanner, app scanner, package lookup, Play Store URL, cracked key app.

### Navigation

`/dashboard/api/playstore-scanner`.

### Quick answer

Open Playstore Scanner, enter package name or Play Store URL, search, and review app intelligence.

### Steps

  1. Open Playstore Scanner.
  2. Enter Package or Playstore URL.
  3. Select Search.
  4. Wait for result.
  5. Review fields and indicators.
  6. Download report if available.

### Troubleshooting

Play Store package/URL lookup works best with package identifiers and store links.

## Entity API - Software Scanner

### Overview

Software-title lookup, game-title lookup, cracked mirrors, pirated packages, suspicious software references, unofficial downloads, configured-source exposure, software exposure results, and structured software scanner output.

**Search terms:** software scanner, cracked software, software lookup, suspicious software, software exposure.

### Navigation

`/dashboard/api/software-scanner`.

### Quick answer

Open Software Scanner, enter software name, search, and review matching software intelligence.

### Steps

  1. Open Software Scanner.
  2. Enter Software Name.
  3. Select Search.
  4. Wait for result.
  5. Review returned fields.
  6. Download report if available.

### Troubleshooting

Broaden the software name or try alternate product names if no result appears.

## Entity API - Crypto Scanner

### Overview

Crypto wallet lookup, blockchain artifact lookup, wallet address search, transaction hash search, crypto risk context, balances, received totals, sent totals, transaction counts, TXID confirmations, block height, fees, inputs, outputs, wallet pivots, and transaction pivots.

**Search terms:** crypto scanner, wallet address, transaction hash, crypto risk, blockchain lookup, TXID.

### Navigation

`/dashboard/api/crypto-scanner`.

### Quick answer

Open Crypto Analysis, enter wallet address or transaction hash, search, and review address/transaction risk details.

### Steps

  1. Open Crypto Scanner.
  2. Enter wallet address or transaction hash.
  3. Select Search.
  4. Wait for result.
  5. Review balance, transactions, risk, or TX details.
  6. Copy or export values where available.

### Troubleshooting

Invalid address/hash format can return no result. Use Social Intel Wallet/Crypto entity if the value must appear on a graph.

## File Analysis And IOC Extraction

### Overview

File uploads, choose-file workflows, 30 MB file limits, IOC extraction, indicator grouping, file metadata, extracted fields, processing states, no-finding states, failure states, copying extracted values, and file-analysis downloads.

**Search terms:** file analysis, file scanner, upload file, IOC extraction, extract IOCs, file report.

### Navigation

`/dashboard/api/file-scanner`.

### Quick answer

Open File Analysis, choose a file, wait for processing, review extracted results, and download the report if needed.

### Steps

  1. Open File Analysis.
  2. Select Choose File.
  3. Upload the file.
  4. Wait for processing.
  5. Review extracted fields and indicators.
  6. Copy values as needed.
  7. Download report or upload another file.

### Troubleshooting

If processing fails, use Try Again and verify file type/size. Empty findings can indicate limited extractable IOC content.

## Text Analysis

### Overview

Pasted text inspection, email body review, chat message review, short snippet analysis, URL list analysis, spam detection, phishing detection, malicious URL checks, unsafe indicators, URL reputation rows, confidence values, and final text verdicts.

**Search terms:** text analysis, analyze text, phishing text, spam text, malicious URL, URL reputation.

### Navigation

`/dashboard/api/text-analysis`.

### Quick answer

Open Text Analysis, paste the text, select Analyze Text, and review spam verdict and URL reputation.

### Steps

  1. Open Text Analysis.
  2. Paste text into the textarea.
  3. Keep within 5000 characters.
  4. Select Analyze Text.
  5. Review verdict, confidence, URLs, unsafe URLs, and rows.
  6. Copy or use results for investigation.

### Troubleshooting

If Analyze is disabled, text is missing or invalid. Long content should be shortened or uploaded as a file if supported.

## Web Scans And APK Analysis

### Overview

Network Scan, Repository Scan, SEO Scan, APK Scan upload, APK file upload, scan report generation, security posture grades, findings, severities, confidence values, proof blocks, code blocks, target URL input, domain input, repository input, Android package uploads, print actions, and report downloads.

**Search terms:** web scan, network scan, repository scan, SEO scan, APK scan, security posture, scan report.

### Navigation

`/dashboard/scanner/network-scan`, `/dashboard/scanner/repository-scan`, `/dashboard/scanner/seo-scan`, `/dashboard/scanner/apk-scan`.

### Quick answer

Open the scan type, enter or upload the target, run the scan, wait for completion, then review and export the report.

### Steps

  1. Open Web Scans.
  2. Choose Network Scan, Repository Scan, SEO Scan, or APK Scan.
  3. Enter domain, URL, repository, IP-like target, or upload APK.
  4. Select Search/Scan.
  5. Wait for processing.
  6. Review security posture and findings.
  7. Download or print report.

### Troubleshooting

If scan fails, verify target format, reachability, license access, and network availability. Wait for completion before exporting.

## Network Intel

### Overview

Live infrastructure reconnaissance, Host Recon, IP Scan, Vulnerability Scan, scan-depth controls, vulnerability target selection, completed vulnerability result cards, severity summaries, scanned URL metadata, request evidence, Geo Fencing, Geo IoT, domain-to-IP resolution, host details, exposed-device discovery, geographic IP range scanning, scan status, expandable DNS/IP/vulnerability rows, geo radius input, and network report downloads.

**Search terms:** network intel, NETINT, host recon, IP scan, vulnerability scan, scan depth, vulnerability depth, geo fencing, geo IoT, resolve IP.

### Navigation

`/dashboard/netint` or `/dashboard/scanner/network-scan`.

### Quick answer

Open Network Intel, choose the tab, enter the required target, run the scan, expand results, and download the report when available.

### Steps

  1. Open Network Intel.
  2. Choose Host Recon, IP Scan, Vulnerability Scan, or Geo Fencing.
  3. Enter host/domain/IP/URL or open Geo modal.
  4. For Vulnerability Scan, choose the visible scan depth before opening the target result.
  5. For Geo manual mode, enter coordinates, radius, and max IPs.
  6. Start the scan.
  7. Expand DNS/IP/vulnerability rows or open the vulnerability target.
  8. Download report after completion.
  9. Cancel while scanning if needed.

### Troubleshooting

If a domain resolves to multiple IPs, inspect each row. If download is disabled, run a scan first or wait for completion. Vulnerability detail is available only after the target scan completes.

## CTI Graph

### Overview

Threat-intelligence relationship visualization, clusters, documents, properties, group nodes, edges, graph sessions, CTI filters, advanced graph builder, graph view, list view, physics toggle, node search, context menus, maximum nodes, maximum depth, export modal, current-session export, CTI report export, and relationship exploration.

**Search terms:** CTI graph, threat graph, cluster graph, document graph, property graph, advanced graph builder, graph export, graph sessions.

### Navigation

`/dashboard/ctigraph`.

### Quick answer

Open CTI Graph, set filters, apply them, inspect graph/list view, and export session or CTI report if needed.

### Steps

  1. Open CTI Graph.
  2. Create or select a session.
  3. Choose Type: cluster, document, or property.
  4. Fill cluster type, document ID, or property value where required.
  5. Set maximum nodes and depth.
  6. Select Apply.
  7. Use graph search, graph/list toggle, physics toggle, context menu, or advanced builder controls.
  8. Open the export modal and export Current Session or CTI Report.

### Troubleshooting

If graph is empty, adjust filters or use a more specific document/property. If graph is slow, reduce node/depth limits.

## Social Intel Overview

### Overview

Profile Intelligence is the Social Intelligence profile-search workspace: username scans, image-based discovery, saved profile details, live enrichment, scan history, relationship graphs, and report exports. Social Monitoring is the separate Manage Profiles page for browser sessions, personas, connected accounts, and monitored activity.

**Search terms:** Social Intelligence, Profile Intelligence, profile search, social mapper, social graph, username scan, image search, profile mapping, graph/list view.

### Navigation

`/dashboard/social-intel` or `/dashboard/social-graph`; `/dashboard/social-mapper` redirects to Social Intel.

### Quick answer

Open Social Intel, scan a username or upload an image, manage discovered profiles, add profiles/entities to graph, and inspect relationships in graph or list view.

### Steps

  1. Open Social Intel.
  2. Enter username and select Scan, or upload an image.
  3. Watch Scan History for status.
  4. Open Manage Profiles if candidates appear.
  5. Select profiles and update graph.
  6. Switch graph/list view as needed.
  7. Open profile summary or metadata popups.
  8. Export report/session when needed.

### Troubleshooting

Graph search filters loaded graph content. If image upload returns empty candidates, try a clearer profile image or username scan. If live fetching fails, review any saved Details, Online Presence, or Stealer Logs that remain visible. For **Login required**, open Login, finish signing in to the platform in the same browser, return, and select Reload. An empty profile with no saved data still needs a successful fetch. See [Profile Search](profile-search-profile-intelligence) for the full workflow.

## Social Intel Entities

### Overview

Custom graph nodes, manual entities, API-mode entities, entity lookup pivots, email nodes, domain nodes, wallet nodes, phone nodes, breach nodes, social scanner nodes, wanted-list nodes, national-identity nodes, software/app nodes, DNS nodes, Wayback nodes, IOC nodes, APK nodes, crypto nodes, entity cards, progress states, On Graph status, delete controls, and graph focus actions.

**Search terms:** Social Intel entities, custom entity, add entity, manual entity, API entity, graph node, active entities.

### Navigation

Social Intel > Entity Manager / Add Entity.

### Quick answer

Open Add Entity, choose API or Manual mode, choose entity type, enter value/query and optional label, submit, then add or focus the entity on the graph.

### Steps

  1. Open Social Intel.
  2. Open Entity Manager or Add Entity.
  3. Choose API mode or Manual mode.
  4. Select entity type.
  5. Enter value/API query.
  6. Add optional label.
  7. Submit.
  8. Click saved entity to add/focus it on graph.

### Troubleshooting

Manual entities usually have no report data. API entities can fail if input format, license, or provider/source availability is invalid.

## Social Intel Profile Summary And Stealer-Log Pivot

### Overview

Profile summary panels, loaded-platform profile details, posts, images, metadata, profile connections, Fetch Profile Leaks, profile leak intelligence, Email Breach Results, Stealer Log Results preview, copied profile values, and profile-based leak pivots.

**Search terms:** see stealerlog in Social Intel, profile leaks, Fetch Profile Leaks, profile summary, Email Breach Results, Stealer Log Results.

### Navigation

Social Intel > profile summary popup > All Platforms Summary > Fetch Profile Leaks.

### Quick answer

Scan or load a profile, open profile summary, select Fetch Profile Leaks, then review Profile Leak Intelligence for Email Breach Results and Stealer Log Results.

### Steps

  1. Open Social Intel.
  2. Scan or load a profile with username/email.
  3. Open profile summary from graph or list view.
  4. In All Platforms Summary, select Fetch Profile Leaks.
  5. Review Email Breach Results.
  6. Review Stealer Log Results.
  7. Copy values if needed.
  8. Open Stealer Logs > IOCs for full search/date filtering.

### Troubleshooting

Fetch Profile Leaks cannot run without a username/email. If no response appears, the pivot ran but no leak/stealer data matched.

## Social Intel Followers, Following, Relationships, And Context Menu

### Overview

Social followers graph, following graph, account relationship expansion, follower fetches, following fetches, posts, images, metadata, follower popups, following tabs, connection tabs, related account scans, relationship nodes, relationship details, aliases, clearing connections, delete profile actions, remove node actions, and right-click graph context menus.

**Search terms:** followers, following, social relationships, relationship popup, set alias, delete profile, clear connections, context menu.

### Navigation

Social Intel > profile summary/list row/graph node/context menu.

### Quick answer

Open a profile or relationship node, fetch followers/following/posts/images/metadata as needed, then inspect or add related accounts to graph.

### Steps

  1. Open Social Intel graph or list view.
  2. Open profile summary or follower popup.
  3. Select Followers or Following.
  4. Fetch data when needed.
  5. Filter long lists.
  6. Select accounts to scan.
  7. Add resulting accounts to graph.
  8. Right-click nodes for alias/delete/relationship actions.

### Troubleshooting

Buttons can be disabled during active fetches, after completed data loads, or on platforms with limited relationship actions.

## Social Monitoring and Browser Sessions

### Overview

Social Monitoring opens **Manage Profiles**, with **Sessions**, **Monitoring**, and **Results** tabs. **Monitoring** holds personas and monitors, grouped as **Private Monitors** and **External Monitors**. It is separate from selecting discovered candidates in the profile-search popup.

**Search terms:** Social Monitoring, Manage Profiles, capture session, upload session, verify session, expired session, Add a Monitor, persona, external account, posting, ad monitoring, hate speech, keywords monitoring, cron job, stop daily run.

### Navigation

Social Intelligence > Social Monitoring (`/dashboard/manage-profiles`).

### Steps

1. Install or update the browser extension if prompted, and sign in to the intended social platform account in the same browser.
2. In **Sessions**, use **Fetch session** (or **Upload** a saved `.zip`/`.json` session file), then expand the platform and verify the session. Use **Edit** to recapture an expired session. The limit is 10 sessions per platform.
3. In **Monitoring**, select **Add a Monitor**. Choose the **Account**: **Persona** (your persona's own account) or **External Account** (someone else's profile by URL).
4. Choose the **Activity Type**: **Posting**, **Ad Monitoring**, **Hate Speech Monitoring**, or **Keywords Monitoring** (external accounts support only the last two). The drawer opens with **Ad Monitoring** selected.
5. Select the platform and a session, enter a **Label**, and for external accounts the **Profile URL**. For keyword monitoring add at least one target keyword. Turn on **Automated Cron Job** to include the monitor in daily runs.
6. For a **Persona** account, fill in the persona name, age group, gender, and up to three interests, then save. To create only a persona, choose **Posting** and save without a platform.
7. Use the monitor's **Post**, **Ad**, or **Scan** control and follow the activity in **Results**. **Post publishes externally**; it is not a profile-data fetch.
8. In **Results**, filter by Account and choose **Ads Monitoring**, **Posts**, **Hate Speech Monitoring**, or **Keywords Monitoring** under Show. Expand rows to inspect findings; keyword rows show matched keywords and can show the other posts.
9. **Stop** cancels a manual run. **Stop daily run** also stops the remaining scheduled batch; completed work is not undone.

### Troubleshooting

- Tabs unavailable: check that the extension is ready and its platforms loaded. Only platforms that support automation are listed; X appears as `x`.
- No session in the drawer: capture or upload a session for that platform in **Sessions**. Sessions used by another monitor with the same activity are marked as already in use.
- **No session attached** on a monitor: use **Attach session**; it needs a verified, unused session for the platform.
- **No persona assigned**: posting monitors need a persona. A persona can be assigned to only one monitor.
- Save rejected: the drawer names the first missing item, such as age group, label, profile URL, session, or keyword.
- Own-account ad, hate-speech, and keyword monitors open read-only (eye icon); delete and recreate them to change settings.
- Monitor skipped in the daily run: turn on **Automated Cron Job** and make sure a readable session (and, for posting, a persona) is attached.
- Session Expired: sign in again, recapture/verify the session, and reattach it.
- No post content available: review the persona's age group and interests. Only one successful manual post per profile is allowed per UTC day.
- Daily scheduler running: wait before retrying a manual trigger, or use the explicitly labeled stop control if you intend to stop the daily batch.

Deleting a session removes it from the monitors using it and marks them disconnected. Deleting a persona removes its assignment. Review those confirmations before proceeding. Full instructions are in [Social Monitoring](user_manual/orion_core.md#social-monitoring).

## Identity Resolution And Impersonation Detection

### Overview

Compares a reference (authentic) profile with a candidate (suspected) profile, on the same or a different platform, and reports an impersonation-risk score, a same-person likelihood score, both profiles side by side, and the evidence behind the scores.

**Search terms:** identity resolution, impersonation, fake account, copycat profile, compare profiles, same person, impersonation risk.

### Navigation

Social Intelligence > Identity Resolution (`/dashboard/identity-resolution`).

### Steps

1. Enter the reference profile URL and the candidate profile URL. Each field confirms the detected platform and handle.
2. Use the swap control if the two were entered the wrong way round.
3. Select **Compare Profiles**.
4. Read the **Verdict**: each score is `Low` below 40, `Moderate` from 40, and `High` from 70.
5. Compare the accounts in **Side-by-side Profiles** and review each signal in the **Structured Evidence Matrix**.

### Troubleshooting

- Compare button disabled: both URLs must be profile links on supported platforms, and they must be two different accounts.
- Link rejected: use the profile URL, not a post, video, pin, subreddit, or search link.
- Fewer signals than expected: profile pictures or fields that cannot be collected are skipped.
- Scores are indicators from public metadata, not legal identity verification. Full instructions are in [Identity Resolution](user_manual/orion_core.md#identity-resolution).

## AI Workspace And Report Chat

### Overview

Orion in-app chat, Profile AI, Nexus chat, report chat widgets, chat prompts, quick prompts, streamed responses, saved chat history, stream recovery, retry after interrupted streams, new conversations, cancel request controls, copy actions, edit-and-resend user messages, markdown responses, token limits, shared chat links, public read-only chat transcripts, AI visibility, and system-setting controlled chat access.

**Search terms:** AI workspace, Nexus chat, report chat, ask AI, chatbot, cancel chat, share response, shared chat, chat history, retry AI response.

### Navigation

`/dashboard/ai` (Profile > AI), `/chat-share/:shareId`, report chat panel, chat widget.

### Quick answer

Open Profile > AI or the visible report chat widget, type the question, send it, and wait for the streamed response. Use Share to create a tokenized read-only transcript when sharing is enabled.

### Steps

  1. Open Profile > AI or a report chat widget.
  2. Start a new chat if needed, or let saved history load.
  3. Type a prompt or choose a quick prompt.
  4. Send with Enter or the send button; use Shift+Enter for a new line.
  5. Wait for streaming response and status text.
  6. Cancel if needed.
  7. Copy responses, edit a user message, or retry from a recoverable error state.
  8. Select Share to create a tokenized read-only shared chat link.

### Troubleshooting

If AI is missing, check system setting, license, and deployment configuration. If a page reload leaves a user message without a Nexus answer, the workspace attempts recovery; retry only when the UI exposes retry context. Shared chat links show only the included transcript and do not grant dashboard access.

Full instructions are in [AI Workspace](user_manual/deepintel.md#ai-workspace).

## Kill Chain And Deep Intel

### Overview

Interactive attack-chain graph in the AI Workspace for authorized penetration testing of a single target: target scope, nodes and phases, prebuilt scenarios, AI hypotheses that must be accepted before they run, running and stopping the chain, node and hypothesis statuses, the Deep Intel assistant chat, the compiled report and exports, and `.kc` chain import and export.

**Search terms:** kill chain, Deep Intel, attack chain, pentest chain, hypothesis, test it, run chain, scenario library, kill chain report, export chain.

### Navigation

Profile > AI > `Kill Chain view` (`/dashboard/ai?view=killchain`).

### Quick answer

Open the Kill Chain view, enter a single authorized domain, IP, URL, or CIDR as the target and press Enter, build the chain (add nodes, load a scenario, or ask Deep Intel), accept hypotheses with `Test it`, then press `Run chain`.

### Steps

  1. Open Profile > AI and select `Kill Chain view`.
  2. Enter the target and press Enter.
  3. Add nodes with `Add node`, load a prebuilt chain from `Scenario library`, or describe the objective in `Deep Intel`.
  4. Review the `AI hypotheses` panel and use `Test it` or `Try` to stage the ones to test, or `Dismiss` the rest.
  5. Press `Run chain`; use `Stop` to halt. Completed steps are not re-run.
  6. Open the report node or the `Exports` layout to download the compiled report.

### Troubleshooting

- `Add node` and `Run chain` disabled: set a valid single target first.
- Node shows `blocked — out of scope`: the step resolved to a host outside the target scope and was not run.
- Need to repeat work: use `Re-run` on a node or `More > Re-arm all nodes`.
- Reset and import are blocked while a chain is running.
- Run the chain only against targets you are explicitly authorized to test.

Full instructions are in [Kill Chain](user_manual/deepintel.md#kill-chain).

## Tenant Homepage, Alerts, And Notifications

### Overview

Tenant risk summaries, category alert cards, notification sidebar entries, IOC matches, alert detail drawers, alert detail reports, alert scanner settings, scan-all actions, scheduled alert scan configuration, flush actions, delete actions, mark-seen actions, custom alert modal, alert pivots, old alert date filtering, alert exports, alert prints, scanner pivots, API pivots, Stealer Log pivots, and admin tenant-alert review from case-management workflows.

**Search terms:** alerts, tenant homepage, alert summary, notification bell, notification sidebar, category alert, export alert, old alerts, alert scan, alert scanner settings.

### Navigation

`/dashboard/profile/homepage`, `/dashboard/profile/alerts/:type`, `/dashboard/profile/alert-scanners`, notification bell.

### Quick answer

Open Profile > Homepage, select an alert category or notification, review details, then export/print, pivot to the matching module, or adjust alert scanner settings if the role allows it.

### Steps

  1. Open Profile > Homepage.
  2. Review alert summaries and risk cards.
  3. Select a category card or notification.
  4. Open matching alert details.
  5. Apply Date Range if old alerts appear.
  6. Export or print the alert report.
  7. Mark seen, delete, flush, or run alert scan if allowed.
  8. Open Alert Scanner Settings to enable or disable future alert categories.

### Troubleshooting

Alerts depend on monitored IOCs, allowed scanner categories, and scan cycles. If old alerts show, apply Date Range. New IOCs can require the next scheduled or manual scan before alerts appear. Disabled alert scanner categories affect future scanning and visibility, not existing stored data.

## Take Down And Takedown Requests

### Overview

Domain and web takedowns with abuse-contact evidence capture, social profile takedowns (automated submission for supported platforms, official portals for the rest), the takedown review queue for root administrators and primary-tenant maintainers, accept, reject with a reason, retry of failed captures, the tenant mail account requirement, and Takedown Mail conversations with abuse contacts.

**Search terms:** take down, takedown, initiate takedown, report takedown, social takedown, impersonation takedown, abuse email, takedown request, retry takedown, takedown mail, mailbox needs activation, reject takedown.

### Navigation

Compromise Monitoring report > `Initiate Takedown`; Profile > Takedown Requests (`/dashboard/profile/take-down`); `Takedown Mail` (`/dashboard/profile/take-down-mail`).

### Quick answer

Open an eligible report and select `Initiate Takedown`, then use the `Domain / Web` tab to capture evidence for a review request, or the `Social Takedown` tab to report an impersonating profile. Reviewers accept a pending request to email the abuse contact, reject it with a reason, or retry a failed capture. Replies appear on the `Takedown Mail` page.

### Steps

  1. Open a Compromise Monitoring report with a target URL and select `Initiate Takedown`, or select `Report` on the Take Down page and enter a target URL.
  2. On `Domain / Web`, add an optional custom email note and select `Report takedown`. Wait while Orion captures evidence and finds the abuse contact.
  3. For an impersonating social profile, open `Social Takedown`, choose the platform, and either fill in the fields and select `Launch Automated Takedown` (automated platforms) or use `Open Report Form` and the shown abuse email (manual platforms).
  4. Reviewers open Profile > Takedown Requests, search or filter by date and status, and use `View` to inspect the evidence.
  5. Select `Accept` on a pending request to email the abuse contact, `Reject` to close it with a required reason, or `Retry` on a failed request to capture fresh evidence.
  6. Open `Takedown Mail` to read replies from abuse contacts and select `Reply` to answer.

### Troubleshooting

- `Accept` disabled or `Mailbox Needs Activation`: the tenant mail account must be activated before abuse emails can be sent. Reject and retry still work.
- `Takedown request not created`: no abuse contact was found or capture failed. The request is still recorded as failed; a reviewer can retry or reject it.
- "A takedown request already exists for this domain": only one domain request per domain per tenant is allowed.
- "Please enter a valid profile URL.": the social profile URL must belong to the selected platform and point to a profile.
- Reddit and YouTube social takedowns need a captured session for that platform.
- Review access: root administrators see every request but can decide only root-owned ones; primary-tenant maintainers decide requests from their tenant and its sub-tenants.

Full instructions are in [Take Down](user_manual/orion_core.md#take-down).

## Custom Alert Creation

### Overview

Manually created monitored alerts, custom alert modal, alert status, alert type, alert title, alert description, IOC type, alert source, alert URL, IOC value, content type, risk, custom indicator tracking, Add Alert form fields, saved alert records, alert update actions, and alert delete actions.

**Search terms:** custom alert, add alert, create alert, alert IOC, monitor custom IOC.

### Navigation

`/dashboard/profile/addcustomalert` or Profile > Homepage > Add Alert.

### Quick answer

Open Add Custom Alert, fill type/status/title/description/source/URL and one IOC bucket, then save.

### Steps

  1. Open Profile > Homepage.
  2. Select Add Alert.
  3. Enter Title.
  4. Enter Description.
  5. Choose Alert Type and IOC Type.
  6. Enter Source.
  7. Enter URL if needed.
  8. Enter IOC value.
  9. Save alert.

### Troubleshooting

If alert creation is unavailable, check role, active status, maintainer/tenant settings, alert scanner visibility, and required fields. URLs must use `http://` or `https://` where validation is enforced.

## Manage Tenant IOCs

### Overview

Tenant-level monitored indicators, IOC categories, IOC values, IOC chips, added indicator entries, remove indicator actions, clear category actions, saved monitored values, onboarding IOC setup, alert matching values, and tenant alert scan dependencies.

**Search terms:** add IOC, tenant IOC, monitored IOC, remove IOC, IOC categories, alert monitoring values.

### Navigation

`/dashboard/profile/ioc`.

### Quick answer

Open Profile > IOC, select an IOC category, type a value, add it, confirm it appears, and save/update where required.

### Steps

  1. Open Profile > IOC.
  2. Search/select IOC category.
  3. Type the value.
  4. Press Enter or Add.
  5. Confirm it appears under added IOCs.
  6. Remove wrong values by chip/delete control.
  7. Use Clear All only intentionally.
  8. Save/update if required.

### Troubleshooting

New IOCs can require matching data and the next scan cycle before alerts appear.

## Account Settings

### Overview

Signed-in user preferences, avatar upload, avatar delete, theme preference, 2FA, password changes, global recovery-key generation, profile visibility, user metadata, public profile access from comments, and personal account page updates.

**Search terms:** account settings, avatar, profile image, theme, 2FA, change password, recovery key, profile visibility, delete account.

### Navigation

`/dashboard/profile/account`.

### Quick answer

Open Profile > Account to manage profile preferences and security. Orion asks for the current password before changing the password or 2FA setting, or before generating/replacing the global recovery key.

### Steps

  1. Open Profile > Account.
  2. Edit visible profile preferences.
  3. Upload avatar if needed.
  4. Delete avatar if needed.
  5. Toggle theme/profile visibility where available.
  6. To change password or 2FA, perform the action and enter the current password in `Confirm your identity`.
  7. To create a recovery key, select `Generate / replace recovery key`, confirm the current password, and save the displayed 43-character key immediately.
  8. Save or let other settings auto-apply depending on control.
  9. To delete your own account, select `Delete Account` in the `Delete Account` card and confirm your login password. Deletion is immediate and permanent.

### Troubleshooting

If confirmation fails, re-enter the current account password. If a recovery key was replaced, discard the old value because it is no longer valid. The recovery key is displayed only once. If public profile remains hidden, check both user profile visibility and tenant profile visibility setting.

`Delete Account` is not shown to administrators or `maintainer` license holders. Deleting an account removes the login, Orion Mail data, and the account encryption key, then signs you out. See {ref}`Delete Your Account <delete-your-account>`.

## Orion Vault

### Overview

Personal credential store with browser-side encryption: vault setup with a vault password, one-time recovery key, unlock and lock, saved credentials (website name, URL, username, password), showing a password after login-password confirmation, editing, deleting, changing the vault password, regenerating the recovery key, recovery with the recovery key, and deleting the vault.

**Search terms:** Orion Vault, vault, password manager, credentials, security passphrase, recovery key, unlock vault, lock vault, forgot vault password, delete vault.

### Navigation

Profile > Orion Vault (`/dashboard/profile/orion-vault`).

### Quick answer

Open Profile > Orion Vault, create a vault password, save the recovery key shown once, then add credentials with `+ Add Credential`. Unlock with the vault password; showing a stored password asks for your login password.

### Steps

  1. Open Profile > Orion Vault.
  2. First time: enter and confirm a `Security Passphrase` that meets the requirements, then select `Enable Data Protection`.
  3. Copy the recovery key, tick `I have saved this code somewhere safe`, and select `I have saved it`.
  4. Select `+ Add Credential`, fill in all four fields, and select `Save Credential`.
  5. To see a password, select `Show` and confirm your application login password.
  6. Use `Lock Vault` when finished. The vault also locks on reload, in a new tab, or on sign-out.

### Troubleshooting

- Forgot the vault password: select `Forgot Security Passphrase?` and use the recovery key to set a new one.
- Lost the recovery key: unlock the vault and use `Regenerate Recovery Key`; the old key stops working.
- Lost both: Orion cannot recover the stored passwords.
- After five wrong vault passwords or recovery keys, unlocking is blocked for 15 minutes.
- The `Confirm your identity` dialog wants your Orion login password, not the vault password. Wrong entries count toward the sign-in limit.
- Website name, URL, and username are not encrypted; keep secrets in the password field only.

Full instructions are in [Orion Vault](user_manual/orion_core.md#orion-vault).

### Autofill and save logins on other websites

**Search terms:** autofill, password autofill, Passwords toggle, generate password, save login, sign-up, create account, browser extension vault.

The Orion browser extension can fill saved Vault logins on sign-in forms and, on sign-up forms, generate a strong password and save a new login. It is off by default.

**Quick answer.** In the extension popup, turn on the `Passwords` toggle in the top-right. Be signed in to Orion in the same browser and unlock the Vault. An Orion icon then appears in login and sign-up fields: use it to fill a saved login, or on a sign-up form select `Generate password & save` to create one.

**Steps — create a login on a sign-up form.**

  1. Turn on the `Passwords` toggle in the extension popup, sign in to Orion in the same browser, and unlock the Vault.
  2. On the sign-up form, select the Orion icon in the password field.
  3. In the `Create a login` panel, enter the username or email, then select `Generate password & save`.
  4. Orion saves the new login to your Vault and fills the username and password fields.

**Troubleshooting.**

  - No icon anywhere: the `Passwords` toggle is off, or the page was open before the extension updated. Turn it on and reload the page.
  - `Create a login` shows a sign-in or unlock prompt: sign in to Orion in the same browser and unlock the Vault, then open the panel again.
  - `Could not save the login (…)`: the save was refused or failed, and the code names the cause. Make sure Orion Intelligence is up to date, then retry.

Full instructions are in {ref}`Password Autofill on Other Websites <password-autofill-on-other-websites>` and {ref}`Create a New Login on a Sign-Up Form <create-a-new-login-on-a-sign-up-form>`.

## Tenant Settings

### Overview

Current-tenant profile and configuration: tenant image, overview, licenses and quota, contact data, custom domain with DNS records and verification, custom webmail domain with mail DNS records, network (SMTP) configuration with verification, Slack and Jira alert integrations, the alert schedule, and privacy and access toggles for user profile visibility, Event Management, admin alert visibility, and parent-tenant alert visibility.

**Search terms:** tenant settings, tenant logo, custom domain, verify domain, CNAME, webmail, custom mail domain, MX record, SPF, SMTP, network configuration, Slack, Jira, alert schedule, user profile visibility, event management toggle, admin alert visibility.

### Navigation

Profile > Tenant Settings (`/dashboard/profile/tenant-settings`).

### Quick answer

Open Profile > Tenant Settings, change the section you need, and use that section's `Save changes`. A custom domain needs its DNS records in place and must show `Verified` before custom webmail can be turned on.

### Steps

  1. Open Profile > Tenant Settings and review the overview, licenses, and quota.
  2. Update contact details and save.
  3. To use your own domain, enter it in `Custom Domain`, add the shown DNS records at your DNS provider, save, and verify until the status is `Verified`.
  4. To host mail on that domain, turn on `Use Custom Domain for Webmail`, add the mail DNS records (MX, SPF, webmail A record), and save. Mail uses the root of the domain, for example `mail.acme.com` for `app.acme.com`.
  5. In `Network Configuration`, enter the account mail and SMTP details, use `Verify Configuration`, and save.
  6. In `Alert Integrations`, enter the Slack or Jira app credentials and use `Connect`.
  7. Choose the `Alert Schedule` and set the `Privacy & Access` toggles, then save with the privacy section's `Save changes`.

### Troubleshooting

- Domain stays `Unverified`: DNS changes can take time to propagate; check every listed record, including the sub-tenant wildcard record when shown.
- Webmail toggle unavailable: the custom domain must be verified first. Sub-tenants see the primary organization's setting as read-only.
- SMTP verification fails after saving: re-enter the account mail password before saving `Network Configuration`.
- `Connect` disabled: save the tenant's Slack or Jira client credentials first.
- Fields read-only or hidden: check role, license, free-only status, and whether the tenant is a sub-tenant.

Full instructions are in [Tenant Settings](user_manual/orion_core.md#tenant-settings).

## User Management And Create New User

### Overview

Tenant user administration, user list, Add User form, username suggestions, email, password, confirm password, forced password reset flag, role, status, subscription, licenses, module access, user disable actions, user delete actions, user update actions, account mail setup, SMTP setup, license-aware sidebar access, trial subscription banner, subscription upgrade modal, and subscription request notification.

**Search terms:** create new user, add user, user creation, Add User information, manage users, disable user, assign license, require password reset, trial banner, subscription upgrade.

### Navigation

`/dashboard/profile/users` or Profile > Users.

### Quick answer

Yes, Orion supports new user creation. Open Profile > Users, select Add User, fill the user information form, choose role/status/licenses, then select Add User.

### Steps

  1. Open Profile.
  2. Select Users.
  3. Select Add User.
  4. Enter Username.
  5. Enter Email.
  6. Enter Password.
  7. Enter Confirm Password.
  8. Choose Role.
  9. Choose Status.
  10. Select visible Licenses/module access.
  11. Set password-reset requirement if the user must change password on first login.
  12. Resolve validation errors.
  13. Select Add User.

### Troubleshooting

If Add User is disabled, configure Account Mail/SMTP fields in System Settings and confirm maintainer/tenant permissions. If username exists, use suggested alternatives. License assignment controls what sidebar groups the user can open. Limited users can see subscription prompts instead of module access, and near-expiry trial users can see a trial banner.

## Tenant Administration

### Overview

Tenant records, verification, status, quotas, and licenses. Administrators manage top-level tenants; primary maintainers manage their associated sub-tenants. Secondary tenant and sub-tenant refer to the same child organization.

**Search terms:** tenant administration, view tenants, manage tenant, tenant profile, tenant status, tenant verification, tenant licenses.

### Navigation

`/dashboard/tenant/view-tenants`, `/dashboard/tenant/view-profiles`, `/dashboard/profile/tenant`.

### Quick answer

Open Tenant > View Tenants or Profile > Tenant, select the tenant, review details, and update allowed verification/status/license/quota fields.

### Steps

  1. Open Tenant menu.
  2. Select View Tenants or View Profiles.
  3. Search/select a tenant.
  4. Review tenant metadata, users, quotas, status, and licenses.
  5. Edit allowed fields.
  6. Save changes.

### Troubleshooting

If tenant screens are missing, check admin/maintainer role and license. Admin tenant management has broader controls than current-tenant settings.

## Create Primary and Secondary Tenants

### Quick answer

Register a top-level tenant through the root domain, then have an administrator enable **Primary Tenant** in **Tenants** and assign its quotas. Register a secondary through that primary's signup page, then have the primary maintainer verify it and allocate its licenses and user quota.

**Search terms:** primary tenant, secondary tenant, sub-tenant, parent tenant, tenant signup, tenant quota, reserved users, account temporarily disabled.

### Checks

- Use the primary's URL for secondary signup; root-domain signup creates a standalone tenant.
- **Add User** creates an account, not a tenant.
- The primary's user pool counts its own non-maintainer users plus the quotas reserved for its children. Each child also has its own user limit.
- No signup link can mean the primary's tenant quota is full.
- **Account temporarily disabled** directs affected users to their network administrator. The responsible operator must check the tenant's parent, status, verification, and quota warnings.
- Disabling primary status disables its children; deleting the primary deletes its children as well.

See [Primary Tenants and Sub-Tenants](primary-tenants-and-sub-tenants) for the complete setup and quota example.

## Backup, Restore, and Tenant Exports

### Navigation

Profile/administration > **Backup & Restore** (`/dashboard/profile/backup-restore`). Tenant-row exports are under **Tenants**; administrators configure **Scheduled Backup** under **System Settings**.

**Search terms:** create backup, instant backup, scheduled backup, retention, tenant export, import ZIP, index.html, tenant.enc, recovery date, backup statistics, secondary backup.

### Quick answer

Administrators create system backups. Standalone maintainers recover their own organization, primary maintainers recover their permitted tenant family, and secondary maintainers request recovery through their primary. Tenant exports use an existing backup and do not capture new changes.

### Steps

1. For a current recovery point, ask an administrator to run **Instant Backup** and wait for completion. Scheduled backups use a three-day loop when enabled; only two system backups are retained.
2. Use **Download** for a listed backup, or **Export** on an authorized tenant row for its latest available recovery point.
3. Check the **backup taken** date, not just the date you downloaded the file. Export confirmation warns about older data.
4. Extract a copy of a tenant ZIP and open **index.html** for general statistics: backup/export dates, included tenants, users, documents, search documents, files, and counts by collection/index. A **secondary** badge identifies a child tenant.
5. To recover, choose **Restore** on a stored backup or **Import** the original tenant export ZIP. Review the date and affected tenants, confirm, and wait for the final result.

An administrator's full-system **index.html** has Overview, Account details, Connections, Findings, Organizations, and Files views, with search and empty-category filtering. It is different from the static tenant-export summary.

### Troubleshooting

- No backup contains the tenant: a system backup must complete after that tenant was created.
- A secondary cannot manage backups directly: contact its primary maintainer.
- The tenant ZIP contains **index.html** and **tenant.enc**. The HTML is a statistics summary, not the restore payload; upload the intact ZIP.
- An administrator's whole-system download is not a tenant import ZIP.
- Modified, unsupported, or incompatible encrypted payloads are rejected. Cross-installation import needs compatible encryption-key configuration managed by the operator.
- A busy job, missing disk space, or a recovery requiring manual intervention needs resolution before another attempt.

A tenant restore affects its recovery scope; a system restore places the whole site in maintenance. A restore replaces current data rather than merging later changes. See [Backup and Restore](user_manual/orion_core.md#backup-and-restore) for permissions, statistics, rollback, and recovery instructions.

## Audit Logs

### Overview

Application activity history, user actions, tenant actions, searches, scans, settings changes, administrative events, actors, usernames, timestamps, audit rows, audit date filters, audit exports, and audit delete permissions.

**Search terms:** audit logs, auditlog, user activity, actor search, delete audit, export audit.

### Navigation

`/dashboard/profile/auditlog` or `/dashboard/tenant/auditlog`.

### Quick answer

Open Audit Logs, search actor/username, apply Date Range, review rows, and export/delete if allowed.

### Steps

  1. Open Profile > Auditlog or Tenant > Auditlog.
  2. Search username/actor if needed.
  3. Open Filter.
  4. Apply Date Range.
  5. Review log rows.
  6. Expand details where available.
  7. Export or delete if allowed.

### Troubleshooting

If no logs appear, clear filters or widen date range. Delete is admin-only where enabled.

## Event Management

### Overview

Tenant-scoped SIEM search, raw log search, basic event query mode, advanced event filters, Event Date range, expandable event rows, raw event details, indicator fields, tenant log investigation, and Event Management visibility.

**Search terms:** event management, SIEM logs, event search, raw logs, event date, advanced event search.

### Navigation

`/dashboard/profile/event-management`.

### Quick answer

Open Event Management, enter a basic or advanced event query, apply Event Date if needed, run search, and expand event rows.

### Steps

  1. Open Profile > Event Management.
  2. Use Basic search or open Advanced search.
  3. Enter event query/filters.
  4. Apply Event Date.
  5. Run search.
  6. Review result count.
  7. Expand rows for raw details.

### Troubleshooting

If Event Management is hidden, check maintainer license, tenant setting, role, and active user status.

## Log Manager

### Overview

Admin-only operational log review, application log entries, INFO logs, WARNING logs, ERROR logs, log date filtering, log type filtering, source files, caller fields, message text, pagination, individual log file delete, flush all logs, confirmation prompts, and operational troubleshooting.

**Search terms:** log manager, system logs, application logs, error logs, warning logs, info logs, flush logs, delete log file.

### Navigation

`/dashboard/profile/log-manager` or Profile > Monitoring > Log Manager.

### Quick answer

Admins can open Monitoring > Log Manager, filter by type or date, review log entries, refresh, delete a log file, or flush all logs after confirmation.

### Steps

  1. Open Profile > Monitoring.
  2. Select Log Manager.
  3. Filter by log type if needed.
  4. Filter by available log date if needed.
  5. Review Type, Time, File, Message, and Source columns.
  6. Refresh when checking new operational output.
  7. Delete or flush logs only when operationally intended.

### Troubleshooting

Log Manager is for admins and operational troubleshooting, not analyst investigation. If it is hidden, check admin role and account status.

## Feeder Scripts

### Overview

Feeder-enabled tenant parser configuration, feeder rule catalog, Social Media rule groups, Defacement and other rule workspaces, feeder script viewing, feeder values, Add tab, View tab, Values tab, URL value upload, parser script upload, shared session upload, ownership dialogs, owner transfer, preview, search, sorting, pagination, enable status, disable status, clear all, value management, ingestion behavior, parsing behavior, and feeder license access.

**Search terms:** feeder, feeder scripts, parser upload, feeder values, feeder workspace, defacement feeder, enable feeder, disable feeder, owner dialog.

### Navigation

`/dashboard/profile/feeder`.

### Quick answer

Open Profile > Feeder, choose a rule, review scripts or values, add URL values or parser/session uploads, then enable, disable, clear, delete, or transfer ownership where permitted.

### Steps

  1. Open Profile > Feeder.
  2. Select the relevant rule from the catalog.
  3. Use View to inspect uploaded scripts or stored values.
  4. Use Add to upload a parser file, shared session ZIP, or newline-separated values when the rule supports it.
  5. Preview, search, sort, and paginate long script/value lists.
  6. Enable, disable, toggle, clear, delete, or transfer owner where allowed.
  7. Save or confirm the action.

### Troubleshooting

If Feeder is missing, check feeder license/module and tenant permission. Python parser uploads must use `.py`, shared session uploads must use `.zip`, and upload limits apply.

## Case Management

### Overview

Investigation case workspace, case list, filters, analytics, alerts mode, case creation, primary entity, related entities, artifacts, uploaded artifact files, artifact integrity verification, linked report artifacts, tasks, comments, linked cases, analyst assignment, tracking board, status movement reasons, closure, read-only closed cases, archive and unarchive, PDF export, tokenized public case shares, revoke shares, and admin tenant-alert review inside case workflows.

**Search terms:** case management, create case, case details, case filters, case analytics, case artifact, artifact integrity, linked report artifact, tracking board, move case, close case, archived cases, share case.

### Navigation

`/dashboard/profile/case-management`, `/dashboard/profile/case-management/tracking-board`, `/dashboard/profile/case-management/case-details?caseId=...`, `/case-share/:shareId`.

### Quick answer

Open Profile > Case Management, create or select a case, add entities/artifacts/tasks/comments, move the case through the tracking board with reasons, close it after resolution, then export, share, or archive where permitted.

### Steps

  1. Open Case Management.
  2. Use Case List filters or Analytics to find the case.
  3. Create a case with title, type, severity, priority, intake source, tags, and primary entity.
  4. Open Case Details.
  5. Add related entities, artifacts, linked reports, uploaded files, tasks, comments, linked cases, and analysts.
  6. Verify artifact file integrity when evidence must be checked.
  7. Move the case through Tracking Board statuses and enter a reason for each move.
  8. Close the case from the closure section after it reaches resolved.
  9. Export PDF, create/revoke public share links, or archive/unarchive where allowed.

### Troubleshooting

Case access depends on role and case-management permission. Analysts have narrower access than admins/maintainers. Closed and archived cases are read-only for update-style actions. Public case links are token scoped and do not grant dashboard access.

## System Settings

### Overview

Privileged app-wide configuration, branding images, logos, app name, language, data-source URLs, adversary URLs, pricing URLs, account mail, SMTP host, SMTP port, SMTP password, alert webhook OAuth credentials, AI toggle, documentation toggle, help toggle, whistle blowing toggle, network behavior toggles, login imagery, email-flow prerequisites, and system branding changes.

**Search terms:** system settings, SMTP settings, account mail, branding, app logo, AI toggle, documentation toggle, whistle blowing, pricing URL, alert webhook, Slack OAuth, Jira OAuth, onion address, Tor address.

### Navigation

`/dashboard/profile/system-settings`.

### Quick answer

Open Profile > System Settings, select Edit, update required configuration/branding/mail fields, then Save.

### Steps

  1. Open Profile > System Settings.
  2. Select Edit.
  3. Update App Name, Language, URLs, logos, and login imagery as needed.
  4. Configure Account Mail.
  5. Configure Account Mail Password.
  6. Configure Account SMTP Server.
  7. Configure Account SMTP Port.
  8. Configure Slack/Jira alert webhook OAuth credentials if tenants will connect those providers.
  9. Save settings.

### Troubleshooting

If Add User or reset email is disabled/failing, confirm Account Mail and SMTP fields. If AI/docs/whistle links are hidden, check system toggles.

The root administrator (`Platform Settings > Configuration`) and primary-tenant maintainers (`Branding > Tenant identity`) can select `Assign Tor address` next to `Onion Address` to publish the tenant at a new `.onion` address. Each assignment replaces the previous address, so old onion links stop working. See {ref}`Tenant Onion Addresses <tenant-onion-addresses>`.

## Static Images And Branding

### Overview

Image resources, visual branding assets, system logos, login imagery, signup imagery, tenant logos, user avatars, dashboard graphics, graph sidebar images, report comment avatars, image uploads, image deletes, default image fallbacks, and cached branding refresh.

**Search terms:** logo, branding image, user avatar, tenant logo, system image, default image.

### Navigation

Account Settings, Tenant Settings, System Settings.

### Quick answer

Update user avatar in Account Settings, tenant logo in Tenant Settings, and global branding images in System Settings.

### Steps

  1. Open the relevant settings page.
  2. Select image picker/upload.
  3. Choose image.
  4. Confirm preview.
  5. Save or wait for upload success.
  6. Delete image to return to default if needed.

### Troubleshooting

If an image is missing, Orion uses default static image paths. Refresh if cached image remains after update.

## Public User Profile Activity

### Overview

Analyst profile pages, report comment avatars, profile links, public interaction history, recommended activity, trust activity, untrust activity, comment activity, report feedback comments, public metadata, hidden profiles, hidden profile sidebar state, open thread actions, user visibility settings, and tenant visibility settings.

**Search terms:** public profile, analyst profile, user activity, report activity, profile hidden, hidden profile, comment user, report feedback comments.

### Navigation

`/dashboard/profile/user/:user_id` or report comment user/avatar.

### Quick answer

Click a visible comment user/avatar or open the profile route to review public report activity.

### Steps

  1. Open a report with comments/feedback.
  2. Select the user name/avatar.
  3. Open profile if visible.
  4. Review recommendation, trust, untrust, and comment activity.
  5. Open an activity thread when the action is available.
  6. Return to report or dashboard.

### Troubleshooting

If the profile is hidden, the sidebar shows a hidden-profile state and the profile-open action is unavailable. Check both user profile visibility and tenant profile visibility settings.

## Support, Documentation, Onion Link, Links, And Whistle Blowing

### Overview

Help navigation, support modal, support email, support subject, support message, documentation link, onion access link, links directory, whistle-blowing link, reporting link, configured external resources, and visible support menu items.

**Search terms:** help, support, documentation, docs, onion link, links, whistle blowing, contact support.

### Navigation

sidebar/profile menu > Help & Support, Documentation, Onion Link, Links, Whistle Blowing.

### Quick answer

Open the relevant sidebar/profile support item; for Help & Support, fill email, subject, and message, then submit.

### Steps

  1. Open sidebar or profile menu.
  2. Select Help & Support or the external link item.
  3. For support, enter email.
  4. Enter subject.
  5. Enter message.
  6. Submit.
  7. For documentation/link items, follow the opened page.

### Troubleshooting

If a link is missing, check System Settings feature toggles and license.

`Whistle Blowing` opens the Orion Leaks reporting portal; see [Orion Leaks](user_manual/orion_leaks.md).

## Subscription, Payment, Trial, And Locked Modules

### Overview

License access, paywalls, subscription upgrade modal, subscription request form, subscription request notification, payment gateway redirects, trial banners, remaining days, dimmed demo modules, free user restrictions, strategic search warnings, module unlock behavior, general license, breach license, defacement license, social license, exploit license, stealer logs license, scanning license, CTI graph license, social mapper license, feeder license, maintainer license, and enterprise license.

**Search terms:** subscription, payment, paywall, locked module, trial, license, cannot access module, module hidden.

### Navigation

locked module click, `/paymentGateway`, subscription prompt.

### Quick answer

If a module is locked, check the user's role/license, complete the subscription request flow if shown, or have an admin assign the required license before using it.

### Steps

  1. Try opening the module.
  2. Read the lock/paywall/subscription message.
  3. Check assigned licenses in tenant/user administration.
  4. Fill the subscription request modal if it appears.
  5. Confirm the subscription request notification.
  6. Admin assigns license/module access.
  7. Reload dashboard.

### Troubleshooting

Free/demo users have limited access. Enterprise generally unlocks the broadest module set.

## Error, Empty, Loading, Disabled, And Export States

### Overview

Shared UI state troubleshooting, empty query screens, loading states, searching states, no-result screens, failed scans, disabled submit buttons, disabled export buttons, pagination, export menus, print actions, download actions, STIX export, session export, delete confirmation, flush confirmation, and clear-all confirmation.

**Search terms:** empty state, no results, loading, disabled button, export, download, print, delete confirmation, scan failed.

### Navigation

any module with search, scan, report, graph, alert, or admin actions.

### Quick answer

Read the visible state, fix missing input/filters/access, wait for loading, or use the available export/download action after data exists.

### Steps

  1. Check whether a query/input is present.
  2. Check filters and date range.
  3. Wait for loading/scanning to finish.
  4. Expand rows or open reports if results exist.
  5. Use export/download/print when enabled.
  6. Confirm destructive actions only when intended.
  7. Reset filters if no results appear.

### Troubleshooting

Disabled export usually means completed data is missing. Disabled submit usually means missing input, invalid input, running scan, access restriction, or incomplete required settings.
