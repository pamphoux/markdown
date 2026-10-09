# Apache2 - Virtualisation des domaines

## [A] - Le Virtual Host (VHost)

> *Comment Apache sait quel site afficher quand j'ai plusieurs domaines ?
> Grâce aux Virtual Hosts. Chaque VHost est un fichier de configuration qui dit :
> ServerName : "Si le visiteur demande CE domaine..."
> DocumentRoot : "...alors sers les fichiers de CE dossier"*

```apacheconf
# Traduction humaine de ce VHost :
# "Si quelqu'un demande www.boutique.com, envoie-lui le contenu de /var/www/boutique"
<VirtualHost *:80>
    ServerName www.boutique.com      # ← Le domaine
    DocumentRoot /var/www/boutique   # ← Le dossier des fichiers
</VirtualHost>
```

### [A.1] - Les modules (a2enmod)

|     | Module  | Ce qu'il fait             | Quand l'activer                             |
| --- | ------- | ------------------------- | ------------------------------------------- |
|     | ssl     | HTTPS                     | Dès que vous voulez du chiffrement          |
|     | rewrite | Réécriture d'URLs         | WordPress, redirections, URLs propres       |
|     | proxy   | Reverse proxy             | Quand Apache est devant une app Node/Python |
|     | headers | Modifier les headers HTTP | Sécurité, cache, CORS                       |
|     | deflate | Compression gzip          | Toujours (gain de bande passante)           |

### [A.2] - Virtual Hosts mono-sites

Les quatre étapes suivantes produisent un site accessible sur un domaine dédié,
sans toucher au vhost par défaut. L'ordre compte :

* le répertoire et son propriétaire d'abord,
* la configuration ensuite,
* l'activation en dernier.
  Si vous écrivez le Virtual Host avant de créer le DocumentRoot, apache2ctl
  configtest répondra tout de même « Syntax OK » puisqu'il ne vérifie pas l'existence
  des chemins, et vous récolterez une 403 à la première requête.

#### [A.2.a] - Créer le répertoire et les fichiers

sudo mkdir -p /var/www/wordpress
sudo mkdir -p /var/www/oscommerce
sudo mkdir -p /var/www/opencart
echo '<h1>Wordpress</h1>' | sudo tee /var/www/wordpress/index.html
echo '<h1>OS Commerce</h1>' | sudo tee /var/www/oscommerce/index.html
echo '<h1>Open Cart</h1>' | sudo tee /var/www/opencart/index.html
sudo chown -R www-data:www-data /var/www/wordpress
sudo chown -R www-data:www-data /var/www/oscommerce
sudo chown -R www-data:www-data /var/www/opencart
sudo chmod -R 777 /var/www/wordpress
sudo chmod -R 777 /var/www/oscommerce
sudo chmod -R 777 /var/www/opencart

#### [A.2.b] - Virtual Host par défaut

##### 000-default

###### http (même IP / Différents port)

```shell
sudo vi /etc/apache2/sites-available/000-default.conf
```

```apacheconf
<VirtualHost *:80>
#<VirtualHost localhost:80>
#<VirtualHost 85.215.170.153:80>
#<VirtualHost debian13.home:80>
#       ServerName 85.215.170.153
        ServerName localhost
        ServerAlias localhost
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
### Config Alternative Port 8080
#<VirtualHost *:8080>
#<VirtualHost localhost:8080>
#<VirtualHost 192.168.1.4:8080>
#<VirtualHost debian13.home:8080>
#        ServerName localhost
#        ServerAlias localhost
#        ServerAdmin webmaster@localhost
#        DocumentRoot /var/www/html
#        ErrorLog ${APACHE_LOG_DIR}/error.log
#        CustomLog ${APACHE_LOG_DIR}/access.log combined
#</VirtualHost>
```

```shell-session
sudo a2ensite 000-default.conf
sudo systemctl reload apache2
```

