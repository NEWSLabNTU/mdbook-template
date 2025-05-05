# mdBook Template + Automatic Publication GitHub Workflow

This repository is a mdBook template that allows you to create a book written in Markdown documents. It features real-time HTML rendering and automatic publication to GitHub Pages using GitHub workflows.

Please install *mdbook* before getting started.

## Install *mdbook*

For Linux users,

1. Run the command on [rustup.rs](https://rustup.rs/) to install *cargo*. Restart your shell after installation.
2. Run `cargo install -f mdbook` to install the *mdbook* binary.

MacOSX users may install the *mdbook* [brew package](https://formulae.brew.sh/formula/mdbook) instead.

## Start Writing Your Book

For of all, write down the authors and the name of this book in [`book.toml`](book.toml).

All content of this book resides in the `src/` directory. Get started from [`src/SUMMARY.md`](src/SUMMARY.md). The page lists all chapters and subchapters in the book and defines the navigation order of these pages. Here is a simple summary example:

```md
# Summary

- [Introduction to Project X](./introduction.md)
- [Installation](./install.md)
  - [Install on Linux](./install_on_linux.md)
  - [Install on Windows](./install_on_windows.md)
- [Usage](./usage.md)
```

## Live Preview in a Browser

To read the HTML book locally in a browser:

```sh
mdbook serve --open
```

You can keep editing the book. The browser will refresh the pages automatically.

## Publish on GitHub Pages

The GitHub workflow file enables automated HTML rendering and publication to GitHub Pages. You must configure your repository so that the workflow can work.

- [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml)

### Step 1: Register the Deploy Key and the Secret

1. Prepare the **website repository**. The name of the repository should match the domain name of the website. For example, if you host static pages on `newslabntu.github.io`, then use that as the repository name.

    - https://github.com/NEWSLabNTU/newslabntu.github.io

2. Create a **book repository** that hosts the mdBook source files for this book. For example, create an AutoSDV book repository to store Markdown source files.

    - https://github.com/NEWSLabNTU/autosdv-book

3. Generate a private and public key pair using `ssh-keygen` in your terminal.

    ```sh
    ssh-keygen -t ed25519 -f book
    ```

    This command creates a public key file `book.pub` and a private key file named `book`.

4. Register the public key file content `book.pub` as a *deploy key* on the `newslabntu.github.io` repository. See the steps [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys#add-a-deploy-key) to learn how to add a deploy key to the website repository.

5. Register the private key file content `book` as a *repository secret* on the book repository, such as the *autosdv-book* repository mentioned earlier (do not touch the settings). Name the secret `BOOK_DEPLOY_KEY`. See the steps [here](https://docs.github.com/en/actions/security-for-github-actions/creating-and-managing-secrets-for-a-repository) to learn how to register a repository secret on the book repository.

### Step 2: Configure the Publication Workflow

Edit the workflow file [`deploy.yml`](.github/workflows/deploy.yml). Complete all TODOs marked in comments.

First, you may change the name of your website repository to publish to.

```yaml
repository: NEWSLabNTU/NEWSLabNTU.github.io
```

Then, change the procedure to the pull rendered HTML book into the website repository, which is defined in the `Update Website` step.

The exapmle `deploy.yml` assumes that the website repo adds a git submodule linking to the `gh-pages` branch on the book repo. You can check out the submodule tutorial [here](https://www.atlassian.com/git/tutorials/git-submodule). To pull the freshly rendered HTML book, it updates the submodule to link to the most recent commit hash of `gh-pages`, and store the submodule update on the website repo.

## Appendix: Using mdBook Plugins

mdBook supports various preprocessors and backends. You can discover more plugins in the [list](https://github.com/fredrhen/mdbook-plugin-list). Here are a few useful plugins:

- [mdbook-pagetoc](https://crates.io/crates/mdbook-pagetoc): Create a table of contents for each page.
- [mdbook-bib](https://crates.io/crates/mdbook-bib): Add BibLaTeX citations.
- [mdbook-mermaid](https://crates.io/crates/mdbook-mermaid): Embed diagrams in pages using Mermaid language.
