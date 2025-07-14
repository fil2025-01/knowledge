[Prev](/page-04.md)
[next](/page-06.md)

### Starting Nginx

This is the main command you'll need. It starts Nginx immediately and sets it to launch automatically at login.

```bash
brew services start nginx
```

After running this, Nginx will be active in the background.

-----

### \#\# Essential Nginx Commands

Here are the key commands for managing the Nginx service:

**To Stop Nginx:**

```bash
brew services stop nginx
```

**To Restart Nginx (Very Important\!):**
You **must** run this command every time you modify a configuration file to apply your changes.

```bash
brew services restart nginx
```

**To Check Nginx Status:**
See if the service is running, along with other services managed by Homebrew.

```bash
brew services list
```

-----

### \#\# Troubleshooting and Tips

**Check Your Configuration First 📝**
Before restarting, always test your configuration files for syntax errors. This prevents Nginx from failing to start.

```bash
nginx -t
```

**Privileged Ports (like port 80) 🔒**
Your setup uses port 80, which normally requires root privileges. `brew services` manages this for you automatically. However, if you ever need to start Nginx manually, you would need to use `sudo`.

  * **Manual Start:** `sudo nginx`
  * **Manual Stop:** `sudo nginx -s stop`

For daily use, sticking with `brew services restart nginx` is the best practice.

**Locating Log Files 🪵**
If you encounter connection issues, the Nginx error logs are the first place to check. Based on your previous configuration, your custom log files are located at:

  * **Access Log:** `/Users/fil/Herd/dating-backend/api.dating-backend.access.log`
  * **Error Log:** `/Users/fil/Herd/dating-backend/api.dating-backend.error.log`

If you are using the default Homebrew logs, you can find them here:

  * **Apple Silicon Mac:** `/opt/homebrew/var/log/nginx/`
  * **Intel Mac:** `/usr/local/var/log/nginx/`