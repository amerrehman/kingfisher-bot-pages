# Results Reporting

Kingfisher can now summarize each installed bot's trade closures and publish a single snapshot to GitHub for central comparison.

## What it does

- Reads local closure files from `Trades/*_closures.csv`
- Detects customer identity from `configs/customer_install.json`
- Builds a compact snapshot with:
  - customer name/code
  - machine name
  - account number
  - app version
  - closed-trade count
  - total net profit
  - last balance
  - last close timestamp
- Uploads that snapshot to a GitHub repo
- Loads all uploaded snapshots into a clean table from the GUI

## GUI flow

Open the main app and click `Results`.

- `Analyze Local`
  Refreshes the local summary from this machine's `Trades` folder.
- `Upload Snapshot`
  Pushes this machine's latest summary to GitHub.
- `Refresh GitHub`
  Pulls all uploaded snapshots and displays them in the report table.

## Required GitHub setup

Add a `results_reporting` section to `configs/preferences.json`:

```json
{
    "results_reporting": {
        "enabled": "yes",
        "auto_upload_on_stop": "no",
        "repo": "your-org/your-private-results-repo",
        "branch": "main",
        "repo_path": "bot-results",
        "token_env_var": "KINGFISHER_GITHUB_TOKEN"
    }
}
```

Notes:

- Use a private repo if customer/staff performance data is sensitive.
- The token can come from `github_token.txt` in the project root or the configured environment variable.
- Each bot writes one snapshot file under:
  `bot-results/<customer>/<machine>/<account>.json`

## Best practice

- Ship customer/staff builds with `customer_name` and `customer_code` filled in through the installer profile.
- Keep one shared private repo for all snapshots.
- Open `Results` on your admin machine to compare every distributed bot in one table.