| Action    | Adresses IP autorisées | Protocole | Port(s) | Description |
|:---------:|:----------------------:|:---------:|:-------:|:-----------:|
| Autoriser | Tous                   | TCP       | 80      | HTTP        |
| Autoriser | Tous                   | TCP       | 443     | HTTPS       |

> http://85.215.170.153
> http://85.215.170.153/info.php
> http://85.215.170.153/index.html

###### https (même IP / Différents port)

```shell
sudo vi /etc/apache2/sites-available/default-ssl.conf
```

```apacheconf
<VirtualHost *:443>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html
        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf

        #   SSL Engine Switch:
        #   Enable/Disable SSL for this virtual host.
        SSLEngine on

        #   A self-signed (snakeoil) certificate can be created by installing
        #   the ssl-cert package. See
        #   /usr/share/doc/apache2/README.Debian.gz for more info.
        #   If both key and certificate are stored in the same file, only the
        #   SSLCertificateFile directive is needed.
        SSLCertificateFile      /etc/ssl/certs/apache-self-signed.crt
        SSLCertificateKeyFile   /etc/ssl/private/apache-self-signed.key
        SSLCertificateFile      /etc/ssl/certs/ssl-cert-snakeoil.pem
        SSLCertificateKeyFile   /etc/ssl/private/ssl-cert-snakeoil.key

        #   Server Certificate Chain:
        #   Point SSLCertificateChainFile at a file containing the
        #   concatenation of PEM encoded CA certificates which form the
        #   certificate chain for the server certificate. Alternatively
        #   the referenced file can be the same as SSLCertificateFile
        #   when the CA certificates are directly appended to the server
        #   certificate for convinience.
        # SSLCertificateChainFile /etc/apache2/ssl.crt/server-ca.crt

        #   Certificate Authority (CA):
        #   Set the CA certificate verification path where to find CA
        #   certificates for client authentication or alternatively one
        #   huge file containing all of them (file must be PEM encoded)
        #   Note: Inside SSLCACertificatePath you need hash symlinks
        #         to point to the certificate files. Use the provided
        #         Makefile to update the hash symlinks after changes.
        # SSLCACertificatePath /etc/ssl/certs/
        # SSLCACertificateFile /etc/apache2/ssl.crt/ca-bundle.crt
        #   Certificate Revocation Lists (CRL):
        #   Set the CA revocation path where to find CA CRLs for client
        #   authentication or alternatively one huge file containing all
        #   of them (file must be PEM encoded)
        #   Note: Inside SSLCARevocationPath you need hash symlinks
        #         to point to the certificate files. Use the provided
        #         Makefile to update the hash symlinks after changes.
        # SSLCARevocationPath /etc/apache2/ssl.crl/
        # SSLCARevocationFile /etc/apache2/ssl.crl/ca-bundle.crl

        #   Client Authentication (Type):
        #   Client certificate verification type and depth.  Types are
        #   none, optional, require and optional_no_ca.  Depth is a
        #   number which specifies how deeply to verify the certificate
        #   issuer chain before deciding the certificate is not valid.
        # SSLVerifyClient require
        # SSLVerifyDepth  10

        #   SSL Engine Options:
        #   Set various options for the SSL engine.
        #   o FakeBasicAuth:
        #    Translate the client X.509 into a Basic Authorisation.  This means that
        #    the standard Auth/DBMAuth methods can be used for access control.  The
        #    user name is the `one line' version of the client's X.509 certificate.
        #    Note that no password is obtained from the user. Every entry in the user
        #    file needs this password: `xxj31ZMTZzkVA'.
        #   o ExportCertData:
        #    This exports two additional environment variables: SSL_CLIENT_CERT and
        #    SSL_SERVER_CERT. These contain the PEM-encoded certificates of the
        #    server (always existing) and the client (only existing when client
        #    authentication is used). This can be used to import the certificates
        #    into CGI scripts.
        #   o StdEnvVars:
        #    This exports the standard SSL/TLS related `SSL_*' environment variables.
        #    Per default this exportation is switched off for performance reasons,
        #    because the extraction step is an expensive operation and is usually
        #    useless for serving static content. So one usually enables the
        #    exportation for CGI and SSI requests only.
        #   o OptRenegotiate:
        #    This enables optimized SSL connection renegotiation handling when SSL
        #    directives are used in per-directory context.
        # SSLOptions +FakeBasicAuth +ExportCertData +StrictRequire
        <FilesMatch "\.(?:cgi|shtml|phtml|php)$">
                SSLOptions +StdEnvVars
        </FilesMatch>
        <Directory /usr/lib/cgi-bin>
                SSLOptions +StdEnvVars
        </Directory>
