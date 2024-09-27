# art-gallery-night

## Setting up PostgreSQL Database on a New Machine

To recreate the PostgreSQL database structure on a new machine, follow these steps:

### 1. Install PostgreSQL

Ensure that PostgreSQL is installed and running on the new machine.

### 2. Create the Database

Connect to PostgreSQL as the default `postgres` user and create a new database:

```bash
psql -U postgres
CREATE DATABASE nodelogin;
