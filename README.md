# Documentation SRN Data Dashboard

This repo contains the code necessary to create the Documentation for the SRN Data Dashboard, as shown at https://srnav.com.

For any questions, reach out at hello@srnav.com.

## Building

Needs [Quarto](https://quarto.org), a LaTeX installation, and Python 3.12.

```bash
python3 -m venv ~/.venvs/srn-extraction-whitepaper
~/.venvs/srn-extraction-whitepaper/bin/python -m pip install -r requirements.txt

QUARTO_PYTHON=~/.venvs/srn-extraction-whitepaper/bin/python quarto render main.qmd
```

Keep the venv outside the repo — an in-tree one is ~22k files. `QUARTO_PYTHON` is
required because Quarto renders with the first `python3` on `PATH`, which on macOS
with Homebrew is a bare 3.14 without Jupyter.

## Where the numbers come from

`main.qmd` pulls everything about the dataset at render time from one public,
unauthenticated endpoint:

```
https://api.srnav.com/projects/main-esrs-kpi/releases/latest/meta
```

That returns the release header only — indicator definitions, the company
universe, and the release-time accuracy snapshot — so no data is vendored into
this repo and a render always describes the newest published release. Version
labels, publication date, company counts, the indicator table, and every
accuracy figure in the text are derived from that response.

To reproduce an older build of the paper, pin `RELEASE` in `main.qmd` to an
explicit version (e.g. `"v2.4"`) instead of `"latest"`. Published releases are
immutable, so a pinned render is reproducible.

Rendering therefore requires network access. Note that `srn-benchmarking-documentation.pdf` is committed,
so a re-render right after a new release will change the numbers in it.
