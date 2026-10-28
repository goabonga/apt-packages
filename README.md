<h1 align="center">
  <img src="assets/apt-packages.svg" alt="apt-packages" width="120" /><br/>
  apt-packages
</h1>

<p align="center">
  <em>Debian and Ubuntu packages maintained in a single repository.</em>
</p>

<p align="center">
  <a href="https://github.com/goabonga/apt-packages/blob/main/LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License: MIT"/></a>
</p>

## Packages

| Package | Role |
| --- | --- |
| [`poc-hello`](packages/poc-hello) | The `hello` binary of poc-app - prints a greeting |
| [`poc-pinger`](packages/poc-pinger) | The `pinger` binary of poc-app - reports TCP connect latency |

## Versioning

Each package declares itself in `packages/<name>/multicz.component.toml`
and owns its version, changelog and tag, computed by
[multicz](https://github.com/goabonga/multicz) from
[Conventional Commits](https://www.conventionalcommits.org/): the files a
commit touches decide the package, its type decides the release. The
topmost stanza of its `debian/changelog` is the version source, and a stanza
is prepended on every release.

```bash
uv tool run --from "multicz>=1.10.0" multicz plan   # what would be released, and why
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for the workflow, the commit
convention and the packaging rules. By participating you agree to the
[Code of Conduct](CODE_OF_CONDUCT.md).

Security issues: follow the process in [SECURITY.md](SECURITY.md).

## License

Distributed under the [MIT License](LICENSE).
