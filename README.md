# Masonic Enterprise — IT Infrastructure & Cloud Engineering Portfolio

Personal portfolio showcasing hands-on work with Microsoft Azure, Microsoft 365, Windows Server, Microsoft Entra ID, Intune, networking, and infrastructure security.

**Live website:** [masonicenterprise.com](https://masonicenterprise.com)

## Overview

The website is built with [Astro](https://astro.build/) and hosted on **Azure Static Web Apps**. Changes to the production branch are built and deployed through GitHub Actions. Cloudflare manages DNS and the `www` redirect.

| Component | Implementation |
| --- | --- |
| Web framework | Astro |
| Hosting | Azure Static Web Apps — Free plan |
| Source control and CI/CD | GitHub and GitHub Actions |
| Domain and DNS | Cloudflare |
| TLS | Azure-managed HTTPS certificate |
| Production branch | `main` |
| Build output | `dist/` |

## Local Development

Requires Node.js and npm.

```bash
npm install
npm run dev
```

To build and preview the production output:

```bash
npm run build
npm run preview
```

## Repository Structure

| Path | Purpose |
| --- | --- |
| `src/pages/index.astro` | Portfolio landing page |
| `public/staticwebapp.config.json` | Static Web Apps security headers and configuration |
| `.github/workflows/` | CI/CD deployment workflow |
| `docs/` | Infrastructure and deployment documentation |

## Deployment and Security

The production site uses automated deployment from GitHub to Azure Static Web Apps. The deployment credential is stored in GitHub Actions Secrets, not in the repository.

HTTP security headers are configured for the production website, including an enforced Content Security Policy, `X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy`, and `Permissions-Policy`.

Repository security measures include Dependabot vulnerability alerts and security updates, secret protection, and push protection. Additional CI/CD workflow hardening is in progress and is not documented as deployed.

For architecture, configuration, and validation details, see [Azure Static Web Apps Deployment](docs/azure-static-web-apps-deployment.md).

## Related Project

[Microsoft Enterprise Infrastructure Lab](https://github.com/MasonicEnterprise/Microsoft-Enterprise-Lab) — Windows Server, hybrid Active Directory, Microsoft Entra ID, Microsoft 365, Intune, and endpoint management. The repository may be private; access to its documentation depends on repository permissions.
