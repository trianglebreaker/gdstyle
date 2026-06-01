## gdstyle 0.1.4

Adds file-scope suppression so class- and file-level rules can be
silenced cleanly.

### Added

- **`# gdstyle:ignore-file` directive** for file-scope suppression.

  ```gdscript
  # gdstyle:ignore-file=quality/max-public-methods,quality/max-class-variables
  class_name OrchestrationFacade
  extends Node
  # ... 25 public methods follow
  ```

  Anchor it at the top of the file by convention, but the parser
  accepts it anywhere. A bare `# gdstyle:ignore-file` (no `=...`)
  suppresses every rule in the file — for generated code or
  third-party drops.

  This is the right tool for the four rules whose diagnostic anchors
  at the class header or line 1 of the file and which previously
  couldn't be silenced without uglifying the signature:
  `quality/max-public-methods`, `quality/max-class-variables`,
  `quality/max-inner-classes`, `quality/max-file-length`.

  Per-line `# gdstyle:ignore` is unchanged and remains the right tool
  for spot exemptions on a single line.

### Documentation

- README's *Inline suppression* section is now *Suppressing
  diagnostics* with a directive/scope table, separate per-line and
  per-file subsections, and a "when to use which" guide.

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
  rev: v0.1.4
  hooks:
    - id: gdstyle
    - id: gdstyle-fmt
```
or run `pre-commit autoupdate`.

Full documentation, rule list, configuration reference, and the
GDExtension API live in the [README](./README.md).
