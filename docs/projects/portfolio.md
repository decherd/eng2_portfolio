# Engineering II Portfolio — MkDocs + GitHub Pages (Student Guide, using `uv`)

This guide takes you from zero to a live portfolio site using **MkDocs + Material** and **GitHub Pages**. We’ll use:
- **GitHub** (free hosting + repo),
- **Git** (version control),
- **VS Code** (editor),
- **uv** (Python toolchain runner),
- **MkDocs** (static site generator).

Every command that runs a Python tool is shown with **`uv run ...`**, including **`uv run mkdocs serve`** and **`uv run mkdocs gh-deploy`**.

---

## 0) Make a GitHub account
Create a free GitHub account and verify your email.  
👉 [GitHub: Join](https://github.com/join)

---

## 1) Install tools

### A) Install Git
Download and install Git for your OS (Windows/macOS/Linux):  
👉 [Git Downloads](https://git-scm.com/downloads)

Set your Git identity (needed for commits):
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### B) Install VS Code
Download and install Visual Studio Code:  
👉 [VS Code Downloads](https://code.visualstudio.com/Download)

### C) Install `uv`
Install `uv` (fast Python toolchain).  
👉 [uv Installation](https://docs.astral.sh/uv/getting-started/installation/)

---

## 2) Create your project

Open a terminal (or VS Code terminal) and run:

```bash
# Make and enter your project folder
mkdir eng2_portfolio
cd eng2_portfolio

# Install MkDocs + Material into uv's environment
uv pip install mkdocs mkdocs-material
```

Initialize a starter MkDocs site:
```bash
uv run mkdocs new .
```

Create folders for extra pages and media:
```bash
mkdir -p docs/projects docs/assets/images docs/assets/videos
```

Replace your **`mkdocs.yml`** with:

```yaml
site_name: Engineering II Portfolio
site_description: Projects and notes from Engineering II
site_url: https://<YOUR_GITHUB_USERNAME>.github.io/eng2_portfolio/

theme:
  name: material
  features:
    - navigation.tabs
    - navigation.sections
    - toc.integrate
    - content.code.copy
  palette:
    - scheme: default
      primary: indigo
      accent: indigo

nav:
  - Home: index.md
  - Projects:
      - Hello: projects/hello.md
  - About: about.md

markdown_extensions:
  - admonition
  - attr_list
  - md_in_html
  - pymdownx.details
  - pymdownx.superfences
  - pymdownx.highlight:
      anchor_linenums: true
      line_spans: __span
      pygments_lang_class: true
  - pymdownx.inlinehilite
  - pymdownx.snippets
  - pymdownx.tabbed:
      alternate_style: true
```

**`docs/index.md`**
```markdown
# Engineering II — Portfolio

Welcome to my Engineering II portfolio. Here you’ll find selected projects, notes, and demos from the course.

> **Focus areas**
> - Robotics & AI
> - CAD + FEA in Onshape
> - Statics & Dynamics
> - Independent research & projects

Check the **Projects** tab to see examples.
```

**`docs/projects/hello.md`**
```markdown
# Hello Project

This is a simple placeholder project page. Add images and videos as you go.

## Image example
![Example image](../assets/images/example.jpg){ width=600 }

## Video example
<video controls src="../assets/videos/demo.mp4" width="640"></video>

## Notes
- Problem statement
- Approach / method
- Results (data, screenshots)
- Reflection (what worked, next steps)
```

**`docs/about.md`**
```markdown
# About

Hi! I’m a high school senior taking Engineering II. This portfolio collects my projects and learning notes.

**Skills & tools**
- Python, Git/GitHub
- Onshape (CAD), FEA basics
- Robotics fundamentals
- Documentation & presentation
```

---

## 3) Add a `.gitignore` (what & why)

A `.gitignore` tells Git which files **not** to track (e.g., build artifacts, caches). Keep your repo clean.  
👉 [git-scm.com: gitignore](https://git-scm.com/docs/gitignore)  
👉 [GitHub Docs: Ignoring files](https://docs.github.com/en/get-started/getting-started-with-git/ignoring-files)

Create **`.gitignore`** in the project root:
```
# MkDocs build output
site/

# Python cruft
__pycache__/
*.pyc
*.pyo
*.pyd
.env
```

---

## 4) Preview locally

Run a local dev server with `uv`:
```bash
uv run mkdocs serve
```
Open the printed URL (usually `http://127.0.0.1:8000`).  
👉 [MkDocs: Getting Started](https://www.mkdocs.org/getting-started/)

---

## 5) Create a GitHub repo & push

### A) On GitHub
- Create a new public repo named **`eng2_portfolio`**.

### B) In your project folder
```bash
git init
git add .
git commit -m "Initial MkDocs portfolio"
git branch -M main
git remote add origin https://github.com/<YOUR_GITHUB_USERNAME>/eng2_portfolio.git
git push -u origin main
```

👉 [Pro Git Book (free)](https://git-scm.com/book/en/v2)

---

## 6) Publish with **`uv run mkdocs gh-deploy`** (one command)

MkDocs’ built-in publisher **builds** your site and pushes it to the special **`gh-pages`** branch, which GitHub Pages serves automatically. Run:

```bash
uv run mkdocs gh-deploy
```

Notes:
- This builds locally and **commits & pushes** to `gh-pages`.
- Don’t edit files on `gh-pages` manually; they’ll be replaced on the next deploy.
- Verify locally first with `uv run mkdocs serve`.

👉 [MkDocs: Deploying your docs](https://www.mkdocs.org/user-guide/deploying-your-docs/)  
👉 [Material for MkDocs: Publishing your site](https://squidfunk.github.io/mkdocs-material/publishing-your-site/)

Your live site will be:
```
https://<YOUR_GITHUB_USERNAME>.github.io/eng2_portfolio/
```

---

## 7) Update workflow (repeat daily)

1) Edit content in `docs/…` using VS Code.  
2) Preview:
   ```bash
   uv run mkdocs serve
   ```
3) Stage & commit:
   ```bash
   git add .
   git commit -m "Update portfolio content"
   git push
   ```
4) Publish:
   ```bash
   uv run mkdocs gh-deploy
   ```
5) Refresh your Pages URL.

---

## 8) Troubleshooting

- **Default Jekyll page / blank site:** Re-run:
  ```bash
  uv run mkdocs gh-deploy
  ```
  Then confirm your repo has a **`gh-pages`** branch with built files (`index.html`, `assets/`, `.nojekyll`).

- **Live site 404 right after deploy:** Wait a minute and hard-refresh. Check the URL path exactly matches the repo name.

- **Images/videos not loading:** Fix the relative paths (from `docs/projects/hello.md` to `docs/assets/...` is `../assets/...`).

- **Git identity errors:** Set your name/email again:
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "you@example.com"
  ```

---

## Quick Reference (copy/paste)

```bash
# Setup (once per machine)
uv pip install mkdocs mkdocs-material

# Initialize project (once)
mkdir eng2_portfolio && cd eng2_portfolio
uv run mkdocs new .
mkdir -p docs/projects docs/assets/images docs/assets/videos

# Local preview
uv run mkdocs serve

# Git init + first push
git init
git add .
git commit -m "Initial MkDocs portfolio"
git branch -M main
git remote add origin https://github.com/<YOU>/eng2_portfolio.git
git push -u origin main

# Publish to GitHub Pages
uv run mkdocs gh-deploy
```
