# 20 - Web Server (Apache httpd) & Nginx

## Apache
```bash
dnf install httpd -y
systemctl enable --now httpd
firewall-cmd --permanent --add-service={http,https} && firewall-cmd --reload
httpd -t                       # syntax check
httpd -S                       # virtual hosts
apachectl graceful
```
Files: `/etc/httpd/conf/httpd.conf`, `/etc/httpd/conf.d/`, docroot `/var/www/html`, logs `/var/log/httpd/`.

Virtual host `/etc/httpd/conf.d/site1.conf`:
```apache
<VirtualHost *:80>
    ServerName site1.example.com
    DocumentRoot /var/www/site1
    ErrorLog  /var/log/httpd/site1_error.log
    CustomLog /var/log/httpd/site1_access.log combined
</VirtualHost>
```
SELinux:
```bash
semanage fcontext -a -t httpd_sys_content_t "/var/www/site1(/.*)?"
restorecon -Rv /var/www/site1
setsebool -P httpd_can_network_connect on
```

## HTTPS
```bash
dnf install mod_ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/pki/tls/private/site.key -out /etc/pki/tls/certs/site.crt
```

## Nginx
```bash
dnf install nginx
systemctl enable --now nginx
nginx -t ; nginx -s reload
```
Config: `/etc/nginx/nginx.conf`, `/etc/nginx/conf.d/*.conf`

## Test
```bash
curl -I http://localhost
curl -k https://localhost
ss -tlnp | grep :80
```
