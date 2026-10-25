# Azure Static Web Apps — Portfolio Deployment

## Overview

Deployed an Astro-based IT infrastructure and cloud engineering portfolio to Azure Static Web Apps with GitHub Actions CI/CD, Cloudflare DNS, a custom domain, HTTPS, and HTTP security headers.

**Production website:** [masonicenterprise.com](https://masonicenterprise.com)

## Architecture

```text
Local Astro source
       |
       v
GitHub repository (main)
       |
       v
GitHub Actions build/deploy
       |
       v
Azure Static Web Apps
       |
       v
Cloudflare-managed DNS -> masonicenterprise.com
```

| Component | Configuration |
| --- | --- |
| Frontend | Astro |
| Azure resource | `swa-masonicenterprise` |
| Resource group | `rg-portfolio-prod` |
| Hosting tier | Azure Static Web Apps Free |
| Source | GitHub, `main` branch |
| App location | `/` |
| API location | None |
| Output location | `dist` |
| CI/CD | GitHub Actions |
| Deployment credential | GitHub Actions encrypted repository secret |

## Implementation

### Application and CI/CD

The site was developed locally with Astro and committed to the `portfolio-website` repository. An Azure Static Web Apps resource was connected to GitHub to provision automated builds and production deployments.

The deployment workflow runs on pushes to `main` and supports pull request preview deployments. Its Azure deployment token is referenced through GitHub Actions Secrets; the token value is not included in workflow source.

A separate workflow-hardening branch was created to simplify authentication and review permissions. **The simplified workflow had not yet been merged or deployment-tested at the time of documentation.** The original Azure-generated workflow remains the verified production configuration.

### Custom Domain and DNS

Cloudflare manages the domain's DNS records:

- The apex hostname is configured with a DNS-only CNAME to the Azure Static Web Apps-provided hostname.
- The `www` hostname is proxied through Cloudflare and uses a permanent redirect to the apex domain.
- The redirect preserves URL paths and query strings.
- Existing Microsoft 365 DNS records were retained during the change.

The custom domain was validated in Azure and HTTPS was verified with an issued certificate.

### Security Headers

The file `public/staticwebapp.config.json` configures production response headers:

| Header | Purpose |
| --- | --- |
| `Content-Security-Policy` | Restricts content sources, framing, and form targets |
| `X-Content-Type-Options: nosniff` | Prevents MIME sniffing |
| `X-Frame-Options: DENY` | Disallows framing |
| `Referrer-Policy: strict-origin-when-cross-origin` | Limits referrer exposure |
| `Permissions-Policy` | Disables unneeded camera, microphone, and geolocation access |

The Content Security Policy was first deployed in report-only mode and inspected using browser developer tools. After troubleshooting an extension-related report, the policy was changed to enforcement. HTTP response headers were subsequently verified with `curl`.

### Repository Security

Verified repository settings include:

- Default GitHub Actions `GITHUB_TOKEN` permission set to read repository contents and packages
- Azure deployment token stored in Actions Secrets
- Dependency graph and Dependabot security alerts/updates enabled
- Grouped security updates and malware alerts enabled
- Secret Protection and push protection enabled

CodeQL default setup was reviewed for JavaScript/TypeScript and GitHub Actions; a completed successful CodeQL run has not yet been established in this record.

## Validation

| Verification | Result |
| --- | --- |
| GitHub Actions production build/deployment | Verified |
| Apex custom-domain HTTPS | Verified |
| Apex HTTP response | `200 OK` |
| `www` canonical redirect | `301 Moved Permanently` |
| Redirect path/query preservation | Verified |
| Configured response security headers | Verified |
| Enforced Content Security Policy | Verified |

Tests included browser access, developer tools, GitHub Actions status, and `curl` header inspection. These results describe the verified deployment configuration before the subsequent, unmerged workflow-hardening changes.

## Follow-up Work

- Review, merge, and independently verify the simplified deployment workflow
- Separate untrusted pull request validation from credentialed deployment, where appropriate
- Confirm CodeQL analysis completes successfully and review findings
- Configure scheduled npm dependency version updates
- Review Microsoft 365-related DNS records for service correctness independently of website hosting

## Outcome

Established a deployed, HTTPS-enabled portfolio backed by automated Azure Static Web Apps CI/CD and protected by configurable response headers and repository security features.
