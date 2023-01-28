SSL Certificate: 

- Uses HTTPS for secure communication.
- Certifies the ownership of a public key.
- Used for encrypting data sent between a browser and a remote server.


TLS: Transport Layer Security.




# Types of Certificates:

Domain Scopes:
- Single domain (www.coolsite.com)
- Wildcard (*.coolsite.com)
- Multi-domain (coolsite.com, greatsite.org, coolsite.net)

Validation Levels:
- Domain Validation(DV)
- Organization validation(OV)
- Extended validation(EV)

# Create key and signed with CSR (certificate signing request):

- Generate private key: 
$ openssl genrsa -out private.key 2048

- Sign with private key, get the csr which will be public:
$ openssl req -new -key private.key -out csr.key

- Get the information of the public key (inside the csr.key):
$ openssl req -text -in csr.key -noout


# Install on Apache:
- Configuration files in:
/etc/httpd or /etc/apache2

# Install on NGINX:
- Configuration files in:
/etc/nginx or /opt/nginx


```
server  {
    listen 80 default_server;
    server_name yourdomain.com;
    root /var/www/html;
    index index.html;
    listen 443 ssl;
    ssl_certificate /path/to/domain.crt;
    ssl_certificate /path/to/domain.key;
}
```

- Test if it works:
$ sudo /etc/init.d/nginx -t
$ sudo /etc/init.d/nginx restart
$ sudo /etc/init.d/nginx -s reload


# HTTP Strict Transport Security (HSTS):


Add a directive to the HTTP Header for a period time:
- "Strict-Transport-Security" 

```
add _header Strict-Transport-Security "max-age=300; includeSubdomains;";
``` 


# Renew certificates with Certbot:
$ sudo certbot renew --dry-run
$ sudo certbot renew 

Use cronjob:

0 */12 * * * certbot -q renew


Check if cronjob is already created by certbot:

/etc/cron.d/certbot

Use:
crontab -e