# Git guidelines

## Version control using Git

All projects use [Git](https://git-scm.com), the free and open-source distributed version control system.

Open-source and portfolio based projects will be hosted on [GitHub](https://github.com).

Commercial projects will be hosted using a private GitHub repository or using an alternative source control platform.

### Git workflow

For all projects (with multiple-contributors) use [GitHub Flow](https://gitversion.readthedocs.io/en/latest/git-branching-strategies/githubflow/), a workflow designed to allow for fast, continous release to production without the overhead and complexity of the popular [GitFlow](http://datasift.github.io/gitflow/IntroducingGitFlow.html) methodology.

Further reading:

- [Getting Git Right by Atlassian](https://www.atlassian.com/git)
- [GitHub Flow on GitHub Guides](https://guides.github.com/introduction/flow/)

### Commit messages

Use sane, clean commit messages at all times. Preferences will vary between projects and people (:tada: emoji commit message for example) — ideally follow the semantic commit message model:

#### Semantic commit messages

Inspired by [Sparkbox's article](https://seesparkbox.com/foundry/semantic_commit_messages) and [semantic-git-commits](https://github.com/fteem/git-semantic-commits):

1. Use a rigid commit message format.
2. Prefix commit messages with a type (feat, chore, fix, etc.)
3. Provide a summary in present tense.

Available types: `chore`, `docs`, `feat`, `fix`, `localize`, `refactor`, `style`, `test`

Examples:

```
chore: add build script
docs: explain how feature works
feat: add new component
fix: remove broken confirmation message
localize: translate help to Italian
refactor: share logic between x and y
style: convert tabs to spaces
test: ensure a retains b
```

I have aliases for these set in my `.gitconfig` — see my [dotfiles](https://github.com/aaronbates/dotfiles).

## Semantic versioning

Projects must subscribe to [Semantic Versioning](http://semver.org/) (current version 2.0.0) basing version numbers on `X.Y.Z` where:

1. `X` is the `MAJOR` version 
2. `Y` is the `MINOR` version
3. `Z` is the `PATCH` version

Major version `0.y.z` is for initial development, it should not be considered stable.

Pre-release versions may be denoted using a hyphen and `[0-9A-Za-z-]` identifiers, for example: `1.0.0-alpha` and `1.0.1-beta.1`

Generally begin initial development at `0.1.0` and work towards a stable production `1.0.0` release.

#### When to increment version numbers

Given a version `MAJOR.MINOR.PATCH`, increment the:

1. `MAJOR` version when changes are _not_ backwards-compatible.
2. `MINOR` version when you _add functionality_.
3. `PATCH` version when you make _bug fixes_.