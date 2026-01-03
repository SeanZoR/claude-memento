# Contributing to Memento

Thank you for your interest in contributing to Memento! This document provides guidelines and information for contributors.

## Ways to Contribute

### Reporting Issues
- Use GitHub Issues to report bugs or suggest features
- Include clear reproduction steps for bugs
- Describe expected vs actual behavior

### Improving Documentation
- Fix typos or clarify existing docs
- Add examples of memento suggestions
- Translate documentation

### Code Contributions
- Fix bugs
- Add new features (discuss in an issue first)
- Improve the command prompt

## Development Setup

1. Clone the repository:
```bash
git clone https://github.com/SeanZoR/memento.git
cd memento
```

2. Install the command locally:
```bash
cp .claude/commands/memento.md ~/.claude/commands/memento-dev.md
```

3. Test your changes by running `/memento-dev` in Claude Code

## Pull Request Process

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Test thoroughly
5. Commit with clear messages (`git commit -m 'Add amazing feature'`)
6. Push to your fork (`git push origin feature/amazing-feature`)
7. Open a Pull Request

### PR Guidelines
- Keep changes focused and atomic
- Update documentation if needed
- Add examples for new features
- Follow existing code style

## Code Style

### Command Files (.md)
- Use clear, imperative language
- Keep instructions specific and actionable
- Test with various conversation types

### Documentation
- Use GitHub-flavored Markdown
- Include code examples where helpful
- Keep language accessible

## Questions?

- Open a GitHub Discussion for general questions
- Tag maintainers in issues if needed

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
