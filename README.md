# Kai 學習筆記

## MkDocs

Host using MkDocs.

For full documentation visit [mkdocs.org](https://mkdocs.readthedocs.io/en/stable/).

[GitHub Pages](https://alfredkai.github.io/KaiDoc/)

## Commands

```sh
  MkDocs - Project documentation with Markdown.

Options:
  -V, --version  Show the version and exit.
  -q, --quiet    Silence warnings
  -v, --verbose  Enable verbose output
  -h, --help     Show this message and exit.

Commands:
  build      Build the MkDocs documentation
  gh-deploy  Deploy your documentation to GitHub Pages
  new        Create a new MkDocs project
  serve      Run the builtin development server
```

## Project layout

```sh
mkdocs.yml    # The configuration file.
docs/
    index.md  # The documentation homepage.
    ...       # Other markdown pages, images and other files.
```

## Configure Pages and Navigation

```yaml
nav:
- Home: 'index.md'
- User Guide:
    - 'Writing your docs': 'writing-your-docs.md'
    - 'Styling your docs': 'styling-your-docs.md'
- About:
    - 'License': 'license.md'
    - 'Release Notes': 'release-notes.md'
```

## Warning

MkDocs will allow you to name your index pages as `README.md` instead of `index.md`. **Notice that `README.md` must be capital**.

## Deploy to GitHub Pages

```bat
mkdocs gh-deploy
```

## Themes

[Themes](https://github.com/mkdocs/mkdocs/wiki/MkDocs-Themes)
