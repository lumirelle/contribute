# Contribution Guide

> Forked from [Anthony's](https://github.com/antfu/contribute).

Hey there! I'm really excited that you are interested in contributing. This is a general contribution guide for most of [Lumirelle's projects](https://lumirelle.me/projects). Before submitting your contribution, please make sure to take a moment and read through the following guide:

## 🤖 Using AI

You're welcome to use AI tools to help you contribute. But there are two important ground rules:

### 1. Never let an LLM speak for you

When you write a comment, issue, or PR description, use your own words. Grammar and spelling don't matter &ndash; real connection does. AI-generated summaries tend to be long-winded, dense, and often inaccurate. Simplicity is an art. The goal is not to sound impressive, but to communicate clearly.

### 2. Never let an LLM think for you

Feel free to use AI to write code, tests, or point you in the right direction. But always understand what it's written before contributing it. Take personal responsibility for your contributions. Don't say "ChatGPT says..." &ndash; tell us what _you_ think.

PRs that we consider fully vibe-coded may be closed without further explanation.

For more context, see [Using AI in open source](https://roe.dev/blog/using-ai-in-open-source).

## 👨‍💻 Repository Setup

We use [Mise](https://mise.jdx.dev/) to manager what devtools we used and also their versions, cross-platform, also cross-language.

To set Mise up, just following [the official getting started guide](https://mise.jdx.dev/getting-started.html).

## 💡 Commands

### `mise run dev`

Start the development environment.

If it's a backend package, it will start the build process in watch mode.

If it's a frontend project, it usually starts the dev server. You can then develop and see the changes in real time.

### `mise run build`

Build the project for production.

### `mise run start`

If the project is a server application, you can run `mise run start` to start the server after building the project.

If the project is a CLI tool, you can run `mise run start` to run the CLI after building the project.

### `mise run docs`

If the project contains documentation, you can run `mise run docs --dev` to start the documentation dev server. Use `mise run docs --build` to build the docs for production, `mise run docs --start` to start the server after building the documentation.

### `mise run play`

If the project contains playground, it may start the playground usecases.

### `mise run check`

We use [hk](https://hk.jdx.dev/) to manager git hooks, code checking & code fixing.

#### JS/TS

- **OxLint**: Source code linting for JS/TS project, will replace ESLint in the future, tracking https://github.com/antfu/eslint-config/issues/767 for the status;
- **ESLint**: Source code linting (also formatting, [**we don't use formatter, like Prettier and Oxfmt**](#no-formatter)) for JS/TS project, slower but legacy, support more file types than OxLint;
- **tsc**: If the project is written in TypeScript, we may use the TypeScript compiler with `--noEmits` option to make sure there is no type errors;
- **Knip**: We believe less is more. We use [Knip](https://knip.dev/) to make sure there is no unused dependencies/files, or missing dependencies/files;
- **Publint** & **Arethetypeswrong**: We use [publint](https://publint.dev/) and [arethetypeswrong](https://arethetypeswrong.github.io/) to check our distribution to make sure it's ready for publishing.

### `mise run fix`

Like `mise run check`, but will apply fixes automatically if available.

### `mise run test`

Run the tests.

You can filter the tests to be run by `mise run test [match]`, for example, `mise run test foo` will only run test files that contain `foo`.

For some projects, we might have multiple types of tests set up. For example `mise run test --project unit` for unit tests, `mise run test --project e2e` for end-to-end tests. `mise run test` commonly run them together, you can run them separately as needed.

#### Vitest

Config options are often under the `test` field of `vitest.config.ts` or `vite.config.ts`.

Vitest runs in [watch mode by default](https://vitest.dev/guide/features.html#watch-mode), so you can modify the code and see the test result automatically, which is great for [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development).

For projects using Vitest, we make `mise run test` running only once, and `mise run test --watch` running with watch mode.

### `mise run release`

Release a new version. Bump version, then commit the changes with git tag.

Please ensure you have have the latest code from upstream and all tests pass before releasing.

In most cases, we have a `prerelease` task to run all checks and tests before releasing.

### `mise run`

For more, you can run bare `mise run`, which will prompt a list of all available tasks.

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

#### JS/TS

In JS/TS projects, we use [`taze`](https://github.com/antfu/taze) to update the dependencies manually most of the time. As deps updating bots like [Dependabot](https://github.com/dependabot) or [Renovate](https://renovatebot.com/) could be a bit annoying when you have a lot projects.

With `taze`, you can run `taze major -Ir` to check and select the versions to update interactive. `-I` stands for `--interactive`, `-r` stands for `--recursive` for monorepo.

After bumpping, you should runing check, build and test to verify nothing breaks before pushing to main.

### Releasing

Before you do, make sure you have lastest git commit from upstream and all CI passes.

For most of the time, We do `mise run release`.

## JS / TS

After running `mise run release`, it will prompts a list for the target version you want to release. After select, it will bump your package.json and commit the changes with git tag, powered by [`bumpp`](https://github.com/antfu/bumpp).

As NPM has [deprecated the access tokens](https://github.blog/changelog/2025-12-09-npm-classic-tokens-revoked-session-based-auth-and-cli-token-management-now-available/), now it's recommended to build packages on CI/CD, and we may only keep the manual build for the first time to publish the package to NPM:

<table><tr><td valign="top">

#### Build Manually

For the first time, please build the package and publish it to NPM manually. The CLI will ask you for your granular access tokens.

</td></tr><tr><td valign="top">

#### Build on CI/CD

They will be triggered by the `v` prefixed git tag added by `bumpp`. The action is usually defined under `.github/workflows/release.yml`

If you want to create and publish your own fork, you should change the package name and publish it manually at the first time, then following the [guides](https://docs.npmjs.com/trusted-publishers) and setup trusted publishing on your NPM dashboard.

</td></tr></table>

Changelogs are always generated by GitHub Actions.

## 📖 References

### No Formatter

Since ESLint is already configured to format the code, there is no need to duplicate the functionality with Prettier and Oxlint ([_Why We don't Use Prettier_](https://antfu.me/posts/why-not-prettier)). To format the code, you can run `mise run fix` easily.

If you have Prettier or Oxlint installed in your editor, We recommend you disable them when working on the project to avoid conflict.

## 🗒 Additional Info

In case you are interested in, here is Lumirelle's personal configrations and setups:

- [dotfiles](https://github.com/lumirelle/dotfiles) - My dotfiles. 😃

Dev Tools

  - [mise](https://mise.jdx.dev) - Your dev environment, prepped and ready
  - [hk](https://hk.jdx.dev) - Fast, powerful, and flexible hook management for modern development workflows
  - [nub](https://nubjs.com) - The all-in-one JavaScript toolkit that augments Node.js instead of trying to replace it
  - [taze](https://github.com/antfu/taze) - A modern cli tool that keeps your deps fresh
