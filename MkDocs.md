# Welcome to MkDocs

For full documentation visit [mkdocs.org](https://mkdocs.readthedocs.io/en/stable/).

## Commands

```sh
mkdocs new [dir-name] - Create a new project.
mkdocs serve - Start the live-reloading docs server.
mkdocs build - Build the documentation site.
mkdocs -h - Print help message and exit.
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

## Themes

[Themes](https://github.com/mkdocs/mkdocs/wiki/MkDocs-Themes)
