# art-gallery-night

## Setting up PostgreSQL Database on a New Machine

To recreate the PostgreSQL database structure on a new machine, follow these steps:

### 1. Install PostgreSQL

Ensure that PostgreSQL is installed and running on the new machine.
 
 `-U` stands for user and `-d` stands for database. 

Connect to PostgreSQL as the default `postgres`. 
Enter this in your cmd/terminal.

```bash
psql -U postgres -d postgres
```

### 2. Create a New User
Change the `username` to your name or anything you like.

The default password is `password` you can change it. Write password inside single quotes.

```sql
CREATE USER username WITH PASSWORD 'password' CREATEDB;
```
You can check by using:
```bash
\du
```
Exit from psql(PostgreSQL)
```bash
\q
```

### 3. Connect to newly created database

Connect to PostgreSQL as the newly created user and create a new database:

Change `username`
```bash
psql -U username -d postgres
```

#### Enter `password` if not change in STAGE 2;

### 4. Create new database
```bash
CREATE DATABASE nodelogin;
```
### 5. Connect to the Database

After creating the database, switch to the new database:
```bash
\c nodelogin
```
### 6. Create the Tables

Create the `users` table:

```sql
CREATE TABLE users (
    id BIGINT NOT NULL PRIMARY KEY DEFAULT nextval('users_id_seq'),
    name VARCHAR(200) NOT NULL,
    email VARCHAR(200) NOT NULL UNIQUE,
    password VARCHAR(200) NOT NULL,
    reset_token VARCHAR(255),
    reset_token_expiry TIMESTAMP
);
```

Create the sequence for `users_id_seq`:
```sql
CREATE SEQUENCE users_id_seq;
```

Create the `images` table:

```sql
CREATE TABLE images (
    id SERIAL NOT NULL PRIMARY KEY,
    public_id VARCHAR(255) NOT NULL,
    url VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT now(),
    user_id INTEGER NOT NULL,
    CONSTRAINT fk_user FOREIGN KEY(user_id) REFERENCES users(id) ON DELETE CASCADE
);
```
Create the sequence for `images_id_seq`:
```sql
CREATE SEQUENCE images_id_seq;
```
### 7. Verify the Structure

To verify the structure of the tables:
```bash
\d users
\d images
```
### 8. Set Access Privileges (Optional)

Grant privileges if needed:

Change `username`
```sql
GRANT ALL PRIVILEGES ON DATABASE nodelogin TO username;
```

### Next time If you login psql through terminal use this command to connect to database

Change the `username`
```bash
psql -U username -d nodelogin
```

