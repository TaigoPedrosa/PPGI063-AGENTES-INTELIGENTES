# Intelligent Agents — course site

Static site presenting the material for the **Intelligent Agents** course
(PPGI · IC / UFAL · 2026.1), organized along the timeline **Past → Present → Future**.

It is a **no-build** site: the browser reads a `structure.json` file that describes the
page hierarchy and renders the Markdown content live. There is no server, framework, or
compilation step, just HTML, CSS, and a single JavaScript file. That is why it runs
directly on **GitHub Pages**.

## How it works

- **`content/structure.json`** defines the tree of sections (sidebar, pages, navigation).
- Each node in the tree is a **page**:
  - a node **with `markdown`** renders that Markdown file (a content page);
  - a node **with `children`** becomes a section page showing its description and
    clickable *cards* for the subpages.
- Navigation uses the URL **hash** (`#/present/best-practices/subagents`), so page
  reloads and direct links work without any server configuration.

Markdown is rendered with rich formatting: headings, lists, tables (with horizontal
scrolling on mobile), syntax-highlighted code blocks, **Mermaid** diagrams, images, and
an **inline PDF viewer** (every `.pdf` link becomes a card that opens the PDF inline).

## Design

*Obsidian glass* theme: dark background with frosty-glass elements, a background aurora,
Fraunces / Hanken Grotesk / JetBrains Mono typography, smooth transitions, and
collapsible items. Responsive layout with a side drawer on mobile.

## Repository structure

```
githubpages/
├── index.html              # page shell (sidebar, top bar, CDN libs)
├── .nojekyll               # disables Jekyll on GitHub Pages (serves _ / folders as-is)
├── assets/
│   ├── css/styles.css      # obsidian glass theme (design tokens in :root)
│   ├── js/app.js           # router + renderer (fetch → marked → highlight)
│   └── images/             # favicons, logo
└── content/
    ├── structure.json      # << the course page tree
    ├── past/               # Markdown + assets per section
    ├── present/
    └── future/             # papers (pdfs/) + world-model pages
```

Each section keeps its own Markdown and *assets* (images, PDFs) next to the files that
reference them, using relative paths.

## Editing the content

1. Write/edit Markdown inside `content/<section>/…`.
2. Put images and PDFs next to the `.md` (or in a subfolder) and reference them with a
   relative path, e.g. `![](figures/diagram.png)`, `[Paper](pdfs/paper.pdf)`.
3. Register the page in `content/structure.json` with `name`, `slug`, `description`,
   and `markdown` (content page) **or** `children` (section page).
4. Use `kebab-case` *slugs*, unique among siblings.

## Running locally

The browser blocks `fetch` over `file://`, so serve the folder over HTTP:

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

If you don't have Python, any static server works:

```bash
npx serve .          # Node
# or the VS Code "Live Server" extension ("Go Live" button)
# open http://localhost:3000
```

## Publishing

It is a pure static site: just serve the contents of `githubpages/` on GitHub Pages
(the `.nojekyll` file ensures folders and files are served without processing). There is
nothing to compile.

> Architecture details and maintenance conventions (including for AI agents) are in
> [CLAUDE.md](CLAUDE.md).
