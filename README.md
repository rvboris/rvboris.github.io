# Personal page

Resume built with [RenderCV](https://rendercv.com) — content in YAML, PDF via Typst.

To compile locally:

```bash
pip install "rendercv[full]"
rendercv render resume-ru.yaml
rendercv render resume-en.yaml
```

Or run the GitHub Action, which builds both PDFs and publishes them as a release.
