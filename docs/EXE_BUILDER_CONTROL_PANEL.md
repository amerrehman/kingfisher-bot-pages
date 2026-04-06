# EXE Builder Control Panel

The EXE builder now supports a JSON-driven control profile and a themed GUI for selecting exactly which stages should run.

## Layout Design

The control panel is split into four sections:

- `Build Scope`
  Controls versioning, cleanup behavior, and which PyInstaller targets are built.
- `Packaging`
  Controls release layout staging for the installer, setup generation, and artifact archiving.
- `GitHub`
  Controls whether GitHub publishing runs at all and whether the compiled `setup.exe` is uploaded.
- `Runtime`
  Controls generic runtime settings like extra PyInstaller arguments.

## Config File

The config file lives at:

`configs\exe_builder_config.json`

The checked-in profile is installer-centric:

- Packaging:
  - `stage_release_layout = true`
  - `build_installer_artifacts = true`
  - `compile_setup_exe_mode = "yes"`
- GitHub:
  - `publish_updates_to_github = true`
  - `publish_setup_exe = true`

When GitHub publishing is enabled, the builder now updates:

- the compiled `setup.exe` on `main/AMER-LAPTOP/release/downloads`
- `AMER-LAPTOP/running/release_notes.txt`
- `AMER-LAPTOP/testing/release_notes.txt`
- `AMER-LAPTOP/development/release_notes.txt`

The customer-facing site is now split into separate routes:

- `/kingfisher-bot/AMER-LAPTOP/running/`
- `/kingfisher-bot/AMER-LAPTOP/testing/`
- `/kingfisher-bot/AMER-LAPTOP/development/`

The site root forwards to the Running page. Local staging templates live in:

- `pages\index.html`
- `pages\AMER-LAPTOP\running\index.html`
- `pages\AMER-LAPTOP\testing\index.html`
- `pages\AMER-LAPTOP\development\index.html`

For focused work, you can switch targets off. A good uploader-only profile is:

- Build targets:
  - `setup_uploader = true`
  - everything else `false`
- Packaging:
  - `stage_release_layout = false`
  - `build_installer_artifacts = false`
  - `archive_raw_build_artifacts = false`
- GitHub:
  - all `false`

## GUI Entry Point

Run:

`python tools/builder_control_panel.py`

The GUI uses the same CustomTkinter theme as the bot and admin tools.

