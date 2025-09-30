[Next](/page-03.md)

# MySQL Command Cheatsheet

A quick reference for common MySQL and Laravel database operations.

## User Management

### 1\. Create User

To create a new user, use the following SQL command.

```sql
CREATE USER 'fil'@'%' IDENTIFIED BY 'password';
```

### 2\. Grant Privileges

Grant all permissions on a specific database to the newly created user.

```sql
GRANT ALL PRIVILEGES ON `database_name`.* TO 'fil'@'%';
```

### 3\. Flush Privileges

Apply the changes by reloading the grant tables.

```sql
FLUSH PRIVILEGES;
```

-----

## Exporting Data with `mysqldump`

The `mysqldump` utility creates a `.sql` file containing the commands to recreate a database or specific tables.

### Command Syntax

```bash
mysqldump -u [username] -p [database_name] [table_name] > [output_file.sql]
```

  * `-u [username]`: The MySQL username (e.g., `root`).
  * `-p`: Prompts for the user's password securely.
  * `[database_name]`: The source database.
  * `[table_name]`: (Optional) The specific table to dump. If omitted, the entire database is dumped.
  * `>`: Redirects the output to a file.

### Examples

**Dump a full database:**

```bash
mysqldump -u root -p database_name > database.sql
```

**Dump a single table (`users`):**

```bash
mysqldump -u root -p database_name users > users.sql
```

> **Note:** You can include the password directly in the command (`-p'password'`), but this is less secure as it exposes the password in your command history.

-----

## Importing Data from a `.sql` File

### Method 1: Using the Command Line

This method uses input redirection (`<`) to execute the SQL commands from your file.

**Syntax:**

```bash
mysql -u [username] -p [database_name] < /path/to/your/file.sql
```

**Example:**

```bash
mysql -u root -p value_chain_db < /Users/fil/Fil/value-chain/contacts.sql
```

### Method 2: Inside the MySQL Shell

If you're already logged into the MySQL interactive shell, use the `SOURCE` command.

**Steps:**

1.  **Log in to MySQL:**

    ```bash
    mysql -u root -p
    ```

2.  **Select the database:**

    ```sql
    USE value_chain_db;
    ```

3.  **Run the `SOURCE` command:**

    ```sql
    SOURCE /Users/fil/Fil/value-chain/contacts.sql;
    ```

    > **Tip:** You can use `\.` as a shortcut for `SOURCE`.

-----

## Database Backups in Laravel (Recommended)

For Laravel projects, using a dedicated package like `spatie/laravel-backup` is the industry standard.

### Why Use `spatie/laravel-backup`?

  * **Secure**: Pulls credentials directly from your `.env` file.
  * **Flexible**: Back up the entire database or specify tables.
  * **Integrated**: Run backups with a simple `artisan` command.
  * **Schedulable**: Easily schedule nightly backups with Laravel's task scheduler.
  * **Storage Options**: Supports local disk, Amazon S3, Dropbox, and more.

### How to Use It

1.  **Install the package:**

    ```bash
    composer require spatie/laravel-backup
    ```

2.  **Publish the configuration file (optional):**

    ```bash
    php artisan vendor:publish --provider="Spatie\Backup\BackupServiceProvider"
    ```

3.  **Run a backup:**
    To back up only the `users` table, for example:

    ```bash
    php artisan backup:run --only-tables=users
    ```

The package will create a zip archive containing the `.sql` dump in the storage location defined in your `config/backup.php` file.

[Main](/main.md) | [Next](/page-03.md)