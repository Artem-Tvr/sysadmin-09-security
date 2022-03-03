# Домашнее задание к занятию "3.9. Элементы безопасности информационных систем"

(1) Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.

*Решение*

![1.jpg](./assets/1.jpg)

(2) Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.

*Решение*

Установил приложение, добавил Bitwarden.

![2.jpg](./assets/2.jpg)

(3) Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.

*Решение*

Ниже указал последовательность моих действий, но в браузере сайт не открылся и при проверке на корректность настроек выдал следующее - AH00112: Warning: DocumentRoot [/var/www/test.site/public_html] does not exist - ругается что не существует директория, но как ее создать не понял. Подскажите куда копать?

```

root@vagrant:~# a2ensite test.site
ERROR: Site test.site does not exist!
root@vagrant:~# nano /etc/apache2/sites-available/test.site.conf
root@vagrant:~# a2ensite test.site
Enabling site test.site.
To activate the new configuration, you need to run:
  systemctl reload apache2
root@vagrant:~# systemctl reload apache2
root@vagrant:~# nano /etc/hosts
root@vagrant:~# systemctl reload apache2
root@vagrant:~# apache2ctl -M
AH00112: Warning: DocumentRoot [/var/www/test.site/public_html] does not exist
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
Loaded Modules:
 core_module (static)
 so_module (static)
 watchdog_module (static)
 http_module (static)
 log_config_module (static)
 logio_module (static)
 version_module (static)
 unixd_module (static)
 access_compat_module (shared)
 alias_module (shared)
 auth_basic_module (shared)
 authn_core_module (shared)
 authn_file_module (shared)
 authz_core_module (shared)
 authz_host_module (shared)
 authz_user_module (shared)
 autoindex_module (shared)
 deflate_module (shared)
 dir_module (shared)
 env_module (shared)
 filter_module (shared)
 mime_module (shared)
 mpm_event_module (shared)
 negotiation_module (shared)
 reqtimeout_module (shared)
 setenvif_module (shared)
 status_module (shared)
root@vagrant:~# a2enmod expires
Enabling module expires.
To activate the new configuration, you need to run:
  systemctl restart apache2
root@vagrant:~# systemctl reload apache2
root@vagrant:~# a2enmod headers
Enabling module headers.
To activate the new configuration, you need to run:
  systemctl restart apache2
root@vagrant:~# systemctl restart apache2
root@vagrant:~# a2enmod rewrite
Enabling module rewrite.
To activate the new configuration, you need to run:
  systemctl restart apache2
root@vagrant:~# systemctl restart apache2
root@vagrant:~# a2enmod ssl
Considering dependency setenvif for ssl:
Module setenvif already enabled
Considering dependency mime for ssl:
Module mime already enabled
Considering dependency socache_shmcb for ssl:
Enabling module socache_shmcb.
Enabling module ssl.
See /usr/share/doc/apache2/README.Debian.gz on how to configure SSL and create self-signed certificates.
To activate the new configuration, you need to run:
  systemctl restart apache2
root@vagrant:~# systemctl restart apache2
root@vagrant:~# curl www/test.site/public_html
curl: (6) Could not resolve host: www
root@vagrant:~# curl test.site/public_html
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL was not found on this server.</p>
<hr>
<address>Apache/2.4.41 (Ubuntu) Server at test.site Port 80</address>
</body></html>
root@vagrant:~# openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
Generating a RSA private key
.......................................................................+++++
....................................+++++
writing new private key to '/etc/ssl/private/apache-selfsigned.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:RU
State or Province Name (full name) [Some-State]:Tver
Locality Name (eg, city) []:Tver City
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Home made, Inc
Organizational Unit Name (eg, section) []:Ministry of Magic
Common Name (e.g. server FQDN or YOUR name) []:127.0.0.1
Email Address []:admin@test.site
root@vagrant:~# nano /etc/apache2/conf-available/ssl-params.conf
root@vagrant:~# cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/default-ssl.conf.bak
root@vagrant:~# nano /etc/apache2/sites-available/default-ssl.conf


Use "fg" to return to nano.

[1]+  Stopped                 nano /etc/apache2/sites-available/default-ssl.conf
root@vagrant:~# nano /etc/apache2/sites-available/default-ssl.conf
root@vagrant:~# nano /etc/apache2/sites-available/default-ssl.conf
root@vagrant:~# ufw app list
Available applications:
  Apache
  Apache Full
  Apache Secure
  OpenSSH
root@vagrant:~# ufw status
Status: inactive
root@vagrant:~# a2enmod ssl
Considering dependency setenvif for ssl:
Module setenvif already enabled
Considering dependency mime for ssl:
Module mime already enabled
Considering dependency socache_shmcb for ssl:
Module socache_shmcb already enabled
Module ssl already enabled
root@vagrant:~# a2ensite default-ssl
Enabling site default-ssl.
To activate the new configuration, you need to run:
  systemctl reload apache2
root@vagrant:~# systemctl reload apache2
root@vagrant:~# a2enconf ssl-params
Enabling conf ssl-params.
To activate the new configuration, you need to run:
  systemctl reload apache2
root@vagrant:~# systemctl reload apache2
root@vagrant:~# apache2ctl configtest
AH00112: Warning: DocumentRoot [/var/www/test.site/public_html] does not exist
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
Syntax OK
root@vagrant:~# nano /etc/apache2/sites-available/default-ssl.conf
root@vagrant:~# systemctl reload apache2
root@vagrant:~# apache2ctl configtest
AH00112: Warning: DocumentRoot [/var/www/test.site/public_html] does not exist
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
Syntax OK
root@vagrant:~# nano /etc/apache2/sites-available/default-ssl.conf
root@vagrant:~# nano /etc/apache2/sites-available/test.site.conf
root@vagrant:~# nano /var/www/test.site/public_html
root@vagrant:~# cat /var/www/test.site/public_html
cat: /var/www/test.site/public_html: No such file or directory
root@vagrant:~# ls /var/www/test.site
ls: cannot access '/var/www/test.site': No such file or directory
root@vagrant:~# nano /etc/apache2/sites-available/test.site.conf
root@vagrant:~# nano /etc/apache2/sites-available/default-ssl.conf
root@vagrant:~# nano /etc/apache2/sites-available/test.site.conf
root@vagrant:~# nano /etc/apache2/sites-available/default-ssl.conf
root@vagrant:~# systemctl reload apache2
root@vagrant:~# apache2ctl configtest
AH00112: Warning: DocumentRoot [/var/www/test.site/public_html] does not exist
AH00112: Warning: DocumentRoot [/var/www/test.site/public_html] does not exist
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
Syntax OK
root@vagrant:~# nano /etc/apache2/sites-available/default-ssl.conf
root@vagrant:~# nano /etc/apache2/sites-available/test.site.conf
root@vagrant:~# systemctl reload apache2
Job for apache2.service failed.
See "systemctl status apache2.service" and "journalctl -xe" for details.
root@vagrant:~# nano /etc/apache2/sites-available/test.site.conf
root@vagrant:~# systemctl reload apache2
root@vagrant:~# curl https://test.site/
curl: (60) SSL certificate problem: self signed certificate
More details here: https://curl.haxx.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.
```

