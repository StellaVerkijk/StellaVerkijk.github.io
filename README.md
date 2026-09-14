# Stella Verkijk — personal website

A [Jekyll](https://jekyllrb.com/) site built on the *Kards* template by
styleshout.

**The idea:** you should almost never have to open an HTML file. The
content lives in a few plain-text files (`_config.yml` and `_data/*.yml`),
the colours and sizes live in one CSS file (`css/theme.css`), and the page
is generated from those. This README says which file to open for what.

---

## Running it

```bash
bundle install            # first time only
bundle exec jekyll serve  # then open http://localhost:4000
```

Leave that running while you work: save a file and the page rebuilds by
itself, usually before you can switch back to the browser. The one
exception is **`_config.yml`, which needs a restart** (Ctrl+C, then run
`jekyll serve` again).

---

## Quick map: "where do I change…?"

| I want to change… | Open | Look for |
| --- | --- | --- |
| Name, email, location | `_config.yml` | `name:`, `contact:` |
| Social icon links | `_config.yml` | `social:` |
| Which sections appear, their order, menu labels | `_config.yml` | `sections:` |
| Section headings and intro text | `_data/sections.yml` | the section's name |
| Width and alignment of a section's text | `_data/sections.yml` | `text_width:`, `align:` |
| The bio next to the photo | `_data/sections.yml` | `about:` → `lead:` |
| The two buttons under the bio | `_data/sections.yml` | `about:` → `buttons:` |
| Jobs and education | `_data/resume.yml` | |
| Projects / papers | `_data/projects.yml` | |
| Services carousel | `_data/services.yml` | |
| Colours | `css/theme.css` | the `--color-*` lines |
| Menu spacing, icon size | `css/theme.css` | `--nav-*`, `--academicon-scale` |
| Profile photo | replace `images/profile-pic.jpg` | |
| CV / resume PDF | replace `docs/resume_stella_verkijk.pdf` | |
| Browser-tab icon | `favicon.svg` and `favicon.png` | see [below](#the-browser-tab-icon) |

Anything not in this table probably lives in `css/main.css`, which is the
template's own stylesheet — see [Deeper changes](#deeper-changes).

---

## Editing content

### Sections: turning them on and off

Every section of the homepage is listed under `sections:` in
`_config.yml`:

```yaml
  - id: resume
    nav: Resume
    enabled: false
```

* `enabled: false` hides the section **and** its menu item, but keeps all
  its content — flip it back to `true` whenever you want it again.
  Resume and Services are currently off.
* `nav:` is the label in the top bar. Leave it blank to show the section
  without giving it a menu entry.
* **Reordering this list reorders the page.**

### Text width and alignment

Each section in `_data/sections.yml` accepts two optional settings. For
example, to keep the "I'd Love To Hear From You" text in a narrower,
centred column:

```yaml
contact:
  label:   Contact
  heading: I'd Love To Hear From You.
  lead:    …
  text_width: 60rem     # how wide the text may get; leave out for full width
  align:      center    # left, center or right
```

`1rem` is 10px on this site, so `60rem` is 600px. **The default is 700px**,
so a larger number widens the text and a smaller one narrows it; the page
itself is 1140px wide, so values past `114rem` make no further difference.
Delete the two lines to go back to the default. This works for every
section in that file.

### Projects

Copy a block in `_data/projects.yml` and change the values:

```yaml
- title:  "Out-of-Tune rather than Fine-Tuned: How Pre-training…"
  venue:  Journal of Examples, 2025      # optional small grey line
  bio:    A sentence or three about the work.
  figure: images/projects/my-figure.jpg
  url:    "https://aclanthology.org/…"   # blank = card doesn't link
```

Each entry becomes one card — figure on the left, text on the right — and
the whole card opens `url` in a new tab.

> **Watch out for colons.** If a title contains `: ` (very common in paper
> titles), wrap the whole title in `"double quotes"`. Without them YAML
> reads the colon as the start of a new field and the site fails to build.
> The same applies to any value containing a colon.

### Social icons

Entries under `social:` in `_config.yml` take a **full icon class**, so
two icon sets are available and both are bundled — no internet needed:

| Set | Looks like | Browse the icons |
| --- | --- | --- |
| Font Awesome 4 | `fa fa-github` | <https://fontawesome.com/v4/icons/> |
| Academicons | `ai ai-google-scholar` | <https://jpswalsh.github.io/academicons/> |

Academicons covers the academic services — Google Scholar, ORCID, arXiv,
ResearchGate, Semantic Scholar, DBLP, Open Access. Everything else
(email, LinkedIn, GitHub, Bluesky…) comes from Font Awesome. An entry with
`type: email` links to the address under `contact:` instead of needing a
`url:`.

### The browser-tab icon

The little icon next to the page title in the browser tab is a file in
this repo, not something generated — it is currently a pink "SV"
placeholder. There are two versions of it, and both should be replaced
together:

* `favicon.svg` — used by modern browsers. It is a text file you can open
  in any editor; the colour and the letters are right there.
* `favicon.png` — the fallback, 180×180. Replace it with a square PNG of
  the same size or larger.

**Browsers cache these very aggressively.** After swapping them, hard
refresh with Ctrl+Shift+R, or open the site in a private window — the old
icon lingering is almost never a sign that something is broken.

### Adding a page

Drop a Markdown file into `_pages/`:

```markdown
---
title: Publications
label: Research
---

Write the page in **Markdown** here.
```

It is served at `/publications/`. To put it in the top menu, add an entry
to `sections:` in `_config.yml`.

---

## Colours and sizes

`css/theme.css` is a short file of named settings that the rest of the
stylesheet reads from. Change a value once and it updates everywhere:

```css
--color-accent:      #FF0077;   /* the pink: links, buttons, highlights */
--color-accent-dark: #cc005f;   /* its hover colour                     */
--color-dark:        #151515;   /* dark section backgrounds             */
--color-text:        #313131;   /* body copy and headings               */

--nav-logo-gap:      7rem;      /* space between the name and the menu  */
--nav-item-gap:      3rem;      /* space between menu items             */
--academicon-scale:  1.15;      /* size of ai-* icons vs the fa-* ones  */
```

A couple of these are worth knowing about:

* **`--academicon-scale`** — Academicons draws its glyphs smaller than
  Font Awesome does, so the Google Scholar icon is scaled up to match its
  neighbours. If it looks slightly too big or too small, nudge this
  number; `1` means "same size as the rest".
* **Colours are used by name everywhere.** Changing `--color-accent`
  restyles links, buttons, the menu underline, and the project hover
  state in one go.

One gap to be aware of: a few colours in `css/main.css` are written as
`rgba(255, 0, 119, …)` for see-through effects and are *not* covered by
the tokens. If you change the accent colour and something stays pink,
that's why — search `css/main.css` for `rgba(255, 0, 119`.

---

## Deeper changes

Anything not covered above lives in `css/main.css`, the template's own
stylesheet (~2700 lines). It is organised into numbered sections listed
at the top of the file. The two sections written for this site are at the
very bottom:

* **20. site customisations** — the horizontal header bar
* **21. projects** — the project cards and their hover effect

To change something else — say the spacing above a section — the quickest
route is to right-click the element in the browser, choose *Inspect*, find
the CSS rule shown in the panel, then search `css/main.css` for it.

**Rule of thumb:** if a change belongs to *this* site, add it at the
bottom of `css/main.css` under a comment rather than editing the
template's own rules. That keeps our changes easy to find.

---

## How the repo is laid out

```
_config.yml          site identity, sections list, social links
_data/               all page content, as YAML
  sections.yml         headings, intro text, width/alignment per section
  resume.yml           jobs and education
  projects.yml         papers and projects
  services.yml         services carousel
_includes/
  sections/            one file per homepage section (the HTML)
  header.html footer.html section-intro.html social-links.html
_layouts/            default.html (the page shell), page.html
_pages/              standalone Markdown pages
css/
  theme.css            colours and sizes  ← start here
  main.css             the template's stylesheet
  academicons/ font-awesome/ micons/   icon fonts
docs/                the CV PDF
images/              photos and figures
index.html           the homepage: it just assembles the sections
_site/               the generated site — never edit, never commit
```

---

## If the site won't build

The terminal running `jekyll serve` prints the error and the file and line
number. By far the most common cause is a **YAML formatting slip**:

* an unquoted colon inside a value (see the warning above);
* the same setting listed twice in one block — YAML quietly keeps the
  last one, so a setting that seems to be ignored is worth checking for;
* inconsistent indentation — YAML uses spaces, never tabs;
* a missing `-` at the start of a list entry.

If you get stuck, `git diff` shows what changed since the last working
version, and `git checkout <file>` throws those changes away.

---

## Credits

* Design: the *Kards* template by [styleshout](http://www.styleshout.com/).
* Icons: [Font Awesome 4](https://fontawesome.com/v4/) (SIL OFL 1.1 / MIT)
  and [Academicons 1.9.4](https://jpswalsh.github.io/academicons/) by
  James Walsh (SIL OFL 1.1).
