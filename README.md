# Your website: setup and daily use

This folder is a complete Quarto website. You write pages in Markdown with LaTeX math; Quarto turns them into HTML; GitHub Pages hosts the result for free at `https://USERNAME.github.io`.

---

## Part 1. One-time setup (about an hour)

### 1. Install three things

1. **Git**: <https://git-scm.com/downloads>. On a Mac, running `git --version` in Terminal offers to install it.
2. **Quarto**: <https://quarto.org/docs/get-started/>. Download the installer for your system.
3. **VS Code** (an editor; optional but recommended): <https://code.visualstudio.com>. Inside VS Code, install the extension called **Quarto** (Extensions panel on the left). It gives you a live preview button and math highlighting.

Check they work by opening a terminal (Terminal on Mac, PowerShell on Windows) and running:

```
git --version
quarto --version
```

Each should print a version number.

### 2. Make a GitHub account and the repository

1. Sign up at <https://github.com>. Your username becomes part of your address, so pick it with that in mind.
2. Click **New repository**. Name it exactly `USERNAME.github.io` (with your username). Make it **Public**. Do not add a README.

### 3. Let your computer talk to GitHub

GitHub does not accept passwords from the terminal. The easiest fix is the GitHub CLI: install it from <https://cli.github.com>, then run

```
gh auth login
```

and follow the prompts (choose GitHub.com, HTTPS, and "Login with a web browser").

### 4. Put this folder on GitHub

1. Unzip this folder somewhere permanent, e.g. `Documents/website`.
2. Open `_quarto.yml` and replace `USERNAME` with your GitHub username.
3. In a terminal, go into the folder and run these lines one at a time (replace USERNAME twice in the fifth line):

```
cd ~/Documents/website
git init
git add .
git commit -m "First version"
git branch -M main
git remote add origin https://github.com/USERNAME/USERNAME.github.io.git
git push -u origin main
```

### 5. Publish

```
quarto publish gh-pages
```

Answer **Y** when asked. Quarto builds the site and pushes it to a branch called `gh-pages`. Then on GitHub go to your repository → **Settings** → **Pages** and check that the source is *Deploy from a branch*, branch `gh-pages`, folder `/ (root)`. Set it if not. After a minute or two the site is live at `https://USERNAME.github.io`.

---

## Part 2. Writing a post

1. Inside `posts/`, make a new folder named `YYYY-MM-DD-short-name`, e.g. `posts/2026-11-02-dieudonne-modules/`.
2. Inside it, create `index.qmd`. Start from this template:

```
---
title: "Your title"
subtitle: "Optional one-line summary shown in the list"
date: 2026-11-02
categories: [math, informal]
---

{{< include /_macros.qmd >}}

Your text here, with math like $\Spec \bZ$.
```

3. Preview while you write: run `quarto preview` in the website folder (or click **Preview** in VS Code). A browser tab opens and updates every time you save.
4. When you are happy, publish:

```
git add .
git commit -m "New post"
git push
quarto publish gh-pages
```

The new post appears on the Writing page automatically, newest first. Categories are free-form; use whatever labels you like (`formal`, `informal`, `reflection`, …). The two example posts are there to copy from; delete their folders when you no longer need them.

To keep a post unpublished while you work on it, add `draft: true` to its header.

---

## Part 3. Math cheat sheet

### Math

- Inline: `$\mathcal{O}_X$`. Display: `$$ ... $$` on its own lines.
- Environments such as `align*`, `pmatrix`, `cases` work inside `$$ ... $$`.
- **Your macros** live in `_macros.qmd`. Add lines like `\newcommand{\Frob}{\operatorname{Frob}}`. `\DeclareMathOperator` does not work there; use `\newcommand` with `\operatorname` instead.

### Theorem-like environments

```
::: {#thm-main}
## Optional name
Statement.
:::

::: {.proof}
Proof text.
:::
```

The label prefix decides the type:

| Prefix | Prints as   |
|--------|-------------|
| `thm-` | Theorem     |
| `lem-` | Lemma       |
| `prp-` | Proposition |
| `cor-` | Corollary   |
| `cnj-` | Conjecture  |
| `def-` | Definition  |
| `exm-` | Example     |
| `exr-` | Exercise    |

Unnumbered: `::: {.proof}`, `::: {.remark}`.

**Cross-references:** write `@thm-main` and it prints "Theorem 1" as a link.

### Known limitations

- **Numbering is per type.** You get Definition 1, Theorem 1, Example 1, not one shared counter 1.1, 1.2, 1.3 as with a usual amsthm setup.
- **No TikZ / tikz-cd.** The browser math renderer (MathJax) does not run TikZ. For simple square diagrams, MathJax supports `\begin{CD} ... \end{CD}` (amscd syntax); I have not tested it in this setup. For anything more complex, draw the diagram in LaTeX, export it as an image, and include it with `![](diagram.svg)`.

---

## Part 4. Other pages

- **Home, Notes, Talks, Teaching** are the files `index.qmd`, `notes.qmd`, `talks.qmd`, `teaching.qmd`. Each has a commented format suggestion inside it (lines between `<!--` and `-->` are invisible on the site).
- **PDFs** (CV, notes, talk slides): put them in a folder called `files/` and link to them as `[PDF](files/name.pdf)`. For the CV, save it as `files/cv.pdf` and uncomment the CV line in `index.qmd`.
- **Look and feel**: fonts and colors are in `theme.scss`; theorem styling is in `styles.css`.
