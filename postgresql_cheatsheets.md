# POSTGRESQL
## INTRODUCTION
PostgreSQL is an open source relational database management system (RDBMS) known for its robustness, compliance with SQL standards and advanced features.
## CONNECTION
| Command | Description |
| :---: | :---: |
| `psql -U postgres` | Connect to PostgreSQL as the `postgres` user |
| `psql -U <user> -d <database>` | Connect to PostgreSQL as a specific user and database |
| `psql -U <user> -d <database> -h <host>` | Connect to PostgreSQL on a specific host |
## CLI
| Command | Description |
| :---: | :---: |
| `\c <database>` | Connect to a specific database |
| `\password <user>` | Change password for a specific user |
| `\l` | List all databases |
| `\d+` | Show detailed information about various database objects |
| `\dt` | List all tables in the current database |
| `\du` | List all users |
| `\df` | List all functions |
| `\dv` | List all views |
| `\dn` | List all schemas |
| `\dp` | List all permissions |
| `\di` | List all indexes |
| `\ds` | List all sequences |
| `\d+` | Show detailed information about various database objects |
| `\q` | Quit psql |
| `\x` | Toggle expanded output |
## BACKUP/RESTORE
| Command | Description |
| :---: | :---: |
| `pg_dump <database> > backup.sql` | Backup a database to a file |
| `psql <database> < backup.sql` | Restore a database from a file |

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗

Credits:

[Christian Lempa GitHub](https://github.com/ChristianLempa)
