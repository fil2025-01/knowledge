[Prev](/page-04.md)
[Next](/page-06.md)

## Serve Laravel with Nginx on a Custom Domain

This guide will walk you through setting up your Laravel application to be served by Nginx using a custom local domain like `api.dating-backend.test`.

This setup involves two main steps:

1.  **DNS Resolution**: Making your local machine recognize the custom domain.
2.  **Server Configuration**: Configuring Nginx to serve your Laravel app for that domain.

-----

### Step 1: Update Your Hosts File

First, you need to map the custom domain to your local machine's IP address (`127.0.0.1`) by editing your `hosts` file.

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

Next, create an Nginx "server block" (virtual host) to direct requests for your custom domain to your Laravel project's files.

1.  Create a new Nginx configuration file for your site. The location depends on your OS and installation method:

      * **Linux**: `/etc/nginx/sites-available/api.dating-backend.test.conf`
      * **macOS (Homebrew)**: `/opt/homebrew/etc/nginx/servers/api.dating-backend.test.conf`

    Use a text editor like `nano` to create the file:

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
            fastcgi_pass 127.0.0.1;
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

      * **`root`**: Must point to the `public` directory of your Laravel project.
      * **`fastcgi_pass`**: This directive forwards PHP requests to your PHP-FPM process. The path may vary depending on your PHP version (e.g., `php8.1-fpm.sock`) or setup (e.g., `127.0.0.1:9000`).

-----

### Step 3: Enable the Site and Restart Nginx

Finally, enable the new site configuration and restart Nginx to apply the changes.

1.  **Enable the site (Linux only)**. This creates a symbolic link to the `sites-enabled` directory. This step is not typically required on macOS with Homebrew.

    ```bash
    sudo ln -s /etc/nginx/sites-available/api.dating-backend.test.conf /etc/nginx/sites-enabled/
    ```

2.  **Test your Nginx configuration** to ensure there are no syntax errors:

    ```bash
    sudo nginx -t
    ```

3.  If the test is successful, **restart Nginx**:

    ```bash
    # For Linux (systemd)
    sudo systemctl restart nginx

    # For macOS (Homebrew)
    brew services restart nginx
    ```

You should now be able to visit `http://api.dating-backend.test` in your browser and see your Laravel application. ✨