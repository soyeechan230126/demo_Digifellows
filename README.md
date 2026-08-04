# ELN Documentation

User documentation for an Electronic Lab Notebook (ELN) used in synthetic chemistry —
step-by-step guides, annotated screenshots, and short screen-recording tutorials covering
how to create an entry, draw a chemical equation, calculate a reaction batch, and pull
hazard information into a protocol.

Built as a static site with [MkDocs](https://www.mkdocs.org/) and the
[Material](https://squidfunk.github.io/mkdocs-material/) theme. There is no backend and no
database — the published output is plain HTML, CSS, and JavaScript that can be served from
any static host.

## Contents

|Page|Source|What it covers|
|---|---|---|
|Welcome|`docs/index.md`|Landing page and credits|
|How to start|`docs/how-to-start.md`|The main walkthrough: creating an ELN entry, entering data, chemical equations, batch calculation (*Ansatzberechnung*), GHS safety data, status changes|
|Adding chemical equation|`docs/chemicaleditor.md`|Full toolbar reference for the *OpenChemLib* structure editor|
|Scheme|`docs/scheme.md`|Steps of a synthesis *(stub — not yet written)*|

The navigation in `mkdocs.yml` also reserves space for About, Analysis (IR / NMR / MS), and
Report sections that have not been authored yet. See [Known gaps](#known-gaps).

## Local development

Requires Python 3. MkDocs is not vendored, so install it into a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install mkdocs-material
```

Then:

```bash
mkdocs serve    # live-reload preview at http://127.0.0.1:8000
mkdocs build    # regenerate the static site into site/
```

`mkdocs serve` is the only meaningful test for this project — there is no unit test suite.
Broken image and video paths do **not** fail the build; they only show up as missing media
in the browser, so page through anything you touched before pushing.

## Repository layout

```
mkdocs.yml              Site config and navigation — the source of site structure
docs/
  *.md                  Authored content, one file per page
  assets/               Screenshots: img_<feature>_<descriptor>.png
    videos/             Tutorials: <section>-<step>.mp4, thumbnail_<feature>.png
  javascripts/
    extra.js            seekVideo() — jumps an embedded video to a timestamp
site/                   Build output (committed — see below)
LICENSE                 CC BY 4.0 for content, MIT for code
```

### `site/` is committed

This repository tracks its own build output. If you change anything under `docs/` or
`mkdocs.yml`, run `mkdocs build` and commit the regenerated `site/` in the same commit —
otherwise the deployed site silently falls behind the source. Never edit `site/` by hand;
the next build overwrites it.

## Contributing content

**A new page**

1. Create `docs/<kebab-case-name>.md` starting with a single `#` title.
2. Add it to `nav:` in `mkdocs.yml`. Adding a nav entry without the file produces a live
   404, so do both in the same change.
3. Link to it from a related page with a relative Markdown link:
   `[Scheme](scheme.md)` or `[Method 1](how-to-start.md#method-1)`.

**A screenshot**

Save it as `docs/assets/img_<feature>_<descriptor>.png` and embed it with a real `alt`
attribute:

```html
<img class="screenshot" width="70%" alt="Customized entry title" src="../assets/img_title1.png"/>
```

**A tutorial video**

Save the MP4 as `docs/assets/videos/<section>-<step>.mp4` and embed it with controls:

```html
<video controls muted width="60%">
  <source src="../assets/videos/how-to-start-3-4.mp4" type="video/mp4">
</video>
```

Silent background demos use `autoplay muted loop playsinline` instead of `controls`.

**House style**

- One `#` per page, `##` for numbered steps, `###` for method variants.
- Imperative voice — "Click", "Enter", "Add".
- `>` blockquotes for tips and warnings.
- Asset paths are always relative from `docs/`: `../assets/...`. Never absolute.
- German domain terms (*Ansatzberechnung*, *Versuchsanleitung*) are intentional and stay.

## Known gaps

- Seven nav entries have no source file yet: `eln.md`, `team.md`, `license.md`,
  `ir.md`, `nmr.md`, `ms.md`, `report.md` — each is a 404 until written or removed from
  `mkdocs.yml`. (`license.md` now exists.)
- `docs/index.md` still has `_________` placeholders where the ELN name and the funding
  programme belong.
- `docs/scheme.md` is a teaser with an empty link and no content.
- A handful of typos and one malformed `alt` attribute in `docs/how-to-start.md`.

## License

Documentation content is [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/); the
site code is [MIT](https://opensource.org/licenses/MIT). Screenshots depict third-party
software whose interfaces and trademarks remain with their owners. Full terms in
[`LICENSE`](LICENSE).
