# Contributing

Thank you for helping improve Upfera. This guide applies to every repository in the organization that does not provide its own.

## Workflow

1. Planned work starts with a GitHub issue that describes the goal and acceptance criteria. Trivial, unplanned maintenance (for example a typo fix) may skip the issue when the repository policy allows it.
2. Create a branch from `main`. Do not commit directly to `main`.
3. Make focused changes and keep each pull request about one thing.
4. Open a pull request that references its issue (`Closes #123`). Required checks must pass before it can be merged.
5. Pull requests are squash-merged, so the **pull request title becomes the commit message on `main`**.

## Commit and pull request titles

Titles follow [Conventional Commits](https://www.conventionalcommits.org/):

```
type(optional-scope): short description
```

Allowed types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`.

Use `!` after the type or scope for breaking changes, for example `feat(api)!: remove legacy endpoint`.

Examples:

- `feat(website): add pricing page`
- `fix(ci): pin action versions`
- `docs: update contributing guide`

## Signed commits

Commits must be signed so that they show as **Verified** on GitHub. See GitHub's documentation on [commit signature verification](https://docs.github.com/en/authentication/managing-commit-signature-verification).

## Reporting problems

- Bugs and feature requests: open an issue using the provided templates.
- Security issues: do not open an issue. Follow [SECURITY.md](SECURITY.md).
