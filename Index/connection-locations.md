# Secret Locations

Purpose:

Store locations of secrets, never the secrets themselves.

## SQL Credentials

Location:

`D:\MY AI\.env`

Variables:

- DB_SERVER
- DB_DATABASE
- DB_USER
- DB_PASSWORD
- DB_TRUST_SERVER_CERTIFICATE

Legacy/requested aliases may include:

- SQL_SERVER
- SQL_DATABASE
- SQL_ADMIN_USER
- SQL_ADMIN_PASSWORD

## Cloudflare

Location:

`D:\MY AI\.env`

Variables:

- CLOUDFLARE_API_TOKEN
- CLOUDFLARE_ACCOUNT_ID
- CLOUDFLARE_ZONE_ID
- R2_ACCESS_KEY
- R2_SECRET_KEY

## Rules

- Never store passwords in repositories.
- Never store passwords in Morning Meetings.
- Never store passwords in deployment ZIPs.
- Never store passwords in appsettings files.
- Never commit `.env` files to source control.
- AI may use secrets only when explicitly approved and only for the approved task.