Содержание файла /etc/apache2/sites-available/test.site.conf

```
<VirtualHost *:80>
ServerName test.site
ServerAlias www.test.site
ServerAdmin webmaster@localhost
DocumentRoot /var/www/test.site/public_html
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Содержание файла /etc/apache2/sites-available/default-ssl.conf

```
<IfModule mod_ssl.c>
    <VirtualHost _default_:443>
            ServerAdmin your_email@example.com
            ServerName 127.0.0.1

            DocumentRoot /var/www/html

            ErrorLog ${APACHE_LOG_DIR}/error.log
            CustomLog ${APACHE_LOG_DIR}/access.log combined

            SSLEngine on

            SSLCertificateFile      /etc/ssl/certs/apache-selfsigned.crt
            SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key

            <FilesMatch "\.(cgi|shtml|phtml|php)$">
                            SSLOptions +StdEnvVars
            </FilesMatch>
            <Directory /usr/lib/cgi-bin>
                            SSLOptions +StdEnvVars
            </Directory>
    </VirtualHost>

</IfModule>
```

(4) Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное).

*Решение*

После сканирования сайта найдены две потенциальные уязвимости: BREACH (CVE-2013-3587), LUCKY13 (CVE-2013-0169), experimental

```
root@vagrant:~/testssl.sh# ./testssl.sh -U --sneaky https://www.ozon.ru/

