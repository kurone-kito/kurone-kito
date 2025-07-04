# 📄 Generic repository template

[![Linting](https://github.com/kurone-kito/template/actions/workflows/lint.yml/badge.svg)](https://github.com/kurone-kito/template/actions/workflows/lint.yml)

## Features

- CI/CD
  - [CodeRabbit](https://www.coderabbit.ai/)
  - [ImgBot](https://imgbot.net/)
  - Linting on GitHub Actions
  - Stale issues and pull requests management on GitHub Actions
- Documents for GitHub
- Git attributes
- Linters
  - [CSpell](https://cspell.org/)
  - [EditorConfig](https://editorconfig.org/)
  - [MarkdownLint](https://github.com/DavidAnson/markdownlint)
- Visual Studio Code integration

## Using this template

1. Click "Use this template" on GitHub to create your repository.
2. Replace the LICENSE file if you prefer a different license.
3. Review workflows under `.github/workflows` and adjust them to your needs.
4. Customize the configuration files:
   - `.editorconfig` sets editor rules.
   - `.gitattributes` manages export rules.
   - `.imgbotconfig` controls image optimization.
   - `.markdownlint.yml` and `.markdownlint-cli2.yaml` define Markdown lint rules.
   - `cspell.config.yml` configures spell checking.
   - `.coderabbit.yaml` contains CodeRabbit settings.
   - `.vscode/` provides recommended settings for VS Code.
5. Update documents in `.github/` such as CONTRIBUTING.md to match your policies.

## License

[MIT](./LICENSE)
