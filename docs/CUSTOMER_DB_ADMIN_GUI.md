# Admin Control Panel

Kingfisher now includes a standalone admin control panel for managing both the customer/staff database and distributed bot results on GitHub.

## Purpose

This tool is separate from the bot GUI. It is meant for you or staff members who manage installs, customer records, deployment metadata, and fleet-wide results.

## Launch

```powershell
python tools/customer_db_admin_app.py
```

## What it does

- `Profiles` tab:
  Pulls the customer/staff database JSON from GitHub, edits records, pushes updates back to GitHub, saves a local snapshot, and previews installer profiles.
- `Results` tab:
  Pulls distributed bot snapshots from GitHub and shows them in one table for customer/staff monitoring.

## Default GitHub location

Profiles:

- Repo: `amerrehman/kingfisher-bot`
- Branch: `main`
- Path: `customer-data/customer_staff_db.json`

Results:

- Repo: `amerrehman/kingfisher-bot`
- Branch: `main`
- Path: `bot-results`

You can change these in the top bar of the admin app if needed.

## Notes

- The tool uses the same GitHub token pattern already used elsewhere in the project.
- This is an admin-side utility and does not open the trading bot UI.
- Customer/staff bot copies can stay stripped down while this separate tool remains your management console.