</VirtualHost>
```

- [x] **Parefeu Ionos**

| Action    | Adresses IP autorisées | Protocole | Port(s) | Description |
|:---------:|:----------------------:|:---------:|:-------:|:-----------:|
| Autoriser | Tous                   | TCP       | 80      | HTTP        |
| Autoriser | Tous                   | TCP       | 443     | HTTPS       |

> **https://85.215.170.153**
> **https://85.215.170.153/info.php**

### [A.3] - Vhost multi sites

#### [A.3.a] - Créer le répertoire et les fichiers

```bash
sudo mkdir -p /var/www/wordpress
sudo mkdir -p /var/www/oscommerce
sudo mkdir -p /var/www/opencart
echo '<h1>Wordpress</h1>' | sudo tee /var/www/wordpress/index.html
echo '<h1>OS Commerce</h1>' | sudo tee /var/www/oscommerce/index.html
echo '<h1>Open Cart</h1>' | sudo tee /var/www/opencart/index.html
```

#### [A.3.b] - Attribuer les permissions

```bash
sudo chown -R www-data:www-data /var/www
sudo chown -R www-data:www-data /var/www/wordpress
sudo chown -R www-data:www-data /var/www/oscommerce
sudo chown -R www-data:www-data /var/www/opencart
sudo chmod -R 644 /var/www/wordpress
sudo chmod -R 644 /var/www/oscommerce
sudo chmod -R 644 /var/www/opencart
```

#### [A.3.c] - Configuration

##### [A.3.c.1] - 001-wordpress.conf

```shell
sudo tee /etc/apache2/sites-available/001-wordpress.conf << 'EOF'
<VirtualHost *:80>
    ServerName localhost
    ServerAlias localhost
    DocumentRoot /var/www/wordpress

    <Directory /var/www/wordpress>
        Options -Indexes +FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/monsite-error.log
    CustomLog ${APACHE_LOG_DIR}/monsite-access.log combined
