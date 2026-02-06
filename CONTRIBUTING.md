# Contributing to SQLoot

Thank you for your interest in contributing! This document provides guidelines for contributing to SQLoot projects.

## Getting Started

1. Fork the repository
2. Clone your fork
3. Create a feature branch: `git checkout -b feat/your-feature`
4. Make your changes
5. Submit a pull request

## Development Setup

### Prerequisites

- [Bun](https://bun.sh) (latest)
- Git

### Install dependencies

```bash
bun install
```

### Run quality checks

```bash
bun run check
```

## Code Style

We use **Biome** for linting and formatting. Configuration is in `biome.json`.

```bash
# Format code
bun run format

# Lint code
bun run lint
```

## Commit Messages

Use clear, descriptive commit messages:

- `feat: add user authentication`
- `fix: resolve memory leak in sync`
- `docs: update API documentation`
- `refactor: simplify data layer`

## Pull Request Process

1. Ensure all checks pass
2. Update documentation if needed
3. Request review from maintainers
4. Address feedback

## Code of Conduct

Please read our [Code of Conduct](CODE_OF_CONDUCT.md).

## Questions?

Open a discussion or reach out via issues.

---

<div align="center">
  <a href="https://github.com/enterprises/ownCTRL"><img src="https://img.shields.io/badge/©️_2026-ownCTRL™-333?style=flat&labelColor=ddd" alt="© 2026 ownCTRL™"/></a>
  <a href="https://github.com/miccy"><img src="https://img.shields.io/badge/⚙️_Maintained_with_🩶_by-%40miccy-333?style=flat&labelColor=ddd" alt="Maintained by @miccy"/></a>
</div>
