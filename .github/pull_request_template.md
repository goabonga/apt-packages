## Description

<!-- What does this change, and why? -->

## Type

<!-- Check the one that applies. The scope of your commits must be the
     package name whenever you touched packages/<name>/. -->

- [ ] `feat(<package>)` - new capability, new file installed
- [ ] `fix(<package>)` - the package was wrong and now is not
- [ ] `build(<package>)` - packaging changed, payload did not
- [ ] `docs` / `ci` / `chore` / `refactor` / `test` - no package change

## Related issues

<!-- Closes #123 -->

## Checklist

- [ ] Commits follow [Conventional Commits](https://www.conventionalcommits.org/), **scoped to the package name**
- [ ] Branch is up to date with `main`

If you touched a package:

- [ ] `debian/changelog` written with `dch`, for a real series
- [ ] `debian/copyright` is in the DEP-5 format and still true
- [ ] `lintian --pedantic` is clean on the source package
- [ ] The Debian revision is bumped (or the upstream version, for an upstream move)
