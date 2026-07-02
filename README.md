# fcastellanos.dev

Personal portfolio & CV site for **Francisco Castellanos** — Senior Full-Stack & Agentic Engineer.

🔗 **Live:** [fcastellanos.dev](https://fcastellanos.dev)

[![License: MIT](https://img.shields.io/badge/License-MIT-2FB380.svg)](LICENSE)
[![Firebase Hosting](https://img.shields.io/badge/hosting-Firebase-EF9F27.svg)](https://firebase.google.com/products/hosting)

> A single-file, dependency-free static site with a terminal-inspired design.

*[Español abajo ↓](#español)*

---

## English

### Features

- 🌐 **Bilingual** — instant ES/EN toggle across every section
- 🎨 **Light / dark theme** toggle
- ⌘ **Command palette** (`⌘K`) for keyboard navigation
- 🖥️ Terminal boot animation, typed section headers and scroll reveals
- 🗂️ **Project filtering** by tech tag
- 📨 **Contact form** wired to [Formspree](https://formspree.io) (with honeypot + validation)
- ⚡ No build step, no framework — just HTML, inline CSS and vanilla JS

### Tech stack

| Area | Choice |
|------|--------|
| Markup / style / logic | Vanilla HTML + inline CSS + vanilla JS (single file) |
| Fonts | Google Fonts — Inter + JetBrains Mono |
| Hosting | Firebase Hosting |
| Forms | Formspree (AJAX) |
| CI/CD | GitHub Actions (auto-deploy on merge, PR previews) |

### Project structure

```
.
├── public/               # site root (Firebase "public")
│   ├── index.html        # the entire site (markup + CSS + JS)
│   ├── images/           # assets (profile.svg placeholder)
│   └── *.pdf             # downloadable CVs (ES/EN)
├── firebase.json         # hosting config
├── .firebaserc           # Firebase project alias
└── .github/workflows/    # CI: deploy + PR preview
```

### Run locally

```bash
# any static server works; e.g.
npx serve public
# → http://localhost:3000
```

### Deploy

```bash
npm run deploy          # firebase deploy --only hosting
```

Merges to `main` also deploy automatically via GitHub Actions.

### Swap the profile photo

`public/index.html` ships with an on-brand SVG placeholder. Replace the
`<img src="images/profile.svg">` in the About section with your own square
photo (e.g. `images/profile.jpg`, ≥ 400×400, optimized). A `TODO` comment
marks the exact spot.

---

## Español

Sitio de portafolio y CV personal de **Francisco Castellanos** — Ingeniero Senior Full-Stack & Agentic.

### Características

- 🌐 **Bilingüe** — toggle ES/EN instantáneo en todas las secciones
- 🎨 Toggle de tema **claro / oscuro**
- ⌘ **Paleta de comandos** (`⌘K`) para navegar con teclado
- 🖥️ Animación de arranque tipo terminal, encabezados tecleados y reveals al hacer scroll
- 🗂️ **Filtrado de proyectos** por tecnología
- 📨 **Formulario de contacto** con [Formspree](https://formspree.io) (honeypot + validación)
- ⚡ Sin build, sin framework — solo HTML, CSS inline y JS vanilla

### Correr localmente

```bash
npx serve public
```

### Desplegar

```bash
npm run deploy
```

Los merges a `main` también despliegan automáticamente vía GitHub Actions.

---

## Contributing & license

Contributions welcome — see [CONTRIBUTING.md](CONTRIBUTING.md) and the
[Code of Conduct](CODE_OF_CONDUCT.md). Security issues: [SECURITY.md](SECURITY.md).

Licensed under the [MIT License](LICENSE).
