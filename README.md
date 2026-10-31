# Personal website — Hugo + PaperMod

Personal professional website and blog about **Product Analytics · Experimentation · Causal Inference**.

- Public URL: <https://ekomissarov.github.io/>
- Intended GitHub repository: `ekomissarov/ekomissarov.github.io`
- Generator: [Hugo](https://gohugo.io/)
- Theme: [PaperMod](https://github.com/adityatelange/hugo-PaperMod)
- Hosting: GitHub Pages
- Content: Markdown, with LaTeX mathematics and occasional code snippets

The site is deliberately minimal: no CMS, database, JavaScript application framework, or npm build pipeline is required for the baseline setup. Hugo generates static files; GitHub Pages serves them.

> **Configuration note:** This document records the agreed project design and provides a reproducible *reference setup*. It is not an audit of the current repository. Before treating it as an exact backup, compare the examples below with the actual `hugo.toml`, theme revision, layouts, and `.github/workflows/` files. In particular, KaTeX integration and the deployment workflow must be verified in the existing repository.

## 1. Site structure and editorial principles

| Section | URL | Purpose |
| --- | --- | --- |
| Home | `/` | Short professional introduction and links |
| Blog | `/posts/` | Essays and technical articles |
| Projects | `/projects/` | Selected public projects |
| About | `/about/` | Professional background |

Editorial guidelines:

- Write articles in **English**.
- Keep the presentation text-first: no decorative cover images.
- Use Markdown headings, links, lists, tables, fenced code blocks, and LaTeX only where they improve clarity.
- Prefer precise explanations and reproducible examples over marketing language.
- Do not publish employer-confidential information, credentials, or internal datasets.
- Hugo/PaperMod renders the page title from front matter: **do not repeat it as `# H1` inside the article**.
- Draft posts should have `draft = true`; published posts should have `draft = false`.

## 2. Prerequisites

On macOS, install Hugo using a trusted package source:

```bash
brew install hugo
hugo version
```

Install Git if necessary and confirm access to your GitHub account:

```bash
git --version
```

Prefer the **Hugo Extended** build for compatibility with theme asset processing. Record the working Hugo version and PaperMod commit when you establish a known-good build; future major upgrades should be tested before deployment.

**Security:** Do not run Hugo with `sudo`. For this baseline setup, there is no need to run arbitrary installation scripts, `npm install`, or commands copied from untrusted themes. Hugo and its templates are not a complete security sandbox, so use trusted binaries and a pinned theme revision.

## 3. Rebuild from scratch

These steps create a new site. **Do not run them over an existing working repository without a backup.**

### 3.1 Create the site and initialize Git

```bash
hugo new site ekomissarov.github.io --format toml
cd ekomissarov.github.io
git init
```

If the repository already exists on GitHub, cloning it (including submodules) is preferable to creating a new site:

```bash
git clone --recurse-submodules https://github.com/ekomissarov/ekomissarov.github.io.git
cd ekomissarov.github.io
```

### 3.2 Add PaperMod

For a fresh repository, add the official PaperMod theme as a Git submodule:

```bash
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

This creates `.gitmodules` and pins the theme to a Git commit. Commit both so the same theme version can be restored later. When cloning an existing repository, use:

```bash
git submodule update --init --recursive
```

Do not blindly update PaperMod to its newest revision; first check the site locally.

### 3.3 Create the minimal configuration

The following `hugo.toml` is a **reference example**, not a claim about the exact current file:

```toml
baseURL = 'https://ekomissarov.github.io/'
languageCode = 'en-us'
title = 'Eugene Komissarov'
theme = 'PaperMod'

[params]
  description = 'Product Analytics · Experimentation · Causal Inference'
  defaultTheme = 'auto'
  ShowReadingTime = true
  ShowPostNavLinks = true

[params.homeInfoParams]
  Title = 'Product Analytics · Experimentation · Causal Inference'
  Content = 'Notes on experiments, statistical inference, and causal reasoning.'

[[menu.main]]
  name = 'Home'
  url = '/'
  weight = 10

[[menu.main]]
  name = 'Blog'
  url = '/posts/'
  weight = 20

[[menu.main]]
  name = 'Projects'
  url = '/projects/'
  weight = 30

[[menu.main]]
  name = 'About'
  url = '/about/'
  weight = 40
```

Adjust the homepage text and other PaperMod options to match the current live site. For a custom domain, `baseURL` and GitHub Pages settings would need to change.

### 3.4 Create content sections

```bash
mkdir -p content/posts content/projects
```

Suggested structure:

```text
ekomissarov.github.io/
├── .github/
│   └── workflows/
│       └── hugo.yaml
├── .gitmodules
├── archetypes/
│   └── default.md
├── content/
│   ├── _index.md
│   ├── about.md
│   ├── posts/
│   │   ├── _index.md
│   │   ├── does-randomness-really-exist.md
│   │   └── what-does-probability-mean.md
│   └── projects/
│       └── _index.md
├── layouts/              # optional custom overrides
├── static/               # optional static assets
├── themes/
│   └── PaperMod/         # Git submodule
├── hugo.toml
└── README.md
```

Minimal section index examples:

`content/posts/_index.md`:

```toml
+++
title = 'Blog'
+++
```

`content/projects/_index.md`:

```toml
+++
title = 'Projects'
+++
```

`content/about.md`:

```toml
+++
title = 'About'
+++

Professional background and contact links go here.
```

The homepage can use PaperMod's `homeInfoParams` configuration shown above. Keep custom templates to a minimum.

## 4. Markdown and mathematical formulas

Posts use TOML front matter delimited by `+++`:

```markdown
+++
title = 'Does Randomness Really Exist?'
date = 2026-10-30
draft = true
description = 'Determinism, uncertainty, and the data-generating process.'
tags = ['Probability', 'Statistics', 'Causal Inference']
+++

*Determinism, uncertainty, and the data-generating process.*

## A Roll of the Dice

Inline mathematics: $P(Y=1)=\theta$.

Display mathematics:

$$
Y=f(X,U)
$$
```

### KaTeX

The intended mathematical renderer is **KaTeX**, with math delimiters passed through Hugo's Goldmark parser. This requires **both** (1) Hugo passthrough configuration and (2) a template that loads and renders KaTeX. The configuration alone does not render formulas.

For Hugo versions supporting Goldmark passthrough, a reference configuration is:

```toml
[markup.goldmark.extensions.passthrough]
  enable = true

  [markup.goldmark.extensions.passthrough.delimiters]
    block = [['$$', '$$']]
    inline = [['$', '$']]
```

Then add a trusted KaTeX integration using a Hugo template override or partial. **Restore the existing tested integration from the repository if present** rather than assuming PaperMod includes it by default. If loading KaTeX from a CDN, the browser requests external assets; vendoring those assets locally avoids that dependency. Pin the KaTeX version, and check the rendered page for inline and display math.

Useful formula-rendering test cases:

- `$\theta$`
- `$P(Y\mid X)$`
- `$T\perp\!\!\!\perp (Y(0),Y(1),X)$`
- `$P(Y\mid\operatorname{do}(T=t))$`
- A multiline `$$ ... $$` expression

## 5. Local preview and build

Preview **including drafts**:

```bash
hugo server -D
```

Open the local URL printed by Hugo (usually <http://localhost:1313/>). The server watches for edits and rebuilds automatically. Stop it with `Ctrl+C`.

Build the publishable site **excluding drafts**:

```bash
hugo --minify
```

Generated files go to `public/` by default. They are build artifacts, not the source of truth; do not commit `public/` when using the GitHub Actions deployment described below.

A useful prepublication check:

```bash
hugo --minify
```

Review the generated page, links, tables, code formatting, formulas, and mobile layout. Confirm that no draft-only or confidential content is published.

## 6. GitHub Pages deployment

For the repository `ekomissarov/ekomissarov.github.io`, the expected Pages URL is <https://ekomissarov.github.io/>.

Recommended setup: **GitHub Actions builds Hugo; GitHub Pages serves the generated static files**. This also means a production build does not need to execute Hugo on the computer used to write articles.

In GitHub repository settings, choose **Settings → Pages → Build and deployment → Source: GitHub Actions**.

Create `.github/workflows/hugo.yaml`:

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
    steps:
      - name: Checkout with theme
        uses: actions/checkout@v4
        with:
          submodules: recursive
          persist-credentials: false

      - name: Install Hugo
        env:
          HUGO_VERSION: '0.150.0' # EXAMPLE: replace with your tested Hugo version
        run: |
          wget -q "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz" -O hugo.tar.gz
          tar -xzf hugo.tar.gz hugo
          sudo mv hugo /usr/local/bin/hugo
          hugo version

      - name: Configure Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: Build
        env:
          HUGO_ENVIRONMENT: production
        run: hugo --gc --minify --baseURL "${{ steps.pages.outputs.base_url }}/"

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    permissions:
      pages: write
      id-token: write
    steps:
      - name: Deploy
        id: deployment
        uses: actions/deploy-pages@v4
```

**Important:** `0.150.0` above is an illustrative version, not a verified current installation. Replace it with a tested, available Hugo Extended release before using this workflow. GitHub Actions references are major-version tags; for stricter supply-chain security, pin actions to reviewed full commit SHAs. Also review workflows before enabling them: unlike Markdown, workflow commands are executable code.

After pushing to `main`, open the repository's **Actions** tab and confirm that both build and deployment succeed. Then verify the public site.

## 7. Standard post publishing cycle

### Step 1 — Create a draft

```bash
hugo new content posts/my-new-article.md
```

If the command uses the default archetype, verify that the resulting front matter contains `draft = true`. Alternatively create the Markdown file manually.

Suggested front matter:

```toml
+++
title = 'My New Article'
date = 2026-10-31
draft = true
description = 'One-sentence summary for readers and search results.'
tags = ['Statistics', 'Experimentation']
+++
```

Use a stable, descriptive filename (slug). Changing it later changes the article URL unless you explicitly configure redirects or aliases.

### Step 2 — Write and review

- Draft in English using Markdown.
- Check definitions, assumptions, equations, references, and code examples.
- Add links to original sources when making historical or technical claims.
- Do not put a duplicate H1 heading into the article body.
- Check that no company-confidential information or secrets are included.

### Step 3 — Preview locally

```bash
hugo server -D
```

Review the draft in the browser, especially KaTeX, code blocks, internal links, and mobile layout.

### Step 4 — Publish

Change the post front matter to:

```toml
draft = false
```

Build without drafts:

```bash
hugo --minify
```

Commit and push:

```bash
git status
git add content/posts/my-new-article.md
git commit -m "Publish article: My New Article"
git push origin main
```

If configuration or templates changed too, review and stage those files deliberately. Avoid `git add .` when unsure whether generated files or secrets are present.

### Step 5 — Verify the live post

1. Check that the GitHub Actions workflow succeeded.
2. Open `https://ekomissarov.github.io/posts/my-new-article/`.
3. Verify text, mathematics, code blocks, metadata, and navigation.
4. Check the Blog listing and internal links.

### Step 6 — Correct or update an existing post

Edit its Markdown source, preview, and commit again. A push to `main` redeploys the site. Keep the original slug for stable links. For substantive changes, consider adding a brief `Last updated` note in the article.

**Quick reference:**

```bash
# Preview all posts, including drafts
hugo server -D

# Build only publishable content
hugo --minify

# Inspect changes
git status
git diff

# Publish reviewed changes
git add content/posts/my-new-article.md
git commit -m "Publish article"
git push origin main
```

## 8. Restore, upgrade, and troubleshoot

**Restore an existing site:**

```bash
git clone --recurse-submodules https://github.com/ekomissarov/ekomissarov.github.io.git
cd ekomissarov.github.io
hugo server -D
```

**Update the theme intentionally:**

```bash
cd themes/PaperMod
git fetch origin
# Select and checkout a reviewed tag or commit.
cd ../..
git status
hugo server -D
```

Commit the submodule pointer change only after testing. Do not modify the theme directly if a Hugo `layouts/` override can achieve the same result.

**Common problems:**

- **Theme not found:** run `git submodule update --init --recursive`.
- **Draft missing from live site:** confirm `draft = false` and check the build logs.
- **Math shown as raw `$...$`:** check both Goldmark passthrough and KaTeX loading/rendering.
- **404 after deployment:** confirm `baseURL`, GitHub Pages source, workflow success, and the article slug.
- **Local preview works but production differs:** compare Hugo versions, production flags, environment, and theme commit.
- **Unexpected extra scripts:** inspect `.github/workflows/`, `package.json`, `go.mod`, Hugo modules, and custom layouts before running build commands.

## 9. Backup and reproducibility checklist

Keep these in Git:

- `hugo.toml` and any additional config files
- `content/`, `layouts/`, `static/`, and `assets/` if used
- `.gitmodules` and the pinned PaperMod submodule reference
- `.github/workflows/` deployment configuration
- `README.md`

Do not commit passwords, API tokens, wallet files, private keys, or recovery phrases. Do not rely on the generated `public/` folder as your backup: the **source repository** is the authoritative copy.

For exact reproducibility, record the working Hugo version, PaperMod commit, and KaTeX version after confirming them from the real project. Rebuild from a clean clone periodically to ensure that the instructions remain accurate.
