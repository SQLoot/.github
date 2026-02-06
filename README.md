# .github

Special GitHub repository for organization-wide health files and templates ✨

This repository contains default community health files, issue/PR templates, and workflow templates that apply across all SQLoot repositories.

---

## 📁 Repository Structure

```
.github/
├── ISSUE_TEMPLATE/          # Issue templates for bug reports and features
├── workflow-templates/       # GitHub Actions workflow templates
├── profile/                 # Organization profile README
├── CODE_OF_CONDUCT.md       # Community guidelines
├── CONTRIBUTING.md          # Contribution guidelines
├── SECURITY.md              # Security policy and vulnerability reporting
├── SUPPORT.md               # Support resources
├── CODEOWNERS               # Code ownership rules
├── FUNDING.yml              # Sponsorship configuration
├── dependabot.yml           # Dependabot configuration
└── renovate.json            # Renovate bot configuration (alternative)
```

## 📚 Documentation

- **[Repository Settings Guide](REPOSITORY_SETTINGS.md)** - Comprehensive recommendations for repo configuration, branch protection, and security settings
- **[Workflow Best Practices](WORKFLOW_BEST_PRACTICES.md)** - Guidelines for creating secure and efficient GitHub Actions workflows
- **[Contributing Guidelines](CONTRIBUTING.md)** - How to contribute to SQLoot projects
- **[Security Policy](SECURITY.md)** - How to report security vulnerabilities
- **[Code of Conduct](CODE_OF_CONDUCT.md)** - Community standards and expectations
- **[Support Resources](SUPPORT.md)** - Where to get help

## 🎯 Features

### Community Health Files
Default templates that apply to all repositories in the SQLoot organization:
- Issue templates (bug reports, feature requests)
- Pull request template
- Contributing guidelines
- Code of conduct
- Security policy

### Workflow Templates
Reusable GitHub Actions workflows:
- **CI**: Linting, type checking, and testing with Bun
- **Security**: Dependency auditing and secret scanning
- **Release**: Automated releases with semantic versioning

### Configuration Files
- **CODEOWNERS**: Automatic reviewer assignment
- **FUNDING.yml**: GitHub Sponsors support
- **dependabot.yml**: Automated dependency updates
- **renovate.json**: Alternative automated dependency management

## 🚀 Using This Repository

### For Repository Maintainers

1. **Apply Templates**: Templates in this repo automatically apply to all SQLoot repositories
2. **Use Workflow Templates**: Copy workflow templates to your `.github/workflows/` directory
3. **Follow Guidelines**: Reference the best practices documentation when configuring repos

### For Contributors

1. **Read Contributing Guidelines**: Check [CONTRIBUTING.md](CONTRIBUTING.md) before making changes
2. **Follow Code of Conduct**: All interactions must follow our [Code of Conduct](CODE_OF_CONDUCT.md)
3. **Report Security Issues**: Use our [Security Policy](SECURITY.md) for vulnerability reports

## 🔒 Security

Security is a top priority for SQLoot. This repository includes:
- Security policy with responsible disclosure process
- Workflow templates with security scanning
- Dependabot/Renovate for automated security updates
- Best practices documentation

**Found a vulnerability?** See [SECURITY.md](SECURITY.md) for reporting instructions.

## 🛠️ Configuration Recommendations

This repository includes comprehensive guides for:
- Branch protection rules
- Required status checks
- Security and analysis features
- Access control and permissions
- GitHub Actions best practices

See [REPOSITORY_SETTINGS.md](REPOSITORY_SETTINGS.md) for detailed recommendations.

## 📦 Tech Stack

SQLoot projects primarily use:
- **Runtime**: [Bun](https://bun.sh)
- **Language**: TypeScript
- **Linting**: Biome
- **CI/CD**: GitHub Actions

## 🤝 Contributing

We welcome contributions! Here's how to get started:

1. Read the [Contributing Guidelines](CONTRIBUTING.md)
2. Review the [Code of Conduct](CODE_OF_CONDUCT.md)
3. Fork and create a pull request
4. Ensure all checks pass

## 📄 License

MIT License - see [LICENSE](LICENSE) for details

## 💬 Support

Need help? Check out our [Support Resources](SUPPORT.md):
- GitHub Discussions for questions
- GitHub Issues for bugs
- Security policy for vulnerabilities

---

<div align="center">
  <a href="https://github.com/enterprises/ownCTRL"><img src="https://img.shields.io/badge/©️_2026-ownCTRL™-333?style=flat&labelColor=ddd" alt="© 2026 ownCTRL™"/></a>
  <a href="https://github.com/miccy"><img src="https://img.shields.io/badge/⚙️_Maintained_with_🩶_by-%40miccy-333?style=flat&labelColor=ddd" alt="Maintained by @miccy"/></a>
</div>