[Prev](https://www.google.com/search?q=/page-04.md) | [Next](https://www.google.com/search?q=/page-06.md)

## Serve Laravel with Nginx on a Custom Domain

This guide will walk you through setting up your Laravel application to be served by Nginx using a custom local domain like `api.dating-backend.test`.

This setup involves four main steps:

1.  **DNS Resolution**: Making your local machine recognize the custom domain.
2.  **Server Configuration**: Configuring Nginx to serve your Laravel app for that domain.
3.  **Activation**: Enabling the new site and restarting Nginx.
4.  **Permissions**: Granting the web server the correct permissions to run your Laravel app.

-----

### Step 1: Update Your Hosts File

First, you need to map the custom domain to your local machine's IP address (`127.0.0.1`) by editing your `hosts` file. This tells your computer to look for `api.dating-backend.test` on your own machine.

1.  Open your terminal and run the following command to edit the file with administrator privileges:
    ```bash
    sudo nano /etc/hosts
    ```
2.  Add this line to the end of the file:
    ```plaintext
    127.0.0.1   api.dating-backend.test
    ```
3.  Save and exit (`Ctrl+X`, then `Y`, then `Enter`).

-----

### Step 2: Configure Nginx

Next, create an Nginx "server block" (also known as a virtual host) to direct requests for your custom domain to your Laravel project's files.

1.  Create a new Nginx configuration file for your site. The location depends on your OS and installation method:

      * **Linux**: `/etc/nginx/sites-available/api.dating-backend.test.conf`
      * **macOS (Homebrew)**: `/opt/homebrew/etc/nginx/servers/api.dating-backend.test.conf`

    Use a text editor like `nano` to create and open the file:

    ```bash
    # Adjust the path based on your system
    sudo nano /etc/nginx/sites-available/api.dating-backend.test.conf
    ```

2.  Paste the following server block configuration into the file.

    ```nginx
    server {
        listen 80;
        server_name api.dating-backend.test;
        root /Users/fil/Fil/dating-backend/public; # <-- IMPORTANT: Set this to your project's public directory

        add_header X-Frame-Options "SAMEORIGIN";
        add_header X-XSS-Protection "1; mode=block";
        add_header X-Content-Type-Options "nosniff";

        index index.php;
        charset utf-8;

        location / {
            try_files $uri $uri/ /index.php?$query_string;
        }

        location = /favicon.ico { access_log off; log_not_found off; }
        location = /robots.txt  { access_log off; log_not_found off; }

        error_page 404 /index.php;

        location ~ \.php$ {
            # IMPORTANT: Adjust this path to your PHP-FPM socket
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
            include fastcgi_params;
        }

        location ~ /\.(?!well-known).* {
            deny all;
        }
    }
    ```

    **Key Configuration Points:**

      * **`root`**: This must point to the `public` directory inside your Laravel project.
      * **`fastcgi_pass`**: This directive forwards PHP requests to your PHP-FPM process. The value may vary depending on your PHP version (e.g., `unix:/var/run/php/php8.2-fpm.sock`) or setup (e.g., using a TCP socket like `127.0.0.1:9000`).

-----

### Step 3: Enable the Site and Restart Nginx

Finally, enable the new site configuration and restart Nginx to apply the changes.

1.  **Enable the site (Linux only)**. This creates a symbolic link from the `sites-available` directory to the `sites-enabled` directory, which Nginx actively reads. This step is not typically required on macOS with Homebrew.

    ```bash
    sudo ln -s /etc/nginx/sites-available/api.dating-backend.test.conf /etc/nginx/sites-enabled/
    ```

2.  **Test your Nginx configuration** to ensure there are no syntax errors:

    ```bash
    sudo nginx -t
    ```

    If it's successful, you'll see a message like `nginx: configuration file /etc/nginx/nginx.conf test is successful`.

3.  If the test is successful, **restart Nginx**:

    ```bash
    # For Linux (systemd)
    sudo systemctl restart nginx

    # For macOS (Homebrew)
    brew services restart nginx

    brew services restart php@8.3
    ```

-----

### Step 4: Set Laravel Permissions

For Laravel to work correctly, the web server (Nginx) needs permission to write to certain directories, primarily for logs, cache, and file uploads.

1.  Navigate to your project's root directory in the terminal:

    ```bash
    cd /Users/fil/Fil/dating-backend
    ```

2.  Change the ownership of the `storage` and `bootstrap/cache` directories to the web server user. The user is typically `www-data` on Debian/Ubuntu systems and `nginx` on CentOS/RHEL.

    ```bash
    # For Debian/Ubuntu
    sudo chown -R www-data:www-data storage bootstrap/cache

    # For CentOS/RHEL
    # sudo chown -R nginx:nginx storage bootstrap/cache

    # For macos
    sudo chown -R $(whoami):staff storage bootstrap/cache
    ```

3.  Set the correct file and directory permissions. Directories should be `775` and files should be `664`. This allows the owner and group to read/write/execute, and others to only read/execute.

    ```bash
    sudo find storage -type d -exec chmod 775 {} \;
    sudo find storage -type f -exec chmod 664 {} \;
    sudo find bootstrap/cache -type d -exec chmod 775 {} \;
    sudo find bootstrap/cache -type f -exec chmod 664 {} \;
    ```