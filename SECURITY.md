# Security Policy

## Supported versions

Security fixes are applied only to the latest state of the `main` branch.

## Reporting a vulnerability

**Please do not open a public issue.** GitHub's
[private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability)
is the preferred channel:

1. Go to the repository's **Security** tab.
2. Click **Report a vulnerability**.
3. Describe the issue with reproduction steps and a suggested mitigation.

If you cannot use GitHub's form, email **goabonga@pm.me** with the same
information. PGP encryption is available on request.

You can expect an acknowledgement within **3 business days**, a triage
assessment within **10 business days**, and a fix or written mitigation
plan before any public disclosure.

## Scope

This repository holds Debian source packages. The parts most relevant to
security are the `Build-Depends` and `Depends` of `debian/control`, the
upstream source and its checksums, the maintainer scripts (`postinst`,
`prerm`, ...) that run as root on the user's machine, and the files a
package installs. Vulnerabilities in the upstream software itself should be
reported upstream, but please let us know so the package can be updated.

Thanks for helping keep the project and its users safe.
