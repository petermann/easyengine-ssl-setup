# EasyEngine: Creating a Site without a Valid Certificate and Updating to Let's Encrypt Certificate

## Introduction

This tutorial guides you through creating a WordPress site with EasyEngine using an initial self-signed certificate and without a wildcard SSL certificate. This approach is designed to avoid the error: `Error: Update from wildcard SSL to normal SSL is not supported yet.` By following these steps, you will be able to update your site to a valid Let's Encrypt certificate in the future.

**Note:** Replace `www.domain.com` with the URL of the site you want to create.

### Initial Setup

1. **Create a simple HTML site with a self-signed certificate:**
   ```bash
   ee site create www.domain.com --type=html --ssl=self --skip-status-check
   ```

2. **Copy the site certificates from `/opt/easyengine/services/nginx-proxy/certs` to `/root/certs/`:**
   ```bash
   mkdir -p /root/certs/
   cp /opt/easyengine/services/nginx-proxy/certs/www.domain.com.* /root/certs/
   ```

3. **Delete the HTML site:**
   ```bash
   ee site delete www.domain.com --yes
   ```

4. **Create a new WordPress site using custom certificates:**
   ```bash
   ee site create www.domain.com --type=wp --ssl=custom --ssl-key='/root/certs/www.domain.com.key' --ssl-crt='/root/certs/www.domain.com.crt' --skip-status-check
   ```

5. **Remove the certificates from `/root/certs/` after creating the WordPress site:**
   ```bash
   rm -rf /root/certs/www.domain.com.*
   ```

### Updating to a Valid Certificate

When necessary to update to a valid certificate:

1. **Backup the current certificates (optional, for recovery purposes):**
   ```bash
   mkdir -p /root/certs/
   cp /opt/easyengine/services/nginx-proxy/certs/www.domain.com.* /root/certs/
   ```

2. **Disable the current SSL configuration:**
   ```bash
   ee site update www.domain.com --ssl=off
   ```

3. **Delete the existing certificates:**
   ```bash
   rm -rf /opt/easyengine/services/nginx-proxy/certs/www.domain.com.*
   ```

4. **Update the site with a new valid Let's Encrypt certificate:**
   ```bash
   ee site update www.domain.com --ssl=le
   ```

### Resources

- [EasyEngine Official Website](https://easyengine.io)
- [EasyEngine Site Commands Documentation](https://easyengine.io/commands/site/)

---

This README.md file provides step-by-step instructions for setting up a WordPress site with EasyEngine, starting with a simple HTML site and transitioning to a WordPress site with custom certificates. This setup avoids the wildcard SSL error and prepares the site for future updates with a valid Let's Encrypt certificate. For more details on EasyEngine commands, visit the [EasyEngine Site Commands Documentation](https://easyengine.io/commands/site/).