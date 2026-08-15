# AGENTS.md — rvboris.github.io

Personal site of Boris Ryabov: bilingual (RU/EN) landing page + résumé PDFs.
Two independent deliverables in one repo:

1. **Résumé**: `resume-ru.yaml` / `resume-en.yaml` → RenderCV (YAML→Typst→PDF)
2. **Landing page**: `docs/index.html` — self-contained HTML (GitHub Pages serves `docs/`)

## Build & verify

```bash
# install once (version is pinned — see Invariants)
pip install "rendercv[full]==2.8"

# render résumés → rendercv_output/ (gitignored)
rendercv render resume-ru.yaml
rendercv render resume-en.yaml

# preview landing page (script is plain <script>, file:// works too)
cd docs && python3 -m http.server 8765
```

Verification for page changes: screenshots in light AND dark scheme
(`--color-scheme` flag applies at browser launch only — use a separate
session per scheme), mobile at 390px and 360px. For the fluid sim, static
screenshots are not enough — interact (mouse sweep) and compare pixel diff
or record video.

## CI/CD (push to main)

`.github/workflows/main.yml`: rendercv renders both YAMLs → renames
`Борис_Рябов_CV.pdf` → `resume-ru.pdf`, `Boris_Ryabov_CV.pdf` →
`resume-en.pdf` → GitHub release `resume-N` (softprops) → Pages deploys `docs/`.

## Invariants — do not break

- Résumé links in `docs/index.html` must stay byte-identical:
  `https://github.com/rvboris/rvboris.github.io/releases/latest/download/resume-{ru,en}.pdf`
- Release asset names must stay `resume-ru.pdf` / `resume-en.pdf` (page links depend on them)
- RenderCV is pinned `==2.8`: highlights use a `#v(0.5em) **Подзаголовок:**` prefix
  (raw Typst command that survives the markdown→Typst pipeline). A parser change
  in a newer version can silently break this
- Landing page: ONE self-contained HTML file, vanilla JS only, no CDNs except
  Google Fonts (Manrope — needs Cyrillic)
- Accessibility fallbacks must survive any change: `prefers-reduced-motion`
  → fully static page; no WebGL → static themed gradient; JS disabled →
  Russian content fully readable (markup default is RU)
- Language: RU is the default, EN via toggle, persisted in `localStorage('lang')`;
  switching updates `<html lang>`, `<title>`, meta description, OG tags
- Positioning: headline is "Руководитель отдела фронтенд-разработки" /
  "Head of Frontend Development" — deliberately WITHOUT "CTO" (department
  head ≠ C-level; decision made 2026-08). Email everywhere: `rvboris@mail.ru`

## Design system

- Brand green `#27AE60` (rgb(39,174,96)) — same accent in PDF and page
- Résumé theme: `engineeringresumes`, A4
- Page: Manrope, frosted card over WebGL fluid (green dye, dimmed:
  gain 0.16 dark / 0.42 light, uInkDepth 0.5, DENSITY_DISSIPATION 1.6)

## Known gotchas

- `#bg` canvas is a replaced element: `position:fixed; inset:0` does NOT
  stretch it — explicit `width:100%; height:100%` required, otherwise the
  sim renders into a 300×150 corner
- Mobile (`max-width: 620px`): `.card` uses `justify-items:stretch` (needed
  for full-width buttons) — `.portrait` must keep `justify-self:start`,
  or the `::before` green frame stretches card-wide and the photo escapes it
- Headless Chromium defaults to dark color-scheme — pass explicit
  `--color-scheme` flags for deterministic theme screenshots

## Content debt (owner: Boris, not yet provided)

Numbers for achievements (before→after); RU "4k+ RPM" vs EN "70+ RPS"
describes the same fact — unify; a Cyrillic "С" sneaked into the EN stack
line (`nodejs, С#, C++`); drop generic "Задачи" lists from recent roles;
compress ENGECON/E-Citrus entries; add a Skills section; state English
level; strengthen the Summary.
