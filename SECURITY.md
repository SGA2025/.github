# Security Policy

This security policy applies to all repositories under the
SGA2025 account, including:
- ifpa-website (https://ipfa.info)
- (future repos)

## Supported Versions

For each repository, only the latest version on the `main`
(or `master`) branch is supported for security updates.
Older commits or branches are not maintained.

## Reporting a Vulnerability

If you discover a security vulnerability in any SGA2025 repository,
please report it privately:

1. **Do NOT** open a public GitHub issue.
2. Email: `info@ipfa.info` with subject prefix `[SECURITY]` and
   the affected repository name.
3. Include in your report:
   - Repository affected
   - Description of the vulnerability
   - Reproduction steps
   - Potential impact
   - Suggested mitigation (optional)

We will acknowledge receipt within **5 business days** and provide
a preliminary assessment within **10 business days**.

**Note:** If the affected repository has its own SECURITY.md with a
different contact, use that contact instead. Otherwise, use the
address above.

## Scope

### In scope
- Code in any SGA2025 repository
- GitHub Actions workflows
- Configuration files
- Production deployments

### Out of scope
- Third-party services we link to
- DNS provider issues (report to the registrar)
- GitHub platform issues (report to GitHub Security directly)
- Social engineering attacks against SGA2025 staff

## Security Practices

All SGA2025 repositories follow these practices:

- **Branch protection** on default branch (PR-only merging)
- **Multi-party review protocol** for production changes
  (see CONTRIBUTING.md)
- **Automated security scanning** via:
  - GitHub Dependabot (dependency vulnerabilities)
  - GitHub Secret Scanning (credential leaks, where plan allows)
  - GitHub CodeQL (static code analysis, where plan allows)
- **No secrets in code** — credentials managed via secure
  channels outside the repository
- **No third-party trackers** by default

## Acknowledgments

We thank security researchers who follow responsible disclosure.
With your permission, we will credit you in release notes.
