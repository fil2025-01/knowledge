[Prev](/page-05.md) | [Next](/page-07.md)

To configure Xdebug on your system, you'll need to edit the PHP configuration files directly. Here’s a streamlined guide to updating your Xdebug settings.

-----

### 1\. Find Your Xdebug Configuration (.ini) File

First, locate the correct `.ini` file that PHP uses for Xdebug configuration. Run the following command in your terminal:

```bash
php --ini
```

This will display information about your PHP setup. Look for the file listed under **"Additional .ini files parsed"** that contains "xdebug".

**Example Output:**

```plaintext
Configuration File (php.ini) Path: /usr/local/etc/php/8.2
Loaded Configuration File:         /usr/local/etc/php/8.2/php.ini
Scan for additional .ini files in: /usr/local/etc/php/8.2/conf.d
Additional .ini files parsed:      /usr/local/etc/php/8.2/conf.d/ext-opcache.ini,
                                   /usr/local/etc/php/8.2/conf.d/ext-xdebug.ini
```

In this example, the Xdebug configuration file is `/usr/local/etc/php/8.2/conf.d/ext-xdebug.ini`. Your PHP version might differ.

-----

### 2\. Edit the Xdebug Configuration File

Once you have the file path, open it with a text editor. For instance:

```bash
nano /usr/local/etc/php/8.2/conf.d/ext-xdebug.ini
```

You can now manage your Xdebug settings within this file.

-----

### 3\. Configure Xdebug

Here are the most common configurations.

#### To Enable Step Debugging (for IDEs like PhpStorm or VS Code)

Set the `debug` mode to enable step debugging. Your configuration file should look like this:

```ini
[xdebug]
zend_extension="xdebug.so"
xdebug.mode=debug
xdebug.start_with_request=yes
xdebug.client_host=127.0.0.1
xdebug.client_port=9003
```

  * `zend_extension="xdebug.so"`: Loads the Xdebug extension.
  * `xdebug.mode=debug`: Enables step debugging.
  * `xdebug.start_with_request=yes`: Attempts to connect to your IDE on every request.
  * `xdebug.client_host=127.0.0.1`: The IP address where your IDE is listening (correct for local setups).
  * `xdebug.client_port=9003`: The port your IDE listens on (default for Xdebug 3).

#### To Turn Xdebug Off

For optimal performance, especially in production or when running tests, you can disable Xdebug by changing its mode:

```ini
[xdebug]
zend_extension="xdebug.so"
xdebug.mode=off
```

-----

### 4\. Restart PHP-FPM

After saving your changes, you **must restart the PHP-FPM service** for them to take effect. If you used Homebrew, run:

```bash
brew services restart php
```

If you have a specific PHP version, you may need to specify it (e.g., `brew services restart php@8.2`).

-----

### 5\. Verify the Changes

To confirm that your new settings are active, execute:

```bash
php -i | grep xdebug.mode
```

This command will display the current `xdebug.mode`, reflecting the changes you just made. Your Nginx sites will now use the updated configuration.

[Prev](/page-05.md) | [Next](/page-07.md)