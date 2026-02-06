# Security Policy

## Reporting a Vulnerability

We take security seriously. If you discover a security vulnerability, please report it responsibly.

### How to Report

1. **DO NOT** create a public GitHub issue
2. **Preferred**: Use [GitHub's private vulnerability reporting](https://github.com/SQLoot/.github/security/advisories/new)
3. **Alternative**: Email: security@sqloot.dev
4. Include:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact
   - Any suggested fixes (optional)

### What to Expect

- **Acknowledgment**: Within 48 hours
- **Initial assessment**: Within 7 days
- **Resolution timeline**: Depends on severity (critical: ASAP, high: 30 days, medium: 90 days)

### Scope

This policy applies to all SQLoot repositories.

### Recognition

We appreciate responsible disclosure and will acknowledge security researchers in our release notes (unless you prefer to remain anonymous).

## Supported Versions

| Version  | Supported               |
| -------- | ----------------------- |
| Latest   | ✅                       |
| < Latest | ❌ (upgrade recommended) |

## Security Best Practices

When contributing to SQLoot projects:

- Never commit secrets, API keys, or credentials
- Use environment variables for sensitive data
- Keep dependencies up to date
- Follow secure coding practices
- Enable 2FA on your GitHub account

## Security.txt

For automated security tools, we follow [RFC 9116](https://www.rfc-editor.org/rfc/rfc9116.html).
See `/.well-known/security.txt` in production deployments.