###########################################################
    testssl.sh       3.1dev from https://testssl.sh/dev/
    (740756b 2022-02-26 14:18:56 -- )

      This program is free software. Distribution and
             modification under GPLv2 permitted.
      USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!

       Please file bugs @ https://testssl.sh/bugs/

###########################################################

 Using "OpenSSL 1.0.2-chacha (1.0.2k-dev)" [~183 ciphers]
 on vagrant:./bin/openssl.Linux.x86_64
 (built: "Jan 18 17:12:17 2019", platform: "linux-x86_64")


 Start 2022-02-28 17:44:05        -->> 45.60.40.164:443 (www.ozon.ru) <<--

 rDNS (45.60.40.164):    --
 Service detected:       HTTP


 Testing vulnerabilities

 Heartbleed (CVE-2014-0160)                not vulnerable (OK), no heartbeat extension
 CCS (CVE-2014-0224)                       not vulnerable (OK)
 Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK)
 ROBOT                                     not vulnerable (OK)
 Secure Renegotiation (RFC 5746)           supported (OK)
 Secure Client-Initiated Renegotiation     not vulnerable (OK)
 CRIME, TLS (CVE-2012-4929)                not vulnerable (OK)
 BREACH (CVE-2013-3587)                    potentially NOT ok, "gzip" HTTP compression detected. - only supplied "/" tested
                                           Can be ignored for static pages or if no secrets in the page
 POODLE, SSL (CVE-2014-3566)               not vulnerable (OK)
 TLS_FALLBACK_SCSV (RFC 7507)              No fallback possible (OK), no protocol below TLS 1.2 offered
 SWEET32 (CVE-2016-2183, CVE-2016-6329)    not vulnerable (OK)
 FREAK (CVE-2015-0204)                     not vulnerable (OK)
 DROWN (CVE-2016-0800, CVE-2016-0703)      not vulnerable on this host and port (OK)
                                           make sure you don't use this certificate elsewhere with SSLv2 enabled services
                                           https://censys.io/ipv4?q=66799C5A87D7AB8A97925D1E4B1D6E05B7CACEAB3B24911190F825C992103124 could help you to find out
 LOGJAM (CVE-2015-4000), experimental      not vulnerable (OK): no DH EXPORT ciphers, no DH key detected with <= TLS 1.2
 BEAST (CVE-2011-3389)                     not vulnerable (OK), no SSL3 or TLS1
 LUCKY13 (CVE-2013-0169), experimental     potentially VULNERABLE, uses cipher block chaining (CBC) ciphers with TLS. Check patches
 Winshock (CVE-2014-6321), experimental    not vulnerable (OK)
 RC4 (CVE-2013-2566, CVE-2015-2808)        no RC4 ciphers detected (OK)


 Done 2022-02-28 17:44:53 [  51s] -->> 45.60.40.164:443 (www.ozon.ru) <<--

```

(5) Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.

*Решение*

(6) Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.

*Решение*

(7) Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.

*Решение*
