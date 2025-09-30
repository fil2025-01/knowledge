[Prev](/page-05.md) | [Next](/page-07.md)

To configure Xdebug on your system, you'll need to install the extension and then edit the PHP configuration files directly. Here’s a streamlined guide to getting started.

-----

### **New Info:** 1. Install Xdebug

Before you can configure Xdebug, you need to install it. The standard method for PHP installations managed by Homebrew is to use **PECL** (PHP Extension Community Library).

1.  **Run the PECL install command** in your terminal:

    ```bash
    pecl install xdebug
    ```

2.  **Note the path to `xdebug.so`**. After the installation is complete, PECL will output the full path to the compiled extension file. This is very important.

    **Example Output:**

    ```plaintext
    ...
    Build process completed successfully
    Installing '/usr/local/lib/php/pecl/20220829/xdebug.so'
    install ok: channel://pecl.php.net/xdebug-3.1.5
    ```

    Copy the full path (e.g., `/usr/local/lib/php/pecl/20220829/xdebug.so`) from your terminal. You will need it for the `zend_extension` setting in the configuration step.

-----

### 2\. Find Your Xdebug Configuration (.ini) File

Once installed, you need to locate the correct `.ini` file that PHP uses for Xdebug configuration. Run the following command in your terminal:

```bash
php --ini
```

This will display information about your PHP setup. Look for the file listed under **"Additional .ini files parsed"** that contains "xdebug". If it doesn't exist, you can create a new file named `ext-xdebug.ini` inside the directory listed for "Scan for additional .ini files in:".

**Example Output:**

```plaintext
Configuration File (php.ini) Path: /usr/local/etc/php/8.2
Loaded Configuration File:         /usr/local/etc/php/8.2/php.ini
Scan for additional .ini files in: /usr/local/etc/php/8.2/conf.d
Additional .ini files parsed:      /usr/local/etc/php/8.2/conf.d/ext-opcache.ini,
                                   /usr/local/etc/php/8.2/conf.d/ext-xdebug.ini
```

In this example, the Xdebug configuration file is `/usr/local/etc/php/8.2/conf.d/ext-xdebug.ini`.

-----

### 3\. Edit the Xdebug Configuration File

Once you have the file path, open it with a text editor. For instance:

```bash
nano /usr/local/etc/php/8.2/conf.d/ext-xdebug.ini
```

You can now manage your Xdebug settings within this file.

-----

### 4\. Configure Xdebug

Here are the most common configurations.

#### To Enable Step Debugging (for IDEs like PhpStorm or VS Code)

Set the `debug` mode to enable step debugging. Your configuration file should look like this, using the full path from Step 1.

```ini
[xdebug]
zend_extension="/usr/local/lib/php/pecl/20220829/xdebug.so"
xdebug.mode=debug
xdebug.start_with_request=yes
xdebug.client_host=127.0.0.1
xdebug.client_port=9003
```

  * `zend_extension`: **Crucially, use the full path** you copied after running the `pecl install` command.
  * `xdebug.mode=debug`: Enables step debugging.
  * `xdebug.start_with_request=yes`: Attempts to connect to your IDE on every request.
  * `xdebug.client_host=127.0.0.1`: The IP address where your IDE is listening (correct for local setups).
  * `xdebug.client_port=9003`: The port your IDE listens on (default for Xdebug 3).

#### To Turn Xdebug Off

For optimal performance, especially in production or when running tests, you can disable Xdebug by changing its mode:

```ini
[xdebug]
zend_extension="/usr/local/lib/php/pecl/20220829/xdebug.so"
xdebug.mode=off
```

-----

### 5\. Understanding All Xdebug Modes

Xdebug is more than just a step debugger. You can enable multiple modes at once by separating them with a comma.

```ini
# Example: Enable development helpers and step debugging simultaneously
xdebug.mode=develop,debug
```

Here are the available modes:

  * **develop**: Provides enhanced development aids, such as an improved `var_dump()` function and more detailed error reporting. It has very low overhead and is great to leave on during development.
  * **profile**: Enables the performance profiler, which helps you identify bottlenecks in your code. It generates a "cachegrind" file for analysis in tools like QCacheGrind or PhpStorm.
  * **trace**: Creates a detailed log of every function call, including arguments, variables, and return values. This is useful for deep analysis but can generate very large files.
  * **coverage**: Used for code coverage analysis, typically with a testing framework like PHPUnit, to see which parts of your code are executed by your tests.

-----

### 6\. Advanced Configuration & Troubleshooting

#### More Efficient Debug Triggering

Using `xdebug.start_with_request=yes` is easy but can slow down your site because it tries to debug every page load. A more professional approach is to use a trigger.

```ini
xdebug.start_with_request=trigger
```

When this is set, Xdebug will only activate when it receives a specific trigger, which is most often managed with a **browser extension**.

  * Install a browser extension like "Xdebug helper" for Chrome or Firefox.
  * Enable the "Debug" mode in the extension's icon.
  * Now, Xdebug will only attempt a connection when the extension is active for a specific tab, leaving other requests to run at full speed.

#### Setting an Output Directory for Profiles and Traces

When you use `profile` or `trace` mode, Xdebug needs a place to save its output files. You can specify this with `xdebug.output_dir`.

```ini
# Save profiler and trace files to a temporary directory
xdebug.output_dir = "/tmp/xdebug"
```

**Note:** Make sure the directory you specify exists and that PHP has permission to write to it.

#### Troubleshooting with a Log File

If your IDE isn't connecting to Xdebug, it can be hard to know why. Xdebug can create a log file that details every connection attempt and error. This is incredibly useful for troubleshooting.

```ini
# Create a log file to diagnose connection problems
xdebug.log = "/tmp/xdebug.log"
```

-----

### 7\. Restart PHP-FPM

After saving your changes, you **must restart the PHP-FPM service** for them to take effect. If you used Homebrew, run:

```bash
brew services restart php
```

If you have a specific PHP version, you may need to specify it (e.g., `brew services restart php@8.2`).

-----

### 8\. Verify the Changes

To confirm that your new settings are active, execute:

```bash
php -i | grep xdebug.mode
```

This command will display the current `xdebug.mode`, reflecting the changes you just made. If you set multiple modes, you will see them listed (e.g., `develop,debug`). Your Nginx sites will now use the updated configuration.

[Prev](/page-05.md) | [Next](/page-07.md)