## Running SQL Queries from a File

### Method 1: Using the Command Line 💻

This is the most direct method. It uses input redirection (`<`) to send the contents of your `.sql` file to the `mysql` client.

**Command Syntax:**

```bash
mysql -u [username] -p [database_name] < /path/to/your/file.sql
```

**Breakdown:**

  * `mysql`: The command-line client.
  * `-u [username]`: The MySQL user you're logging in as (e.g., `root`).
  * `-p`: Prompts you to enter the user's password securely.
  * `[database_name]`: The target database for the import.
  * `< /path/to/your/file.sql`: Redirects your SQL file's content as input to the command.

**Example:**
To import `contacts.sql` into a database named `value_chain_db` as the `root` user:

```bash
mysql -u root -p value_chain_db < /Users/fil/Fil/value-chain/contacts.sql
```

After running this, you will be prompted for the `root` user's password.

-----

### Method 2: Inside the MySQL Shell 셸

If you're already logged into the MySQL interactive shell, you can use the `SOURCE` command.

**Steps:**

1.  **Log in to MySQL:**

    ```bash
    mysql -u [username] -p
    ```

2.  **Select the database** you want to use:

    ```sql
    USE [database_name];
    ```

3.  **Run the `SOURCE` command** with the full path to your file.

    ```sql
    SOURCE /path/to/your/file.sql;
    ```

    *You can also use `\.` as a shortcut for `SOURCE`.*

**Example:**

```sql
-- First, log in from your terminal
mysql -u root -p

-- Once you see the mysql> prompt, select your database
USE value_chain_db;

-- Now, run the SOURCE command
SOURCE /Users/fil/Fil/value-chain/contacts.sql;
```

Both methods accomplish the same goal. The **command-line method** is faster for single tasks, while the **MySQL shell method** is useful if you are already working within the client.