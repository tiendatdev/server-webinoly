# Hướng Dẫn Webinoly Đầy Đủ

## Mục Lục

- [Giới thiệu](#giới-thiệu)
- [Cài Đặt Webinoly](#cài-đặt-webinoly)
- [1. Stack Manager](#1-stack-manager)
  - [Cài Đặt Stack Components](#cài-đặt-stack-components)
  - [Thay Đổi Phiên Bản](#thay-đổi-phiên-bản)
  - [Gỡ Bỏ Components](#gỡ-bỏ-components)
- [2. Site Manager](#2-site-manager)
  - [Tạo Website](#tạo-website)
  - [Quản Lý Site](#quản-lý-site)
  - [SSL Management](#ssl-management)
  - [Cache Management](#cache-management)
  - [Clone và Staging](#clone-và-staging)
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
  - [Xem Logs Real-time](#xem-logs-real-time)
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

## Giới thiệu
Webinoly là bộ công cụ quản lý LEMP stack (Linux + Nginx + MySQL/MariaDB + PHP) tự động cho Ubuntu server. Bao gồm 5 script chính: `site`, `webinoly`, `stack`, `log`, `httpauth`.

## Cài Đặt Webinoly

```bash
# Tải và cài đặt Webinoly
wget -qO weby qrok.es/wy && sudo bash weby 3

# Kiểm tra cài đặt
sudo webinoly -verify
```

---

# 1. STACK MANAGER

## Cài Đặt Stack Components

### Cài đặt hoàn chỉnh LEMP
```bash
# LEMP stack đầy đủ
sudo stack -lemp

# LEMP với build options
sudo stack -lemp -build=light    # Tối giản
sudo stack -lemp -build=basic     # Cơ bản
```

### Cài đặt từng thành phần
```bash
sudo stack -nginx         # Nginx web server
sudo stack -php           # PHP-FPM
sudo stack -mysql         # MySQL/MariaDB
sudo stack -mysql=client  # Chỉ MySQL client
sudo stack -html          # Alias cho nginx
```

### Công cụ bổ sung
```bash
sudo stack -letsencrypt   # SSL certificates
sudo stack -backups       # Backup tools
sudo stack -postfix       # Mail server
sudo stack -redis         # Redis cache
sudo stack -memcached     # Memcached
sudo stack -pma           # phpMyAdmin
```

## Thay Đổi Phiên Bản

```bash
# Thay đổi PHP version
sudo stack -php-ver=8.1
sudo stack -php-ver=8.2
sudo stack -php-ver=8.3

# Thay đổi MySQL/MariaDB version
sudo stack -mysql-ver=10.11
sudo stack -mysql-ver=8.0
```

## Gỡ Bỏ Components

### Gỡ từng thành phần
```bash
sudo stack -nginx -purge           # Hỏi xác nhận
sudo stack -php -purge=force       # Không hỏi
sudo stack -mysql -purge
sudo stack -mysql=keep-data -purge # Giữ database data
```

### Gỡ công cụ
```bash
sudo stack -letsencrypt -purge
sudo stack -backups -purge
sudo stack -postfix -purge
sudo stack -redis -purge
sudo stack -memcached -purge
sudo stack -pma -purge
```

### Gỡ toàn bộ
```bash
sudo stack -purge-server-all       # Xóa tất cả
sudo stack -purge-server-all=force # Không hỏi xác nhận
```

---

# 2. SITE MANAGER

## Tạo Website

### WordPress Sites
```bash
sudo site domain.com -wp                    # WordPress cơ bản
sudo site domain.com -wp=custom             # WordPress tùy chỉnh
sudo site domain.com -wp=[true,true,localhost,dbname,dbuser,dbpass,wp_]
sudo site domain.com -wp -subfolder=/blog   # WP trong subfolder
```

### Các loại site khác
```bash
sudo site domain.com -html          # HTML static
sudo site domain.com -php           # PHP site
sudo site domain.com -mysql         # MySQL site
sudo site domain.com -empty         # Site trống
sudo site domain.com -empty=blank   # Site trống hoàn toàn
```

### Proxy và Forward
```bash
sudo site domain.com -proxy=http://localhost:3000    # Reverse proxy
sudo site domain.com -parked=maindomain.com          # Parked domain
sudo site domain.com -forward=https://newdomain.com  # Forward domain
```

## Quản Lý Site

```bash
sudo site domain.com -on         # Kích hoạt site
sudo site domain.com -off        # Vô hiệu hóa site
sudo site domain.com -delete     # Xóa site
sudo site -delete-all            # Xóa tất cả sites
sudo site -list                  # Liệt kê sites
sudo site domain.com -info       # Thông tin site
```

## SSL Management

```bash
sudo site domain.com -ssl=on                    # Kích hoạt SSL
sudo site domain.com -ssl=off                   # Tắt SSL
sudo site domain.com -ssl=force-renewal         # Làm mới SSL
sudo site -ssl=renew                           # Làm mới tất cả
sudo site domain.com -ssl=on -wildcard         # SSL wildcard
sudo site domain.com -ssl-crt=/path -ssl-key=/path # Custom SSL
```

## Cache Management

```bash
sudo site domain.com -cache=on       # Bật FastCGI cache
sudo site domain.com -cache=off      # Tắt cache
sudo site domain.com -cache=purge    # Xóa cache
```

## Clone và Staging

```bash
sudo site staging.com -clone-from=production.com
sudo site staging.com -clone-from=prod.com -overwrite=on
sudo site domain.com -multisite-convert    # Chuyển WP Multisite
```

## URL Management

```bash
sudo site domain.com -force-redirect=www     # Redirect to www
sudo site domain.com -force-redirect=root    # Redirect to non-www
sudo site domain.com -force-redirect=off     # Tắt redirect
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
sudo webinoly -update                # Cập nhật Webinoly
sudo webinoly -info                  # Thông tin server
sudo webinoly -version               # Phiên bản
sudo webinoly -verify                # Kiểm tra tính toàn vẹn
sudo webinoly -verify=critical       # Kiểm tra nghiêm ngặt
```

## Server Configuration

```bash
sudo webinoly -tools-site=tools.domain.com    # Thiết lập tools site
sudo webinoly -tools-port=8080                # Thay đổi tools port
sudo webinoly -default-site=domain.com        # Default site
sudo webinoly -sftp=on                        # SFTP access
sudo webinoly -timezone=Asia/Ho_Chi_Minh      # Set timezone
```

## Database Management

```bash
sudo webinoly -dbpass                     # Hiển thị DB passwords
sudo webinoly -mysql-password             # Đổi MySQL root password
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

## Xem Logs Real-time

### Website Logs
```bash
log domain.com                    # Access log
log domain.com -error             # Error log
log domain.com -lines=100         # 100 dòng cuối
log -error                       # Tất cả error logs
log                             # Tất cả access logs
```

### System Service Logs
```bash
log -php                         # PHP-FPM logs
log -fpm                         # Alias cho PHP
log -mail                        # Mail logs
log -email                       # Alias cho mail
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
log -mysql=general -enable       # Bật general log
log -mysql=general -disable      # Tắt general log
log -mysql=general               # Xem general log
```

### MySQL Slow Query Log
```bash
log -mysql=slow -enable                      # Bật slow query log
log -mysql=slow -enable -long-query-time=5   # Bật với threshold 5s
log -mysql=slow -disable                     # Tắt slow query log
log -mysql=slow                              # Xem slow query log
```

### MySQL Binary Log
```bash
log -mysql=binary -enable        # Bật binary log
log -mysql=binary -disable       # Tắt binary log
log -mysql=binary                # Xem binary log
```

## WordPress Debug

```bash
log domain.com -wp=on            # Bật WP debug mode
log domain.com -wp=off           # Tắt WP debug mode
log domain.com -wp               # Xem WP debug log
log domain.com -wp=on -display=off   # Debug không hiển thị
log domain.com -wp -subfolder=/blog  # WP debug subfolder
```

## Access Log Management

### Global Settings
```bash
log -access-log=on               # Bật access log global
log -access-log=off              # Tắt access log global
log -only-error=on               # Chỉ error log
log -only-error=off              # Bật lại access log
```

### Per Site Settings
```bash
log domain.com -access-log=on    # Bật access log cho site
log domain.com -access-log=off   # Tắt access log cho site
```

## Log Cleanup

```bash
log -purge=all                   # Xóa tất cả .gz logs (hỏi xác nhận)
log -purge=nginx                 # Xóa nginx .gz logs
log -purge=mysql                 # Xóa mysql .gz logs
log -purge=letsencrypt           # Xóa letsencrypt .gz logs
log -purge=redis                 # Xóa redis .gz logs
log -purge=force                 # Xóa không hỏi xác nhận
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

- Luôn backup trước khi thực hiện thay đổi lớn
- Kiểm tra config nginx với `sudo nginx -t`
- Sử dụng HTTPS cho production sites
- Bật HTTP auth cho admin areas
- Theo dõi logs thường xuyên
- Giữ hệ thống được cập nhật
- Sử dụng mật khẩu mạnh
- Chỉ whitelist các IP đáng tin cậy

---

Tài liệu này cung cấp hướng dẫn toàn diện về Webinoly. Để biết thêm chi tiết, tham khảo tại https://webinoly.com