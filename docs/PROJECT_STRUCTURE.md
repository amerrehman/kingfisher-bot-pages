# Project Structure

This workspace follows a clearer split between source code, secondary tools, scripts, build assets, docs, and runtime-generated files.

## Main areas

- `src/kingfisher_bot/`
  Main application code for the bot, builder, installer, results reporting, customer database helpers, and shared utilities.
- `tm/`
  Trading-management engine modules used by the bot runtime.
- `tools/`
  Secondary Python entrypoints for admin utilities, setup uploading, and builder control helpers.
- `scripts/`
  PowerShell helpers for backups and environment setup.
- `docs/`
  Human-facing documentation, setup notes, admin docs, and help-site content.
- `Images/`
  Shared visual assets and application icons.
- `configs/`
  Local configuration files and the central customer/staff database snapshot.
- `installer_profiles/`
  Installer profile templates and examples.

## Root entrypoints

These root files are intentionally small wrappers or primary entrypoints:

- `bot_gui.py`
- `launcher.py`
- `exe_builder.py`

Compatibility shims also remain at the root for older commands, but the canonical locations are now under `tools/` and `scripts/`.

## Canonical utility entrypoints

- `tools/customer_db_admin_app.py`
- `tools/customer_db_cli.py`
- `tools/builder_control_panel.py`
- `tools/setup_uploader_app.py`
- `scripts/daily_backupper.ps1`
- `scripts/github_publish_token.ps1`
- `scripts/backup_planned_changes.ps1`

## Build assets

- `bot.spec`
- `launcher.spec`
- `admin.spec`
- `setup_uploader.spec`

These are PyInstaller specs for the main app and support utilities.

## Runtime-generated folders

These are operational folders and should not be treated as source:

- `build/`
- `dist/`
- `Logs/`
- `Trades/`
- `Daily Backups/`
- `LagProbeLogs/`

## Legacy content

- `docs/legacy/`
  Old help/reference material retained only for compatibility or historical reference.
