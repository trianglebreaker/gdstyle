<!--
Thanks for sending a PR. A few things that help us review faster:

- Keep one change per PR. Bug fixes, refactors, and new features
  should live in separate PRs, even if you're doing them all in the
  same session.
- If your PR fixes an open issue, write `Closes #N` somewhere in the
  description so GitHub links them.
- For new lint rules: include at least one positive test, one
  negative test, and (if applicable) one autofix test.
- For formatter changes: make sure the existing examples still
  format cleanly (`cargo build --release && ./target/release/gdstyle fmt examples/`).

Delete this comment block before submitting.
-->

## Summary

<!-- One or two sentences on what this PR changes and why. -->

## Type

<!-- Tick the ones that apply (with an x: `[x]`). -->

- [ ] Bug fix (`fix:`) — non-breaking change that resolves an issue
- [ ] New feature (`feat:`) — non-breaking change that adds functionality
- [ ] New lint rule (`feat:`)
- [ ] Refactor (`refactor:`) — no behaviour change
- [ ] Performance (`perf:`)
- [ ] Documentation (`docs:`)
- [ ] CI / build (`ci:` / `build:`)
- [ ] Breaking change

## Test plan

<!--
What did you run to convince yourself this works?

- `cargo test` (please paste the summary line — `test result: ok. N passed`)
- `cargo clippy --release`
- For formatter / lint changes: run on a real Godot project or on `examples/`
- For rule changes: list the new tests added
-->

- [ ] `cargo test` passes locally
- [ ] `cargo clippy --release` is clean
- [ ] If this touches a rule or the formatter: relevant regression tests added

## Linked issues

<!-- e.g. Closes #3 -->
