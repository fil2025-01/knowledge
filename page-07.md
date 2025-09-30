[Prev](/page-06.md) | [Next](/page-08.md)

### \#\# 1. Using a SQL Command 🐘

The most fundamental way is with the `CREATE DATABASE` SQL statement. You can run this from any SQL client (like psql, DBeaver, or TablePlus) connected to your PostgreSQL server.

```sql
CREATE DATABASE your_new_database_name;
```

To use the `psql` command-line tool, first connect to the default `postgres` database:

```bash
psql -U your_username -d postgres
```

Then, run the SQL command at the prompt.

-----

### \#\# 2. Using the `createdb` Command-Line Tool

PostgreSQL includes a handy command-line utility called **`createdb`**, which is a simple wrapper for the SQL command. You can run it directly from your terminal.

```bash
createdb your_new_database_name
```

If you need to create the database as a different user, use the `-U` flag:

```bash
createdb -U your_username your_new_database_name
```

-----

### \#\# 3. The Laravel Way: Let Migrations Do the Work 🚀

For a Laravel project, the easiest method is built right into the framework. Laravel is smart enough to create the database for you when you run your migrations.

First, ensure your `.env` file is configured with your new database credentials:

```ini
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=your_new_database_name
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

[Prev](/page-06.md) | [Next](/page-08.md)