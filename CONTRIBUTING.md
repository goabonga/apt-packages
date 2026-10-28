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

## Commit messages

[Conventional Commits](https://www.conventionalcommits.org/). The scope is
the package name whenever the change touches `packages/<name>/`:

```
feat(example-pkg): install a default configuration file
fix(example-pkg): install the service file under /usr/lib/systemd/system
build(example-pkg): depend on adduser instead of passwd
```

Repository-level work takes **no scope**: `docs: describe the layout`.

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
debuild -S -us -uc
lintian --pedantic ../<name>_*_source.changes
```

## Code of Conduct

By participating you agree to the [Code of Conduct](CODE_OF_CONDUCT.md).
Security issues: follow [SECURITY.md](SECURITY.md).
