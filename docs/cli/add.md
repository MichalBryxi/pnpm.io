---
id: add
title: "pnpm add <pkg>"
---

Installs a package and any packages that it depends on.
By default, any new package is installed as a production dependency.

## TL;DR

| Command                                | Meaning                            |
|----------------------------------------|------------------------------------|
| `pnpm add sax`                         | Save to `dependencies`             |
| `pnpm add -D sax`                      | Save to `devDependencies`          |
| `pnpm add -O sax`                      | Save to `optionalDependencies`     |
| `pnpm add -g sax `                     | Install package globally           |
| `pnpm add sax@next`                    | Install from the `next` tag        |
| `pnpm add sax@3.0.0`                   | Specify version `3.0.0`            |

## Supported package locations

### Install from npm registry

`pnpm add package-name` will install the latest version of `package-name` from
the [npm registry](https://www.npmjs.com/) by default.

If executed in a workspace, the command will first try to check whether other
projects in the workspace use the specified package. If so, the already used version range
will be installed.

You may also install packages by:

* tag: `pnpm add express@nightly`
* version: `pnpm add express@1.0.0`
* version range: `pnpm add express@2 react@">=0.1.0 <0.2.0"`

[the corresponding guide]: #install-from-remote-tarball

### Install from the workspace

Note that when adding dependencies and working within a [workspace], packages
will be installed from the configured sources, depending on whether or not
[`link-workspace-packages`] is set, and use of the
[`workspace: range protocol`].

[workspace]: ../workspaces.md
[`link-workspace-packages`]: ../workspaces.md#link-workspace-packages
[`workspace: range protocol`]: ../workspaces.md#workspace-ranges-workspace

### Install from local file system

There are two ways to install from the local file system:

1. from a tarball file (`.tar`, `.tar.gz`, or `.tgz`)
2. from a directory

Examples:

```sh
pnpm add ./package.tar.gz
pnpm add ./some-directory
```

When you install from a directory, a symlink will be created in the current
project's `node_modules`, so it is the same as running `pnpm link`.

### Install from remote tarball

The argument must be a fetchable URL starting with "http://" or "https://".

Example:

```sh
pnpm add https://github.com/indexzero/forever/tarball/v0.5.6
```

### Install from Git repository

```sh
pnpm add <git remote url>
```

Installs the package from the hosted Git provider, cloning it with Git.

You may install packages from Git by:

```
# latest commit from default branch
pnpm add kevva/is-positive

# git commit hash
pnpm add kevva/is-positive#97edff6f525f192a3f83cea1944765f769ae2678

# git branch
pnpm add kevva/is-positive#master

# git branch relative to refs
pnpm add zkochan/is-negative#heads/canary

# git tag
pnpm add zkochan/is-negative#2.0.1

# v-prefixed git tag
pnpm add andreineculau/npm-publish-git#v0.0.
```

### Semver

You can specify version (range) to install using the `semver:` parameter. For example:

```
# strict semver
pnpm add zkochan/is-negative#semver:1.0.0

# v-prefixed strict semver 
pnpm add andreineculau/npm-publish-git#semver:v0.0.7

# semver version range
pnpm add kevva/is-positive#semver:^2.0.0

# v-prefixed semver version range
pnpm add andreineculau/npm-publish-git#semver:<=v0.0.7
```

#### Subdirectory

You may also install just a subdirectory from a Git-hosted monorepo using the `path:` parameter. For instance:

```
pnpm add RexSkz/test-git-subfolder-fetch#path:/packages/simple-react-app
```

#### Full URL

If you want to be more explicit or are using alternative git hosting, you might want to spell out full git URL:

```
# git+ssh
pnpm add git+ssh://git@github.com:zkochan/is-negative.git#2.0.1

# https
pnpm add https://github.com/zkochan/is-negative.git#2.0.1
```

#### Providers shorthand

You can use a protocol shorthand `[provier]:` for certain Git providers:

```
pnpm add github:zkochan/is-negative
pnpm add bitbucket:pnpmjs/git-resolver
pnpm add gitlab:pnpm/git-resolver
```

If `[provider]:` is omited, it defaults to `github:`.

#### Parameters combination

It is possible to combine multiple parameters by separating them with `&`. This can be useful for forks of monorepos:

```
# Install git branch `beta`
# Install only subfolder `/packages/simple-react-app`
pnpm add RexSkz/test-git-subfolder-fetch.git#beta&path:/packages/simple-react-app
```

## Options

### --save-prod, -P

Install the specified packages as regular `dependencies`.

### --save-dev, -D

Install the specified packages as `devDependencies`.

### --save-optional, -O

Install the specified packages as `optionalDependencies`.

### --save-exact, -E

Saved dependencies will be configured with an exact version rather than using
pnpm's default semver range operator.

### --save-peer

Using `--save-peer` will add one or more packages to `peerDependencies` and
install them as dev dependencies.

### --ignore-workspace-root-check

Adding a new dependency to the root workspace package fails, unless the
`--ignore-workspace-root-check` or `-w` flag is used.

For instance, `pnpm add debug -w`.

### --global, -g

Install a package globally.

### --workspace

Only adds the new dependency if it is found in the workspace.

### --filter &lt;package_selector\>

[Read more about filtering.](../filtering.md)
