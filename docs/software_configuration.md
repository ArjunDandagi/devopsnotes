# SSL on Apache 

for nginx i just needed to to add the combined (hostname cert and ca-bundle) as a certificate
and the key 

but for apache2/httpd [GeekFlare](https://geekflare.com/apache-setup-ssl-certificate) worked well


//TODO : understand ocsp stapling and how to configure in nginx/apache2


## apache ( httpd.conf) 
```
RewriteEngine On
RewriteCond %{HTTPS} !=on
RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]

<VirtualHost *:80>
       ServerName www.mydomain.com
       DocumentRoot /var/www/html/public
       <Directory /var/www/html>
           AllowOverride All
       </Directory>
	Redirect permanent / https://www.mydomain.com/
</VirtualHost>

```

the ssl.conf is something like  ( /etc/httpd/conf.d/ssl.conf , enabled once you install mod_ssl .like yum install mod_ssl -y)
```
<VirtualHost *:443>
    ServerName domain.com
    SSLEngine on
    SSLCertificateFile "/path/to/cert"
   SSLCertificateKeyFile "/path/to/key"
   SSLCertificateChainFile "/path/to/ca-bundle"
</VirtualHost>
```

## NGINX configuration ( /etc/nginx/nginx.conf)
```
from [NGINX docs](http://nginx.org/en/docs/http/configuring_https_servers.html)
server {
    listen              443 ssl;
    server_name         www.example.com;
    ssl_certificate     www.example.com.crt;
    ssl_certificate_key www.example.com.key;
    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers         HIGH:!aNULL:!MD5;
    ...
}

```









