# hMRI Toolbox Docs

Sources for the hMRI Toolbox website

# Editing and Building the site locally

## Setup and Virtual Environment

This website is based on [MkDocs](https://www.mkdocs.org/) with the 
[mkdocs-material](https://squidfunk.github.io/mkdocs-material/) theme.
MkDocs uses Python to build a static website from Markdown files. Therefore, 
ensure you have Python 3 and the `venv` package. 

First, clone this repository and inside the repository directory, create a new environment by using

```shell
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

- If you need to insert a link to the GitHub repository of the Toolbox, don't use `../blob/master/CHANGELOG.md` like before!
  Instead, use `{{repo_url}}/blob/master/CHANGELOG.md`
- If you write `R2*` in Markdown, remember that star means cursive. You have to use `R2\*` instead!
- In every kind of title or section heading, we use ["title case"](https://munch.studio/typography-101-capitalisation-guide) consistently

# Todo

- [ ] Gitter invite link for socials at the bottom
- [x] Convert bullet points of references in tutorials/references into subsubsections so that we can link to specific papers.
- [ ] In docs/getStarted, the first sentence is "Before using the hMRI Toolbox, please read the README.md and LICENSE files."
      No, this here is going to be the central documentation and everything important will be documented here.
