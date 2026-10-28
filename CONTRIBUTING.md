# Contributing

This repository packages software for Debian and Ubuntu, so the conventions
below are Debian's as much as ours.

## Requirements

```bash
sudo apt install git build-essential devscripts debhelper dput lintian
```

A source package is built with `debuild` and checked with `lintian`, which
flags what Debian's policy rejects.

## Layout

Each package lives in `packages/<name>/`, where `<name>` is its source
package: the `Source:` field of `debian/control`. Binary packages are
named by their own `Package:` field and can differ; only the source package
identifies the unit that is built and uploaded.

A package declares itself to [multicz](https://github.com/goabonga/multicz)
in `packages/<name>/multicz.component.toml`, which the root `multicz.toml`
picks up through `[[component.path]]`, so adding a package never edits a
shared file:

```toml
[components.example-pkg]
changelog = "CHANGELOG.md"

[[components.example-pkg.writers]]
type = "debian-changelog"
distribution = "noble"
```

There is no `bump_files`: the topmost stanza of `debian/changelog` is the
version source, and the `debian-changelog` writer prepends a stanza on every
release. Paths in that file are relative to its directory, and `paths`
defaults to the whole directory. The `distribution` is a series Launchpad
accepts; an upload is reheadered to the series it is made for.

To add a package: create `packages/<name>/` with its `debian/` directory, a
`debian/changelog` whose first stanza is `0.0.0-1` and its
`multicz.component.toml`, then run the checks below.

## Branches

A branch is named after the GitHub issue it addresses:

```
issue-<number>-<slug>
```

`<number>` is the issue number and `<slug>` a few lowercase words separated
by hyphens, for example `issue-12-poc-hello-depends`. Keep it free of `/`:
Git cannot hold `issue-12` and `issue-12/...` at the same time, and the
automation only recognises `issue-*` as a base branch. The branch starts
from `main` and its pull request goes back into `main`. The conventional
commit type is carried by the commits and the pull request title, not by
the branch name.

## Commit messages

[Conventional Commits](https://www.conventionalcommits.org/). The scope is
the package name whenever the change touches `packages/<name>/`:

```
feat(example-pkg): install a default configuration file
fix(example-pkg): install the service file under /usr/lib/systemd/system
build(example-pkg): depend on adduser instead of passwd
```

Repository-level work takes **no scope**: `docs: describe the layout`.

| Type | Effect on the scoped package |
| --- | --- |
| `feat` | minor |
| `fix`, `perf`, `revert` | patch |
| `build` | patch - packaging changed, payload did not |
| `docs`, `test`, `style`, `chore`, `refactor`, `ci` | no release |

`!` after the scope, or a `BREAKING CHANGE:` footer, makes it major.

`build` is the packaging-specific type: the same upstream content,
assembled differently. On Debian that is exactly what the revision after the
hyphen of the version is for.

Keep commits **atomic**: one logical change each, refactoring separate
from behaviour, every commit leaving the repository working. Stage
explicitly (`git add <paths>`), never `git add -A`.

## Packaging rules

**`debian/changelog` is generated.** `dch`, never an editor, so the version
(`<upstream>-<revision>`), the date and the maintainer line always agree.
Its distribution is a real series name: Launchpad rejects any other.

**`debian/copyright` is machine-readable** (DEP-5) and names the real
licence of every file.

**Maintainer scripts print, they do not mutate.** A `postinst` runs as root
on every user's machine without confirmation.

**Upstream is not edited.** Changes to the upstream source are patches in
`debian/patches`, never a modified tarball.

**Builds have no network.** Build servers such as Launchpad's cannot fetch
anything: whatever the build needs, including third-party modules, has to be
in the source package.

**`debian/rules` is executable**, and the source format is `3.0 (quilt)`
unless there is a reason not to.

## Before opening a pull request

```bash
cd packages/<name>
dpkg-parsechangelog -l debian/changelog
uv tool run --from "multicz>=1.10.0" multicz validate --strict   # from the root
debuild -S -us -uc
lintian --pedantic ../<name>_*_source.changes
```

## Merging

`main` takes signed commits through a pull request and the CI checks. A
pull request is merged by adding the `auto-merge` label, which needs write
access: the workflow replays its commits GPG-signed onto `main` once the
required checks passed, then fast-forwards it. The commits of a branch
pushed unsigned, such as by GitHub's "Update branch", are re-signed in
place.

Pull requests are merged by rebase, never squashed to a single title, or
multicz loses the types and scopes it releases from.

The workflows read the `AUTO_MERGE_PAT`, `GPG_PRIVATE_KEY` and
`GPG_PASSPHRASE` secrets and the `GIT_USER_NAME`, `GIT_USER_EMAIL` and
`GIT_SIGNING_KEY` variables of the repository.

## Code of Conduct

By participating you agree to the [Code of Conduct](CODE_OF_CONDUCT.md).
Security issues: follow [SECURITY.md](SECURITY.md).
