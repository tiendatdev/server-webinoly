# Complete Webinoly Guide

## Table of Contents

- [Introduction](#introduction)
- [Installing Webinoly](#installing-webinoly)
- [1. Stack Manager](#1-stack-manager)
  - [Installing Stack Components](#installing-stack-components)
  - [Version Changes](#version-changes)
  - [Removing Components](#removing-components)
- [2. Site Manager](#2-site-manager)
  - [Creating Websites](#creating-websites)
  - [Site Management](#site-management)
  - [SSL Management](#ssl-management)
  - [Cache Management](#cache-management)
  - [Clone and Staging](#clone-and-staging)
  - [URL Management](#url-management)
  - [WordPress Tools](#wordpress-tools)
- [3. Webinoly Manager](#3-webinoly-manager)
  - [System Management](#system-management)
  - [Server Configuration](#server-configuration)
  - [Database Management](#database-management)
  - [Cache Management](#cache-management-1)
  - [Backup & AWS S3](#backup--aws-s3)
  - [Email & SMTP](#email--smtp)
  - [Security](#security)
  - [Custom Cache Rules](#custom-cache-rules)
  - [Advanced Options](#advanced-options)
- [4. Log Manager](#4-log-manager)
  - [Real-time Log Viewing](#real-time-log-viewing)
  - [MySQL Logs](#mysql-logs)
  - [WordPress Debug](#wordpress-debug)
  - [Access Log Management](#access-log-management)
  - [Log Cleanup](#log-cleanup)
- [5. HTTP Authentication Manager](#5-http-authentication-manager)
  - [User Management](#user-management)
  - [WordPress Admin Protection](#wordpress-admin-protection)
  - [IP Whitelist Management](#ip-whitelist-management)
  - [Path Protection](#path-protection)
- [Workflow Examples](#workflow-examples)
  - [Production Server Setup](#production-server-setup)
  - [Development Environment](#development-environment)
  - [Maintenance Tasks](#maintenance-tasks)
  - [Security Hardening](#security-hardening)
- [Troubleshooting Common Issues](#troubleshooting-common-issues)
- [File Locations](#file-locations)
- [Important Notes](#important-notes)

## Introduction
Webinoly is an automated LEMP stack (Linux + Nginx + MySQL/MariaDB + PHP) management toolkit for Ubuntu server. Includes 5 main scripts: `site`, `webinoly`, `stack`, `log`, `httpauth`.

## Installing Webinoly

```bash
# Download and install Webinoly
wget -qO weby qrok.es/wy && sudo bash weby 3

# Verify installation
sudo webinoly -verify
```

---

# 1. STACK MANAGER

## Installing Stack Components

### Complete LEMP Installation
```bash
# Full LEMP stack
sudo stack -lemp

# LEMP with build options
sudo stack -lemp -build=light    # Minimal
sudo stack -lemp -build=basic     # Basic
```

### Individual Component Installation
```bash
sudo stack -nginx         # Nginx web server
sudo stack -php           # PHP-FPM
sudo stack -mysql         # MySQL/MariaDB
sudo stack -mysql=client  # MySQL client only
sudo stack -html          # Alias for nginx
```

### Additional Tools
```bash
sudo stack -letsencrypt   # SSL certificates
sudo stack -backups       # Backup tools
sudo stack -postfix       # Mail server
sudo stack -redis         # Redis cache
sudo stack -memcached     # Memcached
sudo stack -pma           # phpMyAdmin
```

## Version Changes

```bash
# Change PHP version
sudo stack -php-ver=8.1
sudo stack -php-ver=8.2
sudo stack -php-ver=8.3

# Change MySQL/MariaDB version
sudo stack -mysql-ver=10.11
sudo stack -mysql-ver=8.0
```

## Removing Components

### Remove individual components
```bash
sudo stack -nginx -purge           # Ask for confirmation
sudo stack -php -purge=force       # No confirmation
sudo stack -mysql -purge
sudo stack -mysql=keep-data -purge # Keep database data
```

### Remove tools
```bash
sudo stack -letsencrypt -purge
sudo stack -backups -purge
sudo stack -postfix -purge
sudo stack -redis -purge
sudo stack -memcached -purge
sudo stack -pma -purge
```

### Remove everything
```bash
sudo stack -purge-server-all       # Delete all
sudo stack -purge-server-all=force # No confirmation
```

---

# 2. SITE MANAGER

## Creating Websites

### WordPress Sites
```bash
sudo site domain.com -wp                    # Basic WordPress
sudo site domain.com -wp=custom             # Custom WordPress
sudo site domain.com -wp=[true,true,localhost,dbname,dbuser,dbpass,wp_]
sudo site domain.com -wp -subfolder=/blog   # WP in subfolder
```

### Other Site Types
```bash
sudo site domain.com -html          # Static HTML
sudo site domain.com -php           # PHP site
sudo site domain.com -mysql         # MySQL site
sudo site domain.com -empty         # Empty site
sudo site domain.com -empty=blank   # Completely empty site
sudo chown -R www-data:www-data /var/www/
```

### Proxy and Forward
```bash
sudo site domain.com -proxy=http://localhost:3000    # Reverse proxy
sudo site domain.com -parked=maindomain.com          # Parked domain
sudo site domain.com -forward=https://newdomain.com  # Forward domain
```

## Site Management

```bash
sudo site domain.com -on         # Enable site
sudo site domain.com -off        # Disable site
sudo site domain.com -delete     # Delete site
sudo site -delete-all            # Delete all sites
sudo site -list                  # List sites
sudo site domain.com -info       # Site information
```

## SSL Management

```bash
sudo site domain.com -ssl=on                    # Enable SSL
sudo site domain.com -ssl=off                   # Disable SSL
sudo site domain.com -ssl=force-renewal         # Force SSL renewal
sudo site -ssl=renew                           # Renew all SSL
sudo site domain.com -ssl=on -wildcard         # Wildcard SSL
sudo site domain.com -ssl-crt=/path -ssl-key=/path # Custom SSL
```

## Cache Management

```bash
sudo site domain.com -cache=on       # Enable FastCGI cache
sudo site domain.com -cache=off      # Disable cache
sudo site domain.com -cache=purge    # Clear cache
```

## Clone and Staging

```bash
sudo site staging.com -clone-from=production.com
sudo site staging.com -clone-from=prod.com -overwrite=on
sudo site domain.com -multisite-convert    # Convert to WP Multisite
```

## URL Management

```bash
sudo site domain.com -force-redirect=www     # Redirect to www
sudo site domain.com -force-redirect=root    # Redirect to non-www
sudo site domain.com -force-redirect=off     # Disable redirect
```

## WordPress Tools

```bash
sudo site domain.com -replace-content -from=old.com -to=new.com
sudo site domain.com -env=staging           # Set environment
sudo site domain.com -env=production        # Production environment
```

---

# 3. WEBINOLY MANAGER

## System Management

```bash
sudo webinoly -update                # Update Webinoly
sudo webinoly -info                  # Server information
sudo webinoly -version               # Version
sudo webinoly -verify                # Verify integrity
sudo webinoly -verify=critical       # Critical verification
```

## Server Configuration

```bash
sudo webinoly -tools-site=tools.domain.com    # Setup tools site
sudo webinoly -tools-port=8080                # Change tools port
sudo webinoly -default-site=domain.com        # Default site
sudo webinoly -sftp=on                        # SFTP access
sudo webinoly -timezone=Asia/Ho_Chi_Minh      # Set timezone
```

## Database Management

```bash
sudo webinoly -dbpass                     # Show DB passwords
sudo webinoly -mysql-password             # Change MySQL root password
sudo webinoly -mysql-public-access=on     # MySQL remote access
sudo webinoly -db-import                  # Import database
```

## Cache Management

```bash
sudo webinoly -clear-cache                # Xóa tất cả cache
sudo webinoly -clear-cache=redis          # Xóa Redis cache
sudo webinoly -clear-cache=memcache       # Xóa Memcache
sudo webinoly -clear-cache=opcache        # Xóa OPcache
sudo webinoly -clear-cache=fastcgi        # Xóa FastCGI cache
sudo webinoly -clear-cache=domain.com     # Xóa cache của site
sudo webinoly -cache-valid=1h             # Cache validity time
```

## Backup & AWS S3

```bash
sudo webinoly -backup                     # Backup wizard
sudo webinoly -backup=local               # Local backup
sudo webinoly -backup=s3                  # S3 backup
sudo webinoly -backup=local -export       # Export config
sudo webinoly -backup=local -import       # Import config
sudo webinoly -backup -list               # List S3 backups
sudo webinoly -aws-s3-credentials         # Setup S3 credentials
```

## Email & SMTP

```bash
sudo webinoly -smtp                       # Setup SMTP
sudo webinoly -smtp -purge                # Remove SMTP
sudo webinoly -email=admin@domain.com     # Set admin email
```

## Security

```bash
sudo webinoly -blockip=1.2.3.4           # Block IP
sudo webinoly -blockip=off               # Unblock all IPs
```

## Custom Cache Rules

```bash
sudo webinoly -skip-cache="/admin,/api"
sudo webinoly -skip-cookie-cache="logged_in"
sudo webinoly -query-string-cache="page,sort"
sudo webinoly -query-string-never-cache="debug"
```

## Advanced Options

```bash
sudo webinoly -custom-headers             # Custom HTTP headers
sudo webinoly -dynvar=varname -value="test"  # Set dynamic variable
sudo webinoly -uninstall                 # Uninstall Webinoly
```

---

# 4. LOG MANAGER

## Real-time Log Viewing

### Website Logs
```bash
log domain.com                    # Access log
log domain.com -error             # Error log
log domain.com -lines=100         # Last 100 lines
log -error                       # All error logs
log                             # All access logs
```

### System Service Logs
```bash
log -php                         # PHP-FPM logs
log -fpm                         # Alias for PHP
log -mail                        # Mail logs
log -email                       # Alias for mail
log -auth                        # Authentication logs
log -ssh                         # SSH logs
log -syslog                      # System logs
log -le                         # Let's Encrypt logs
```

## MySQL Logs

### MySQL Error Log
```bash
log -mysql                       # MySQL error log
log -mysql=error                 # Same as above
```

### MySQL General Log
```bash
log -mysql=general -enable       # Enable general log
log -mysql=general -disable      # Disable general log
log -mysql=general               # View general log
```

### MySQL Slow Query Log
```bash
log -mysql=slow -enable                      # Enable slow query log
log -mysql=slow -enable -long-query-time=5   # Enable with 5s threshold
log -mysql=slow -disable                     # Disable slow query log
log -mysql=slow                              # View slow query log
```

### MySQL Binary Log
```bash
log -mysql=binary -enable        # Enable binary log
log -mysql=binary -disable       # Disable binary log
log -mysql=binary                # View binary log
```

## WordPress Debug

```bash
log domain.com -wp=on            # Enable WP debug mode
log domain.com -wp=off           # Disable WP debug mode
log domain.com -wp               # View WP debug log
log domain.com -wp=on -display=off   # Debug without display
log domain.com -wp -subfolder=/blog  # WP debug subfolder
```

## Access Log Management

### Global Settings
```bash
log -access-log=on               # Enable access log globally
log -access-log=off              # Disable access log globally
log -only-error=on               # Error log only
log -only-error=off              # Re-enable access log
```

### Per Site Settings
```bash
log domain.com -access-log=on    # Enable access log for site
log domain.com -access-log=off   # Disable access log for site
```

## Log Cleanup

```bash
log -purge=all                   # Delete all .gz logs (ask confirmation)
log -purge=nginx                 # Delete nginx .gz logs
log -purge=mysql                 # Delete mysql .gz logs
log -purge=letsencrypt           # Delete letsencrypt .gz logs
log -purge=redis                 # Delete redis .gz logs
log -purge=force                 # Delete without confirmation
```

---

# 5. HTTP AUTHENTICATION MANAGER

## User Management

### Add Users
```bash
sudo httpauth -add                        # Interactive add
sudo httpauth -add=[username,password]    # Direct add
sudo httpauth domain.com -add             # Add for specific domain
sudo httpauth domain.com -add=[user,pass] # Add user for domain
```

### Delete Users
```bash
sudo httpauth -delete                     # Interactive delete
sudo httpauth -delete=username            # Delete specific user
sudo httpauth domain.com -delete=username # Delete from domain
```

### List Users
```bash
sudo httpauth -list                       # List global users
sudo httpauth domain.com -list            # List domain users
```

## WordPress Admin Protection

### Per Site
```bash
sudo httpauth domain.com -wp-admin=on     # Protect WP admin
sudo httpauth domain.com -wp-admin=off    # Remove protection
sudo httpauth domain.com -wp-admin=on -subfolder=/blog  # Subfolder protection
```

### Global Setting (for new sites)
```bash
sudo httpauth -wp-admin=on                # Enable for new sites
sudo httpauth -wp-admin=off               # Disable for new sites
```

## IP Whitelist Management

### Add to Whitelist
```bash
sudo httpauth -whitelist                  # Interactive add
sudo httpauth -whitelist=1.2.3.4         # Add specific IP
sudo httpauth -whitelist=1.2.3.4,5.6.7.8 # Add multiple IPs
```

### Remove from Whitelist
```bash
sudo httpauth -whitelist=1.2.3.4 -delete # Remove specific IP
sudo httpauth -whitelist=1.2.3.4,5.6.7.8 -delete # Remove multiple
sudo httpauth -whitelist -delete-all      # Remove all (confirm)
sudo httpauth -whitelist -delete-all=force # Remove all (no confirm)
```

### List Whitelist
```bash
sudo httpauth -whitelist -list            # Show all whitelisted IPs
```

## Path Protection

### Protect Paths
```bash
sudo httpauth domain.com -path            # Interactive path protection
sudo httpauth domain.com -path=/admin     # Protect specific path
sudo httpauth domain.com -path=/api -exact # Exact match only
sudo httpauth domain.com -path=/          # Protect entire site
sudo httpauth domain.com -path=/blog -subfolder=/blog # Subfolder path
```

### Remove Path Protection
```bash
sudo httpauth domain.com -path=/admin -delete    # Remove specific path
sudo httpauth domain.com -path=all -delete       # Remove all paths
sudo httpauth domain.com -path -delete-all       # Remove all (confirm)
sudo httpauth domain.com -path -delete-all=force # Remove all (no confirm)
```

### List Protected Paths
```bash
sudo httpauth domain.com -list=protected         # Show protected paths
```

---

# WORKFLOW EXAMPLES

## Production Server Setup

```bash
# 1. Cài đặt stack hoàn chỉnh
sudo stack -lemp
sudo stack -letsencrypt -redis -backups

# 2. Cấu hình server
sudo webinoly -tools-site=tools.domain.com
sudo webinoly -email=admin@domain.com
sudo webinoly -aws-s3-credentials

# 3. Tạo WordPress site với bảo mật
sudo site mysite.com -wp
sudo site mysite.com -ssl=on -cache=on
sudo httpauth mysite.com -wp-admin=on

# 4. Setup backup tự động
sudo webinoly -backup=s3

# 5. Bảo mật tools site
sudo httpauth tools.domain.com -add=[admin,strongpass]
sudo site tools.domain.com -ssl=on
```

## Development Environment

```bash
# 1. Setup lightweight
sudo stack -lemp -build=light
sudo stack -pma

# 2. Tạo staging sites
sudo site staging.mysite.com -clone-from=mysite.com
sudo httpauth staging.mysite.com -path=/
sudo httpauth staging.mysite.com -add=[dev,devpass]

# 3. Debug setup
sudo log staging.mysite.com -wp=on
sudo log -mysql=slow -enable -long-query-time=1
```

## Maintenance Tasks

```bash
# 1. System updates
sudo webinoly -update
sudo webinoly -verify

# 2. Backup everything
sudo webinoly -backup=local -export

# 3. Log cleanup
sudo log -purge=nginx
sudo log -purge=mysql

# 4. Cache cleanup
sudo webinoly -clear-cache=all

# 5. SSL renewals
sudo site -ssl=renew
```

## Security Hardening

```bash
# 1. Enable WP admin protection globally
sudo httpauth -wp-admin=on

# 2. Whitelist office IPs
sudo httpauth -whitelist=203.0.113.5,198.51.100.10

# 3. Protect sensitive paths
sudo httpauth api.domain.com -path=/admin
sudo httpauth api.domain.com -path=/config -exact

# 4. Block malicious IPs
sudo webinoly -blockip=1.2.3.4

# 5. Disable access logs for performance
sudo log -access-log=off
sudo log important-site.com -access-log=on
```

## Troubleshooting Common Issues

### Site returns 403 Forbidden
```bash
# Check HTTP auth
sudo httpauth domain.com -list
sudo httpauth -whitelist -list

# Check permissions
sudo chown -R www-data:www-data /var/www/domain.com/
sudo find /var/www/domain.com/ -type d -exec chmod 755 {} \;
sudo find /var/www/domain.com/ -type f -exec chmod 644 {} \;

# Check logs
sudo log domain.com -error
```

### Database connection issues
```bash
# Check DB credentials
sudo webinoly -dbpass
sudo site domain.com -info

# Test DB connection
mysql -u admin -p

# Check MySQL service
sudo systemctl status mysql
sudo log -mysql=error
```

### SSL certificate issues
```bash
# Check SSL status
sudo site domain.com -info

# Renew certificates
sudo site domain.com -ssl=force-renewal
sudo site -ssl=renew

# Check Let's Encrypt logs
sudo log -le
```

### Performance issues
```bash
# Clear all caches
sudo webinoly -clear-cache=all
sudo site domain.com -cache=purge

# Check slow queries
sudo log -mysql=slow -enable
sudo log -mysql=slow

# Monitor access logs
sudo log domain.com
```

## File Locations

```bash
# Nginx configs
/etc/nginx/sites-available/
/etc/nginx/sites-enabled/

# Website files
/var/www/domain.com/htdocs/

# Webinoly config
/opt/webinoly/webinoly.conf

# Logs
/var/log/nginx/
/var/log/mysql/

# SSL certificates
/etc/letsencrypt/live/domain.com/
```

## Important Notes

- Always backup before major changes
- Test nginx config with `sudo nginx -t`
- Use HTTPS for production sites
- Enable HTTP auth for admin areas
- Monitor logs regularly
- Keep system updated
- Use strong passwords
- Whitelist trusted IPs only

---

This document provides a comprehensive guide to Webinoly. For more details, visit https://webinoly.com
