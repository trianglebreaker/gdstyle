## gdstyle 0.1.5

Patch release fixing a formatter explosion on `@abstract` (and any
other unrecognised class-level annotation).

### Fixed

- **`gdstyle fmt` no longer duplicates `@abstract` / `class_name`
  blocks.** When an unrecognised top-level annotation
  (`@abstract` in Godot 4.6+, also `@experimental` and `@deprecated`)
  appeared above `class_name`/`extends` with a function later in the
  file, the parser held it in pending state and silently attached it
  to that function. The formatter then treated lines 1..N as part of
  the function's "block" and re-emitted them on every pass — the
  multi-pass loop could blow the file up to 30+ duplicate
  `@abstract\nclass_name Pickup\n` chunks before stabilising.

  The parser now flushes pending unknown annotations into a new
  class-level `ClassAnnotation` node at six boundary points where the
  pending annotation can't legitimately attach:
  `class_name`, `extends`, `signal`, `enum`, `const`, inner `class`,
  and end-of-file.

  Function-level `@abstract` (`@abstract\nfunc to_implement():`) still
  attaches to its method as before — abstract method declarations are
  unaffected.

  Reported in [#4](https://github.com/atelico/gdstyle/issues/4).

### Behaviour notes

- The fix is structural (by member kind), not name-based. Any future
  Godot annotation that lands at the top of a file will sort with
  `@tool`/`@icon`/`@static_unload` automatically without a code change.
- A trailing top-level annotation at EOF with no following declaration
  used to be silently dropped; it now survives a round-trip through
  `gdstyle fmt`.
- A leading annotation directly above an inner `class Inner:` is
  attributed to the OUTER class (the AST doesn't yet model
  inner-class annotations). If that becomes a practical issue, file
  one and we'll wire annotation slots through inner-class parsing.

### Install

CLI from crates.io:
```bash
cargo install gdstyle
```

Or grab a prebuilt binary from this release page, drop it on your
`PATH`, and run `gdstyle` in your project directory.

For the Godot editor plugin: download `gdstyle-godot-plugin.zip` from
this release, extract the `addons/gdstyle/` folder into your Godot
project, then enable the plugin in *Project > Project Settings >
Plugins*.

For the [pre-commit](https://pre-commit.com) framework, bump your
config to:
```yaml
- repo: https://github.com/atelico/gdstyle
  rev: v0.1.5
  hooks:
    - id: gdstyle
    - id: gdstyle-fmt
```
or run `pre-commit autoupdate`.

Full documentation, rule list, configuration reference, and the
GDExtension API live in the [README](./README.md).
