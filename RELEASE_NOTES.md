## gdstyle 0.1.7

Editor plugin patch release fixing a startup file-lock warning on
Windows.

### Fixed

- **Godot plugin no longer fails to write `settings.json` on startup.**
  Reported on Windows as "Unable to write to file 'settings.json',
  file in use, locked or lacking permissions", with an orphan
  `settings.json#######.tmp` left behind every project startup.

  Cause: `_load_settings` opened the file for READ, then assigned
  `_auto_lint_check.button_pressed = X` and
  `_auto_format_check.button_pressed = X`. Those assignments fire
  the `toggled` signal synchronously, the handler calls
  `_save_settings`, and `_save_settings` opens the same file for
  WRITE while the READ handle is still alive on the call stack. On
  Windows file locks are exclusive so the WRITE open fails, Godot's
  atomic-save rename orphans the `.tmp`, and the warning surfaces.

  The plugin now uses `set_pressed_no_signal()` for both checkboxes
  during load (the idiomatic Godot fix), wraps the load body in a
  `_loading_settings` guard flag, and adds an explicit `file.close()`
  plus a `push_warning` on open failure so a future regression
  surfaces with a real error instead of silence.

  Reported in [#6](https://github.com/atelico/gdstyle/issues/6).

### How to upgrade

After installing v0.1.7, delete any leftover
`settings.json#######.tmp` files in `addons/gdstyle/` (Godot left
them behind from previous startups because the rename kept failing),
then restart Godot.

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
  rev: v0.1.7
  hooks:
    - id: gdstyle
    - id: gdstyle-fmt
```
or run `pre-commit autoupdate`.

Full documentation, rule list, configuration reference, and the
GDExtension API live in the [README](./README.md).
