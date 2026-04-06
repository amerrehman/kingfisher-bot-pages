# Customer/Staff Database

Kingfisher now supports a central customer and staff database so you do not have to keep creating one-off installer profile files by hand.

## Database file

The database lives at:

`configs/customer_staff_db.json`

Each record can represent either a `customer` or a `staff` install target.

Important fields:

- `code`
- `name`
- `type`
- `active`
- `app_id`
- `output_base_filename`
- `updates`
- `custom_preferences`
- `notes`

## Manage records

Use the utility:

```powershell
python tools/customer_db_cli.py list
```

Add or update a record:

```powershell
python tools/customer_db_cli.py upsert --code CLIENT-001 --name "Example Client" --type customer
```

Export one record into a standalone installer profile:

```powershell
python tools/customer_db_cli.py export-profile --code CLIENT-001 --out installer_profile.json
```

## Use the database in the installer builder

Instead of storing the full customer profile in `installer_profile.json`, you can now keep that file very small:

```json
{
    "customer_db_code": "CLIENT-001"
}
```

When the installer builder runs, it will:

1. Load the record from `configs/customer_staff_db.json`
2. Convert it into the installer profile shape
3. Merge any extra keys from `installer_profile.json`

That gives you one clean source of truth for customer and staff installs while preserving the current installer pipeline.

