# art-gallery-night

## Setting up PostgreSQL Database on a New Machine

To recreate the PostgreSQL database structure on a new machine, follow these steps:

### 1. Install PostgreSQL

Ensure that PostgreSQL is installed and running on the new machine.
 
 `-U` stands for user and `-d` stands for database. 

Connect to PostgreSQL as the default `postgres`. 

#### Enter this in your cmd/terminal.

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

#### Enter password as `password` if not change in STAGE 2;

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
    id integer NOT NULL DEFAULT nextval('images_id_seq'::regclass),
    public_id character varying(255) NOT NULL,
    url character varying(255) NOT NULL,
    created_at timestamp without time zone DEFAULT now(),
    user_id integer NOT NULL,
    title character varying(255) NOT NULL DEFAULT 'Untitled',
    year character varying(10),
    artist character varying(255),
    technique character varying(255),
    genre character varying(100),
    PRIMARY KEY (id),
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

```
Create the sequence for `images_id_seq`:
```sql
CREATE SEQUENCE images_id_seq;
```

### 7. `exhibitions` Table
```sql
CREATE TABLE public.exhibitions (
    id serial PRIMARY KEY,
    name character varying(100),
    image_url text,
    description text,
    user_id integer,
    view_exhibition_url character varying(255)
);
```
### 8. `user_exhibitions` Table
```sql
CREATE TABLE public.user_exhibitions (
    id serial PRIMARY KEY,
    name character varying(255) NOT NULL,
    description text,
    image_url character varying(255),
    user_id integer,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    view_exhibition_url character varying(255),
    CONSTRAINT user_exhibitions_user_id_fkey FOREIGN KEY (user_id)
        REFERENCES public.users (id) ON DELETE CASCADE
);
```

### 9. Verify the Structure

To verify the structure of the tables:
```bash
\d users
\d images
```

### 10. Set Access Privileges (Optional)

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

### Additional commands

To `CLEAR` cmd/terminal in psql, use:
```sql
/! cls;
```
To `QUIT` from psql to terminal
```sql
\q
```
To `DELETE users` from database
In below example deleting users 1 and 2
```sql
DELETE FROM users WHERE id IN (1, 2);
```
To `DELETE images` from database
In below example deleting images 1 and 2
```sql
DELETE FROM images WHERE id IN (1, 2);
```

To reset the sequence/id if some user is deleted manually through cmd

change RESTART WITH `1` as you like
```sql
ALTER SEQUENCE users_id_seq RESTART WITH 1;
```

To reset the sequence/id if some images is deleted manually through cmd

change RESTART WITH `1` as you like
```sql
ALTER SEQUENCE images_id_seq RESTART WITH 1;
```
