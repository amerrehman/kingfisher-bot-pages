# Setup Uploader

The project now includes a standalone GUI uploader for publishing a compiled `setup.exe` to GitHub without running the full EXE build pipeline.

## Entry points

- Script: `tools/setup_uploader_app.py`
- PyInstaller spec: `setup_uploader.spec`

## What it does

- Lets you browse for a local `setup.exe`
- Targets the GitHub updates repo by default:
  - Repo: `amerrehman/kingfisher-bot`
  - Branch: `main`
  - Remote folder: `AMER-LAPTOP/release/downloads`
- Uploads through the GitHub Contents API
- Shows progress while the file is prepared and while the request body is sent

## Token lookup

The uploader uses the same token lookup order as the other GitHub tooling:

1. `github_token.txt` in the project root
2. Environment variable `KINGFISHER_GITHUB_TOKEN`

## Typical use

1. Run `python tools/setup_uploader_app.py`
2. Pick the compiled installer EXE
3. Confirm or adjust the remote file name
4. Click `Upload setup.exe`

If you keep the default repo and branch, the uploaded file will be published under:

`https://raw.githubusercontent.com/amerrehman/kingfisher-bot-pages/main/AMER-LAPTOP/release/downloads/<your-file>.exe`

