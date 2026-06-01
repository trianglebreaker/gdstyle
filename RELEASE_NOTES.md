## gdstyle 0.1.3

Patch release fixing a false positive in `quality/unreachable-code`.

### Fixed

- **`quality/unreachable-code`** no longer flags the closing `)` of a
  multi-line `return` statement as unreachable. The previous
  implementation was line-based and walked forward from each
  `return`/`break`/`continue` looking for any same-indent non-blank
  line. The closing `)` of `return floori(\n\t\t1.2\n\t)` lands at the
  same indent as `return`, so it was wrongly reported.

  The rule now tracks open-bracket depth (`(` `[` `{`) and backslash
  continuation across lines, masking delimiters inside string literals
  and trailing `#` comments. While a `return` statement is still
  syntactically open, subsequent lines are treated as continuation of
  that same statement. True positives — actual code at the same indent
  AFTER the statement closes — are still flagged.

  Reported in [#3](https://github.com/atelico/gdstyle/issues/3).

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
  rev: v0.1.3
  hooks:
    - id: gdstyle
    - id: gdstyle-fmt
```
or run `pre-commit autoupdate`.

Full documentation, rule list, configuration reference, and the
GDExtension API live in the [README](./README.md).
