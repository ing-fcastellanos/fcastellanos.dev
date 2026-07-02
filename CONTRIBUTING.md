# Contributing

Thanks for your interest! This is a personal portfolio site, but issues and
pull requests (typo fixes, accessibility, performance, bug reports) are welcome.

## Ground rules

- Be respectful — see the [Code of Conduct](CODE_OF_CONDUCT.md).
- **Never commit secrets.** No API keys, tokens or credentials in the source.
  This is a static, client-side site; anything committed is public. Contact
  details use public form IDs (Formspree) only.

## Workflow

1. **Fork** and create a branch from `main`:
   `feat/<short-name>`, `fix/<short-name>` or `docs/<short-name>`.
2. Make your change. Keep the site **dependency-free** — no build step, no
   frameworks. Everything lives in `public/index.html` (markup + inline CSS + JS).
3. **Test locally** and check both languages, both themes, and mobile widths:
   ```bash
   npx serve public
   ```
4. Open a **Pull Request** against `main` using the PR template. Link any
   related issue (`Closes #123`).

## Style

- Match the surrounding code: inline styles, `data-*` hooks, vanilla JS.
- New user-facing text must be added to **both** the `en` and `es`
  dictionaries and tagged with `data-i18n` so the toggle keeps full coverage.
- Prefer semantic HTML and keep accessibility in mind (`alt`, labels, contrast).

## Reporting bugs

Open an issue with the bug report template — include browser, viewport and
steps to reproduce. Screenshots help.