</VirtualHost>
EOF
```

##### [A.3.c.2] - wordpress-ssl.conf

```shell
sudo tee /etc/apache2/sites-available/wordpress-ssl.conf << 'EOF'
<VirtualHost *:443>
	ServerAdmin webmaster@localhost

	DocumentRoot /var/www/wordpress

	# Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
	# error, crit, alert, emerg.
	# It is also possible to configure the loglevel for particular
	# modules, e.g.
	#LogLevel info ssl:warn

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

	# For most configuration files from conf-available/, which are
	# enabled or disabled at a global level, it is possible to
	# include a line for only one particular virtual host. For example the
	# following line enables the CGI configuration for this host only
	# after it has been globally disabled with "a2disconf".
	#Include conf-available/serve-cgi-bin.conf

	#   SSL Engine Switch:
	#   Enable/Disable SSL for this virtual host.
	SSLEngine on

	#   A self-signed (snakeoil) certificate can be created by installing
	#   the ssl-cert package. See
	#   /usr/share/doc/apache2/README.Debian.gz for more info.
	#   If both key and certificate are stored in the same file, only the
	#   SSLCertificateFile directive is needed.
	#SSLCertificateFile      /etc/ssl/certs/apache-self-signed.crt
	#SSLCertificateKeyFile   /etc/ssl/private/apache-self-signed.key
	SSLCertificateFile      /etc/ssl/certs/ssl-cert-snakeoil.pem
	SSLCertificateKeyFile   /etc/ssl/private/ssl-cert-snakeoil.key

	#   Server Certificate Chain:
	#   Point SSLCertificateChainFile at a file containing the
	#   concatenation of PEM encoded CA certificates which form the
	#   certificate chain for the server certificate. Alternatively
	#   the referenced file can be the same as SSLCertificateFile
	#   when the CA certificates are directly appended to the server
	#   certificate for convinience.
	#SSLCertificateChainFile /etc/apache2/ssl.crt/server-ca.crt

	#   Certificate Authority (CA):
	#   Set the CA certificate verification path where to find CA
	#   certificates for client authentication or alternatively one
	#   huge file containing all of them (file must be PEM encoded)
	#   Note: Inside SSLCACertificatePath you need hash symlinks
	#         to point to the certificate files. Use the provided
	#         Makefile to update the hash symlinks after changes.
	#SSLCACertificatePath /etc/ssl/certs/
	#SSLCACertificateFile /etc/apache2/ssl.crt/ca-bundle.crt

	#   Certificate Revocation Lists (CRL):
	#   Set the CA revocation path where to find CA CRLs for client
	#   authentication or alternatively one huge file containing all
	#   of them (file must be PEM encoded)
	#   Note: Inside SSLCARevocationPath you need hash symlinks
	#         to point to the certificate files. Use the provided
	#         Makefile to update the hash symlinks after changes.
	#SSLCARevocationPath /etc/apache2/ssl.crl/
	#SSLCARevocationFile /etc/apache2/ssl.crl/ca-bundle.crl

	#   Client Authentication (Type):
	#   Client certificate verification type and depth.  Types are
	#   none, optional, require and optional_no_ca.  Depth is a
	#   number which specifies how deeply to verify the certificate
	#   issuer chain before deciding the certificate is not valid.
	#SSLVerifyClient require
	#SSLVerifyDepth  10

	#   SSL Engine Options:
	#   Set various options for the SSL engine.
	#   o FakeBasicAuth:
	#    Translate the client X.509 into a Basic Authorisation.  This means that
	#    the standard Auth/DBMAuth methods can be used for access control.  The
	#    user name is the `one line' version of the client's X.509 certificate.
	#    Note that no password is obtained from the user. Every entry in the user
	#    file needs this password: `xxj31ZMTZzkVA'.
	#   o ExportCertData:
	#    This exports two additional environment variables: SSL_CLIENT_CERT and
	#    SSL_SERVER_CERT. These contain the PEM-encoded certificates of the
	#    server (always existing) and the client (only existing when client
	#    authentication is used). This can be used to import the certificates
	#    into CGI scripts.
	#   o StdEnvVars:
	#    This exports the standard SSL/TLS related `SSL_*' environment variables.
	#    Per default this exportation is switched off for performance reasons,
	#    because the extraction step is an expensive operation and is usually
	#    useless for serving static content. So one usually enables the
	#    exportation for CGI and SSI requests only.
	#   o OptRenegotiate:
	#    This enables optimized SSL connection renegotiation handling when SSL
	#    directives are used in per-directory context.
	#SSLOptions +FakeBasicAuth +ExportCertData +StrictRequire
	<FilesMatch "\.(?:cgi|shtml|phtml|php)$">
		SSLOptions +StdEnvVars
	</FilesMatch>
	<Directory /usr/lib/cgi-bin>
		SSLOptions +StdEnvVars
	</Directory>
</VirtualHost>
EOF
```

#### [A.3.d] - Activation

```shell
sudo a2ensite 001-wordpress.conf
sudo a2ensite wordpress-ssl.conf
sudo systemctl reload apache2
sudo systemctl restart apache2
```

#### [A.3.e] - Ports

```shell
sudo vi /etc/apache2/ports.conf
Listen 80
Listen 8080
Listen 8081
Listen 8082
<IfModule ssl_module>
        Listen 443
        Listen 8443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>
```
