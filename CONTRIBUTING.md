# Contributing

Thanks for your interest in contributing to github-owasp-templates.

---

## Ways to Contribute

- **Bug reports** — open an issue if something doesn't work as expected
- **New examples** — add an example for a scenario not already covered
- **Improvements** — better error messages, cleaner YAML, improved docs
- **New scan types** — additional ZAP scan modes or complementary tools

---

## Getting Started

1. Fork the repo
2. Create a branch: `git checkout -b my-improvement`
3. Make your changes
4. Test against a real repo (see Testing below)
5. Open a pull request

---

## Testing Your Changes

Before submitting a PR, test your changes end to end:

1. Push your branch to your fork
2. Create a test repo
3. Reference your branch in the `uses:` line:
   ```yaml
   uses: YOUR-FORK/github-owasp-templates/.github/workflows/owasp-baseline-reusable.yml@your-branch
   ```
4. Run the workflow and confirm it works
5. Reference the test run in your PR

---

## Pull Request Guidelines

- Keep changes focused — one improvement per PR
- Update the README if you add inputs or change behaviour
- Add an example if you're adding a new use case
- Don't bump version tags — maintainers handle releases

---

## Reporting Issues

Open an issue with:
- What you expected to happen
- What actually happened
- Your workflow YAML (redact any secrets)
- The workflow run URL if possible

---

## Code Style

- YAML: 2 space indentation
- Comments: explain *why*, not *what*
- Keep workflow jobs focused — one job per concern
- Avoid third-party actions where native GitHub or `run:` steps work

---

## Questions

Open an issue or start a discussion.
