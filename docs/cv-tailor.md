# CV Tailoring Studio

A small in-browser tool that **tailors the CV to a specific job with AI** and
exports it to PDF in **English and Spanish**. Fully client-side — no backend,
no build step.

- **Source:** [`public/tailor/index.html`](../public/tailor/index.html)
- **Live:** [fcastellanos.dev/tailor](https://fcastellanos.dev/tailor)

## How it works

1. Enter the **highlights** to emphasize (topics, frameworks, details) and/or
   paste a **job description**.
2. Pick a **tone / preset** — balanced, more agentic/AI, more backend/.NET,
   more full-stack, or leadership/consulting.
3. Paste your **OpenAI API key** and choose a model (gpt-4o-mini, gpt-4o,
   gpt-4.1, o4-mini…). Optionally remember it in this browser (`localStorage`).
4. **Generate** → the model reorders skills and rewrites the summary and bullets
   to surface what's relevant.
5. Review the **preview** (EN/ES toggle) and the *applied adjustments* panel.
6. **Download PDF (EN + ES)** — both languages, timestamped:
   `CV_Francisco_Castellanos_EN_YYYYMMDD-HHMM.pdf`.

## No-invention rule

The AI only **reorders and rewrites** real data to emphasize what's asked. It
never invents companies, roles, dates, metrics, or technologies that aren't in
the master CV.

## Technical notes

- **Privacy:** the API key is only used from your browser to call OpenAI. With
  "remember" on, it's stored in that browser's `localStorage` and sent nowhere else.
- **No hardcoded secrets:** the key is always user-supplied.
- **Master content:** the full CV (EN/ES) is embedded in the `DATA` constant in
  `public/tailor/index.html`. Keep it in sync with the experience content in the
  main site (`public/index.html`) if you update your history.
- **Stack:** OpenAI Chat Completions (strict JSON) + `html2canvas` + `jsPDF`
  (via CDN); Inter + JetBrains Mono from Google Fonts. Shares the site's design
  system (same CSS variables).

## Local development

Because the tool calls the OpenAI API, browsers may block it from `file://`.
Serve it over HTTP instead:

```bash
npx serve public
# then open http://localhost:3000/tailor
```
