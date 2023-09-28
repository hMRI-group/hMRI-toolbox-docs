<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/hMRI-group/hMRI-toolbox-docs/blob/main/docs/assets/images/Banner_Dark%40512px.png">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/hMRI-group/hMRI-toolbox-docs/blob/main/docs/assets/images/Banner_Light%40512px.png">
  <img alt="hMRI Toolbox Banner" src="https://github.com/hMRI-group/hMRI-toolbox-docs/blob/main/docs/assets/images/Banner_Light%40512px.png">
</picture>

# hMRI Toolbox Documentation Website

[![Build](https://github.com/hMRI-group/hMRI-toolbox-docs/actions/workflows/build.yml/badge.svg?branch=main)](https://github.com/hMRI-group/hMRI-toolbox-docs/actions/workflows/build.yml)
[![Deploy Site](https://github.com/hMRI-group/hMRI-toolbox-docs/actions/workflows/deploy.yml/badge.svg?branch=main)](https://github.com/hMRI-group/hMRI-toolbox-docs/actions/workflows/deploy.yml)

This repository contains the sources for the (new) hMRI Toolbox website.
This website is based on [MkDocs](https://www.mkdocs.org/) with the
[mkdocs-material](https://squidfunk.github.io/mkdocs-material/) theme, and it is built and deployed automatically
on every push or commit to the `main` branch using GitHub's actions.

Therefore, the normal workflow should be to create a branch from `main` when you work on adding or fixing content.
Make a pull request, where you can then check if the site builds without errors before merging your changes.
However, it's highly recommended to preview the site locally because it's faster and setting up the environment for
preview is easy.

# Editing and Building the Site Locally

The easiest way to edit the website is to preview your changes locally before making a commit.
With MkDocs this is convenient because you only need a virtual Python environment with necessary packages to
run the whole website preview.

## Setup and Virtual Environment

MkDocs uses Python to build a static website from Markdown files. Therefore, 
ensure you have Python 3 and the `venv` package. 

First, clone this repository and inside the repository directory, create a new environment by using

```shell
git clone git@github.com:hMRI-group/hMRI-toolbox-docs.git
cd hMRI-toolbox-docs
python3 -m venv venv
```

Activate the virtual environment with

```shell
source venv/bin/activate
```

Install all required packages

```shell
pip install -r requirements.txt
```

Next time, you only need to activate the virtual environment again.

## Local Preview

After activating the Python environment, run from within the repository directory

```shell
mkdocs serve
```

This will serve the site on `http://127.0.0.1:8000` and you can preview it in your browser.

# Content and Formatting Tips

- If you need to insert a link to the GitHub repository of the Toolbox, don't use `../blob/master/CHANGELOG.md` like in
  the Wiki before! Instead, use `{{config.repo_url}}/blob/master/CHANGELOG.md` which injects the right repo URL automatically.
- If you write `R2*` in Markdown, remember that star means cursive. You have to use `R2\*` instead!
- In every kind of title or section heading, we use ["Title Case"](https://munch.studio/typography-101-capitalisation-guide) consistently

# Todo

- [x] Gitter invite link for socials at the bottom
- [ ] Add missing reference to Ashburner 2007 in docs/processing.md (look for the TODO item)
- [x] Convert bullet points of references in tutorials/references into subsubsections so that we can link to specific papers.
- [ ] Is the "Future Features" in Development > Contributing still up-to-date?
