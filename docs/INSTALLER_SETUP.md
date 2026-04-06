# Installer-Based Customer Setup

The build pipeline now generates installer artifacts for a setup.exe-first distribution flow.

## What gets produced

After running `exe_builder.py`, the build now creates:

- `dist\Kingfisher Bot\...`
  The staged app files packaged into the installer payload.
- `dist\installers\<customer-slug>\payload\...`
  A copy of the release folder with customer-specific settings applied.
- `dist\installers\<customer-slug>\<OutputBaseFilename>.iss`
  An Inno Setup script that installs the payload into a per-user location.
- `dist\installers\<customer-slug>\output\<OutputBaseFilename>.exe`
  The compiled installer, if `ISCC.exe` is installed.
## GitHub publish behavior

When a GitHub token is available, the EXE build publish step now updates the GitHub updates repo with:

- `AMER-LAPTOP/release/downloads/KingfisherBotSetup.exe` on the `main` branch
  The latest compiled installer for customer installs.
- `bot-results/...` on the configured results branch
  Bot run snapshots uploaded by customer machines when results reporting is enabled.

The old `latest.json`, ZIP package, and GitHub Pages release-page workflow are no longer part of the intended release design.

## Why installs default to LocalAppData

The launcher writes logs, config files, downloaded updates, and version folders under the install root. A `Program Files` install would usually require elevation for later writes, so the installer defaults to:

`{localappdata}\Kingfisher Bot`

## Customer profile

The installer builder reads one of these files:

1. `installer_profile.json` at the repo root, if present
2. `installer_profiles\default.json`

Use `installer_profiles\example_customer.json` as a starting point for customer-specific installers.

Key fields:

- `customer_name`
- `customer_code`
- `app_id`
- `output_base_filename`
- `custom_preferences`

`custom_preferences` is merged into `data\configs\preferences.json` inside the installer payload.

## License behavior

License files are now stored under:

`data\license`

That keeps activation stable across app updates, because the license is no longer tied to the currently-installed version folder.

## Compiling the setup EXE

If Inno Setup 6 is installed and `ISCC.exe` is available, the build compiles the installer automatically.

Supported compiler discovery:

- `INNO_SETUP_COMPILER` environment variable
- `ISCC.exe` on `PATH`
- `C:\Program Files (x86)\Inno Setup 6\ISCC.exe`
- `C:\Program Files\Inno Setup 6\ISCC.exe`

If the compiler is not found, the build still generates the payload and `.iss` script so you can compile it later.
