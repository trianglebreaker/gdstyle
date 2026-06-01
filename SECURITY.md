# Security Policy

## Supported versions

gdstyle is in 0.x and we ship fixes against the latest release on
[crates.io](https://crates.io/crates/gdstyle) and the latest tag in this
repository. Older 0.x patch versions are not separately maintained.

## Reporting a vulnerability

If you find a security issue (parser crash on attacker-controlled input,
denial-of-service via crafted GDScript or `.tscn`/`.tres` files, unsafe
file handling under `--unsafe-fix`, anything along those lines), please
report it privately rather than opening a public issue:

- Use GitHub's **Report a vulnerability** button on the *Security* tab
  of this repo (preferred — uses GitHub's private advisory flow), **or**
- Email **piero@atelico.studio** with `[gdstyle security]` in the
  subject line.

Please include:

1. The smallest GDScript / `.tscn` / `.tres` / config snippet that
   reproduces the issue.
2. The gdstyle version (`gdstyle --version`) and platform.
3. What you observed and what you think the impact is.

We'll acknowledge receipt within a few working days, work with you to
confirm and characterise the issue, and coordinate disclosure. There is
no bug bounty programme — gdstyle is a small open-source project — but
we credit reporters in the release notes when the fix ships unless
you'd rather stay anonymous.

## Out of scope

- Lints that produce false positives or false negatives. Open a regular
  GitHub issue for those.
- Behaviour on syntactically invalid GDScript that gdstyle simply
  reports as a diagnostic rather than a crash.
- Anything depending on a malicious local user already having write
  access to your source tree.
