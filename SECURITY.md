# Security Policy

## Reporting a vulnerability

If you discover a security issue, please **do not open a public issue**.
Instead, email **ing.fcastellanos@gmail.com** with a description and, if
possible, steps to reproduce. You'll get an acknowledgement as soon as possible.

## Scope

This is a static, client-side site hosted on Firebase Hosting. There is no
backend and no server-side secrets in this repository:

- The contact form uses a **public** Formspree form ID (safe by design).
- CI/CD authenticates to Google Cloud via **Workload Identity Federation**
  (keyless OIDC) — no long-lived credentials are stored in the repo.

## No secrets in source

This site is fully client-side: **anything committed is publicly visible.**
Never commit API keys, tokens, SMTP credentials, or private configuration.
Use public identifiers (like a Formspree form ID) only.
