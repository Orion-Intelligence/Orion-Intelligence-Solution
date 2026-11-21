# Security Policy

Genesis Technologies, the owner of Orion Intelligence ("we"), takes the security of its users, data, and services
seriously. We welcome responsible reports from security researchers and the wider community.

## Supported Versions

| Version | Supported |
| --- | --- |
| Latest release ([1.0.3.29](https://github.com/Orion-Intelligence/Orion-Intelligence-Solution/releases/latest)) | Yes |
| Earlier releases | No |

The hosted Orion services run the latest release. Security fixes are made on the current code and ship with the next
release. Older releases, forks, and modified or self-hosted third-party deployments are not maintained by us.

## Report a Vulnerability

Please **do not** disclose a suspected vulnerability through a public issue, discussion, pull request, wiki page, or
social-media post. Report it privately through one of these channels:

1. **GitHub (preferred):** the
   [private vulnerability reporting form](https://github.com/Orion-Intelligence/Orion-Intelligence-Solution/security/advisories/new).
   The report is visible only to you and the maintainers. A GitHub account is required.
2. **Email:** [support@orionintelligence.org](mailto:support@orionintelligence.org) with a subject line that starts
   with `[Security]`. You may send a short summary first and ask for a private GitHub advisory to share the full
   details.

Include as much of the following information as possible:

- The affected component, endpoint, feature, or configuration, and the host or URL involved.
- The version or date you tested.
- A clear description of the vulnerability and its potential impact.
- Reproduction steps or a minimal proof of concept.
- Any required permissions, account state, or deployment conditions.
- Suggested remediation, if available.
- Your preferred contact and attribution details.

Do not include user data, credentials, access tokens, or other sensitive material beyond what is strictly necessary to
demonstrate the issue. Please write reports in English.

## Response Targets

We aim to:

- Acknowledge a new report within 8 hours.
- Validate and prioritize the report as quickly as practical.
- Provide progress updates while remediation is underway.
- Deliver a short-term mitigation for confirmed critical issues within 2 days, whenever feasible.

These are response targets rather than guarantees. Resolution time depends on severity, complexity, affected systems,
and the coordination required for a safe release.

## Scope

This policy covers security issues in:

- The Orion Platform web application and API served from `orionintelligence.org` and its subdomains, including tenant
  sites we host.
- Authentication, session handling, access control, tenant isolation, and data-processing functionality.
- Platform features such as Orion Mail, Orion Leaks, Orion Vault, and DeepIntel.
- The official Orion browser extension builds for Chrome and Firefox that the platform distributes.
- Content in this repository, such as documentation that exposes a secret or personal data.

Test official services only where it can be done safely and without disrupting other users.

The following are generally outside the scope of this policy:

- Vulnerabilities in unrelated third-party services or software with no demonstrated Orion-specific impact.
- DNS, mail, or hosting that a tenant configures for its own custom domain, unless the issue lies in the Orion
  Platform itself.
- Automated scanner output without a validated security impact.
- Missing security headers, version or banner disclosure, or email-authentication settings reported without a
  demonstrated exploit.
- Self-XSS, clickjacking on pages without a sensitive action, and logout CSRF.
- Social engineering, phishing, physical attacks, denial-of-service testing, or traffic flooding.
- Availability reports that do not involve a security vulnerability.
- Issues that require an already-compromised device or operating system, an outdated browser, or an unsupported
  deployment.

## Research Guidelines

When investigating a potential vulnerability:

- Use accounts and data that you own or are explicitly authorized to test.
- Access only the minimum data required to demonstrate the issue, and never access, modify, or delete data belonging
  to other users or tenants.
- Do not establish persistence, pivot to other systems, or move beyond the access needed for verification.
- Keep automated testing to a low request rate, and do not brute-force credentials or run load tests.
- Avoid actions that degrade service availability or affect other users.
- Stop testing and report immediately if you encounter sensitive or personal data, and delete any copy you hold once
  the report is resolved.
- Keep the report confidential until a coordinated disclosure date has been agreed upon.

## Coordinated Disclosure

Once a report is validated, we:

- Reproduce the issue from the steps supplied and confirm which components, endpoints, and versions are affected.
- Share the assessed severity and the planned remediation with the reporter, and keep them updated until the fix ships.
- Agree a disclosure date with the reporter before publishing any detail, and ask that the report stay confidential
  until that date.
- Publish a GitHub security advisory once a fix or mitigation is available, listing the affected and fixed versions.
- Credit the reporter in that advisory under the name and contact they supply, or omit attribution when they ask to
  remain anonymous.

## Safe Harbor

Genesis Technologies considers research performed in good faith and in accordance with this policy to be authorized.
We will not pursue legal action against researchers for accidental, good-faith violations of this policy when they
stop, notify us promptly, and cooperate in preventing further harm. This safe harbor does not bind third parties or
authorize activity against systems and data owned by others.
