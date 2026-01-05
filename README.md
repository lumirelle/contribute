# Contribution Guide

Hey there! I'm really excited that you are interested in contributing. This is a general contribution guide for most of [Lumirelle's projects](https://lumirelle.me/projects). Before submitting your contribution, please make sure to take a moment and read through the following guide:

## 👨‍💻 Repository Setup

We use `bun` for the Bun projects, and [`pnpm`](https://pnpm.io/) for the Node.js projects. We highly recommend you install [`ni`](https://github.com/antfu/ni) so you don't need to worry about the package manager when switching across different projects.

We will use `ni`'s commands in the following code snippets. If you are not using it, you can do the conversion yourself: `ni = pnpm/bun install`, `nr = pnpm/bun run`.

To set the `Bun` repository up:

| Step | Command |
| ---- | ------- |
| 1. Install [Bun](https://bun.com/) | `curl -fsSL https://bun.sh/install \| bash` or `powershell -c "irm bun.sh/install.ps1 \| iex"` |
| 2. Install [`@antfu/ni`](https://github.com/antfu/ni) | `bun i -g @antfu/ni` |
| 3. Install dependencies under the project root | `ni` |

To set the `Node.js` repository up:

| Step | Command |
| ---- | ------- |
| 1. Install [Node.js](https://nodejs.org/), using the [latest LTS](https://nodejs.org/en/about/releases/) | [Installation Guides](https://nodejs.org/en/download) |
| 2. [Enable Corepack](#corepack) | `corepack enable` |
| 3. Install [`@antfu/ni`](https://github.com/antfu/ni) (If you already install this package on bun, you can skip this step) | `npm i -g @antfu/ni` |
| 4. Install dependencies under the project root | `ni` |

## 💡 Commands

### `nr dev`

Start the development environment.

If it's a Node.js package, it will start the build process in watch mode using [`tsdown`](https://github.com/rolldown/tsdown).

If it's a frontend project, it usually starts the dev server. You can then develop and see the changes in real time.

### `nr build`

Build the project for production. The result is usually under `dist/`.

### `nr start`

If the project is a server application, you can run `nr start` to start the server after building the project.

If the project is a CLI tool, you can run `nr start` to run the CLI after building the project.

### `nr docs`

If the project contains documentation, you can run `nr docs` to start the documentation dev server. Use `nr docs:build` to build the docs for production.

### `nr play`

If it's a Node.js package, it starts a dev server for the playground. The code is usually under `playground/`.

### `nr lint`

We use [ESLint](https://eslint.org/) for **both linting and formatting** (sometimes we also use [Stylelint](https://stylelint.io/) for style files). It also lints for JSON, YAML and Markdown files if exists.

You can run `nr lint --fix` to let ESLint formats and lints the code.

Learn more about the [ESLint Setup](#eslint).

[**We don't use Prettier**](#no-prettier).

### `nr typecheck`

If the project is written in TypeScript, you can run `nr typecheck` to run the TypeScript compiler with `--noEmits` option to make sure there is no type errors.

### `nr usagecheck`

We believe less is more.

We use [knip](https://knip.dev/) for usage checking to make sure there is no unused dependencies/files, or missing dependencies/files.

### `nr check`

To run all checks (lint, typecheck, usagecheck), you can run `nr check`.

### `nr test`

Run the tests. We mostly using Bun for the Bun projects, and [Vitest](https://vitest.dev/) for Node.js projects - a replacement of [Jest](https://jestjs.io/).

You can filter the tests to be run by `nr test [match]`, for example, `nr test foo` will only run test files that contain `foo`.

For some projects, we might have multiple types of tests set up. For example `nr test:unit` for unit tests, `nr test:e2e` for end-to-end tests. `nr test` commonly run them together, you can run them separately as needed.

#### Bun

Config options are often under the `test` field of `bunfig.toml`.

#### Vitest

Config options are often under the `test` field of `vitest.config.ts` or `vite.config.ts`.

Vitest runs in [watch mode by default](https://vitest.dev/guide/features.html#watch-mode), so you can modify the code and see the test result automatically, which is great for [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development). To run the test only once, you can do `nr test --run`.

### `nr release`

Release a new version. It will prompt you to select the target version, bump the version in `package.json`, commit the changes with git tag.

Please ensure you have have the latest code from upstream and all tests pass before releasing.

In most cases, we have a `prerelease` script to run all checks and tests before releasing.

### `nr`

For more, you can run bare `nr`, which will prompt a list of all available scripts.

## 🙌 Sending Pull Request

### Discuss First

Before you start to work on a feature pull request, it's always better to open a feature request issue first to discuss with the maintainers whether the feature is desired and the design of those features. This would help save time for both the maintainers and the contributors and help features to be shipped faster.

For typo fixes, it's recommended to batch multiple typo fixes into one pull request to maintain a cleaner commit history.

### Commit Convention

We use [Conventional Commits](https://www.conventionalcommits.org/) for commit messages, which allows the changelog to be auto-generated based on the commits. Please read the guide through if you aren't familiar with it already.

Only `fix:` and `feat:` will be presented in the changelog.

Note that `fix:` and `feat:` are for **actual code changes** (that might affect logic).
For typo or document changes, use `docs:` or `chore:` instead:

- ~~`fix: typo`~~ -> `docs: fix typo`

### Pull Request

If you don't know how to send a Pull Request, we recommend reading [the guide](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request).

When sending a pull request, make sure your PR's title also follows the [Commit Convention](#commit-conventions).

If your PR fixes or resolves an existing issue, please add the following line in your PR description (replace `123` with a real issue number):

```markdown
fix #123
```

This will let GitHub know the issues are linked, and automatically close them once the PR gets merged. Learn more at [the guide](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue#linking-a-pull-request-to-an-issue-using-a-keyword).

It's ok to have multiple commits in a single PR, you don't need to rebase or force push for your changes as I will use `Squash and Merge` to squash the commits into one commit when merging.

## 🧑‍🔧 Maintenance

This section is for maintainers with write access, or if you want to maintain your own forks.

### Update Dependencies

Keeping dependencies up-to-date is one of the important aspects to keep projects alive and getting latest bug fixes on time. We recommend to update dependencies in weekly or bi-weekly intervals.

We use [`taze`](https://github.com/antfu/taze) to update the dependencies manually most of the time. As deps updating bots like [Dependabot](https://github.com/dependabot) or [Renovate](https://renovatebot.com/) could be a bit annoying when you have a lot projects.

With `taze`, you can run `taze major -Ir` to check and select the versions to update interactive. `-I` stands for `--interactive`, `-r` stands for `--recursive` for monorepo.

After bumpping, `taze` will install them, then you should runing check, build and test to verify nothing breaks before pushing to main.

### Releasing

Before you do, make sure you have lastest git commit from upstream and all CI passes.

For most of the time, We do `nr release`. It will prompts a list for the target version you want to release. After select, it will bump your package.json and commit the changes with git tag, powered by [`bumpp`](https://github.com/antfu/bumpp).

As NPM will [deprecate the access tokens soon](https://github.blog/changelog/2025-12-09-npm-classic-tokens-revoked-session-based-auth-and-cli-token-management-now-available/), now we only build packages on CI:

<table><tr><td valign="top">

#### Build on CI

They will be triggered by the `v` prefixed git tag added by `bumpp`. The action is usually defined under `.github/workflows/release.yml`

> When maintaining your own fork, you should change the package name and publish manually at the first time, then following the [guides](https://docs.npmjs.com/trusted-publishers) and setup trusted publishing on your NPM dashboard.

</td></tr></table>

Changelogs are always generated by GitHub Actions.

## 📖 References

### Corepack (Only for the Node.js projects)

#### TL;DR

To enable it, run

```bash
corepack enable
```

You only need to do it once after Node.js is installed.

<table><tr><td width="500px" valign="top">

#### What's Corepack

[Corepack](https://nodejs.org/api/corepack.html) makes sure you are using the correct version for package manager when you run corresponding commands. Projects might have `packageManager` field in their `package.json`.

Under projects with configuration as shown on the right, corepack will install `v7.1.5` of `pnpm` (if you don't have it already) and use it to run your commands. This makes sure everyone working on this project have the same behavior for the dependencies and the lockfile.

</td><td width="500px"><br>

`package.json`

```jsonc
{
  "packageManager": "pnpm@7.1.5"
}
```

</td></tr></table>

### ESLint

I use [ESLint](https://eslint.org/) for both linting and formatting with [`@antfu/eslint-config`](https://github.com/antfu/eslint-config).

<table><tr><td width="500px" valign="top">

#### IDE Setup

I recommend using [VS Code](https://code.visualstudio.com/) along with the [ESLint extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint).

With the settings on the right, you can have auto fix and formatting when you save the code you are editing.

</td><td width="500px"><br>

VS Code's `settings.json`

```json
{
  "editor.codeActionsOnSave": {
    "source.fixAll": false,
    "source.fixAll.eslint": true
  }
}
```

</td></tr></table>

### Stylelint

Sometimes I use [Stylelint](https://stylelint.io/) to lint style files like CSS, SCSS or PostCSS, with [`@lumirelle/stylelint-config`](https://github.com/lumirelle/stylelint-config)

<table><tr><td width="500px" valign="top">

#### IDE Setup

I recommend using [VS Code](https://code.visualstudio.com/) along with the [Stylelint extension](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint).

With the settings on the right, you can have auto fix and formatting when you save the code you are editing.

</td><td width="500px"><br>

VS Code's `settings.json`

```json
{
  "editor.codeActionsOnSave": {
    "source.fixAll": false,
    "source.fixAll.stylelint": true
  }
}
```

</td></tr></table>

### No Prettier

Since ESLint is already configured to format the code, there is no need to duplicate the functionality with Prettier ([_Why I don't Use Prettier_](https://antfu.me/posts/why-not-prettier)). To format the code, you can run `nr lint --fix` or referring the [ESLint section](#eslint) for IDE Setup.

If you have Prettier installed in your editor, I recommend you disable it when working on the project to avoid conflict.

## 🗒 Additional Info

In case you are interested in, here is Lumirelle's personal configrations and setups:

- [lumirelle/starship-butler](https://github.com/lumirelle/starship-butler) - Your best starship (means every things you need) butler. 😃

CLI Tools

- [@sxzz/create](https://github.com/sxzz/create) - Command-line for creating projects from templates.
- [ni](https://github.com/antfu/ni) - package manager alias
- [taze](https://github.com/antfu/taze) - dependency updater

In addition of `ni`, here is a few shell aliases to be even lazier:

- `dev`: alias for `nr dev`
- `build`: alias for `nr build`
- `start`: alias for `nr start`
- `docs`: alias for `nr docs`
- `play`: alias for `nr play`
- `lint`: alias for `nr lint`
- `typecheck`: alias for `nr typecheck`
- `usagecheck`: alias for `nr usagecheck`
- `check`: alias for `nr check`
- `test`: alias for `nr test`
- `release`: alias for `nr release`
