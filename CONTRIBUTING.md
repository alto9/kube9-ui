# Contributing to kube9-ui

Thank you for your interest in contributing to kube9-ui! This document provides guidelines and information for contributors.

## Code of Conduct

This project adheres to a [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

## How to Contribute

### Reporting Issues

- **Bug Reports**: Use the [bug report template](https://github.com/alto9/kube9-ui/issues/new?template=bug_report.yml)
- **Feature Requests**: Use the [feature request template](https://github.com/alto9/kube9-ui/issues/new?template=feature_request.yml)
- **Documentation**: Use the [documentation template](https://github.com/alto9/kube9-ui/issues/new?template=documentation.yml)

### Pull Requests

1. **Fork the repository** and create your branch from `main`
2. **Install dependencies**: `npm install`
3. **Make your changes** with clear, descriptive commits
4. **Add tests** for any new functionality
5. **Run the test suite**: `npm test`
6. **Run the linter**: `npm run lint`
7. **Submit a pull request** using the PR template

### Development Setup

```bash
# Clone your fork
git clone https://github.com/YOUR-USERNAME/kube9-ui.git
cd kube9-ui

# Install dependencies
npm install

# Start development server
npm run dev

# Run tests
npm test

# Run linter
npm run lint
```

### Coding Standards

- **TypeScript**: Use TypeScript for all new code
- **Formatting**: Code is formatted with Prettier
- **Linting**: ESLint rules are enforced
- **Testing**: Write tests for new features
- **Documentation**: Update docs for API changes

### Commit Messages

Follow conventional commit format:

```
type(scope): description

[optional body]

[optional footer]
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

Examples:
- `feat(cluster): add multi-cluster support`
- `fix(ui): resolve resource tree rendering issue`
- `docs(readme): update installation instructions`

### Branch Naming

- `feature/description` - New features
- `fix/description` - Bug fixes
- `docs/description` - Documentation updates
- `refactor/description` - Code refactoring

## Architecture Guidelines

### Frontend Structure

```
src/
├── components/     # React components
├── hooks/          # Custom React hooks
├── services/       # API and Kubernetes services
├── types/          # TypeScript type definitions
├── utils/          # Utility functions
└── pages/          # Page components
```

### Key Principles

1. **Operator-Centric**: All data comes from OperatorStatus CRD when operator is present
2. **Feature Parity**: Maintain consistency with kube9-vscode
3. **Performance**: Optimize for large clusters
4. **Accessibility**: Follow WCAG guidelines
5. **Responsive**: Support desktop, tablet, and mobile

## Getting Help

- **GitHub Discussions**: Ask questions and get help
- **GitHub Issues**: Report bugs or request features

## Recognition

Contributors will be recognized in:
- Release notes
- Contributors list
- GitHub insights

Thank you for contributing to kube9-ui! 🎉

