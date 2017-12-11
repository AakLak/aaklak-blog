---
title: "How to Install Nginx, MySQL, and PHP 7 (LEMP Stack) on CentOS 7"
date: 2017-07-21
subtitle: ""
tags: ["CentOS 7", "LEMP", "Nginx", "Vultr"]
draft: true
---

ssh root@IP_ADDRESS

NGINX
Since Nginx is not available on the default CentOS repositories, install the EPEL repository.
`yum install epel-release -y`

Next, install Nginx
`yum install nginx -y`


Run the next two lines to start Nginx, and have it start at each boot.
`systemctl start Nginx
 systemctl enable Nginx`

 Apparently my VPS had a firewall on it, so I had to run these lines in order to connect to it:
`sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload`

You should now be able to visit your server's IP and see the Nginx welcome screen.

Note: Run ip addr to get IP

MySQL



Grant access to Flarum:
[root@rsbotspot ~]# sudo chmod -R 777 /var/www/html/
[root@rsbotspot ~]# sudo chmod -R 777 /var/www/

vi /var/log/nginx/error.log

chmod -R 0777 /var/lib/php/session/
017/09/12 04:11:55 [error] 12704#0: *5 FastCGI sent in stderr: "PHP message: PHP Warning:  SessionHandler::read(): open(/var/lib/php/session/sess_kf9job6vmcrvdv0gif7vbpp4rj, O_RDWR) failed: Permission denied (13) in /var/www/html/vendor/symfony/http-foundation/Session/Storage/Proxy/SessionHandlerProxy.php on line 69
PHP message: PHP Warning:  SessionHandler::write(): open(/var/lib/php/session/sess_kf9job6vmcrvdv0gif7vbpp4rj, O_RDWR) failed: Permission denied (13) in /var/www/html/vendor/symfony/http-foundation/Session/Storage/Proxy/SessionHandlerProxy.php on line 77
PHP message: PHP Warning:  session_write_close(): Failed to write session data using user defined save handler. (session.save_path: /var/lib/php/session) in Unknown on line 0" while reading response header from upstream, client: 84.243.219.143, server: 104.238.181.129, request: "HEAD http://104.238.181.29:80/mysql/admin/ HTTP/1.1", upstream: "fastcgi://unix:/var/run/php-fpm/php-fpm.sock:", host: "104.238.181.29"
2017/09/12 04:11:55 [error] 12704#0: *5 FastCGI sent in stderr: "PHP message: PHP Warning:  SessionHandler::read(): open(/var/lib/php/session/sess_ovf97unlsge676oeegvavo06q3, O_RDWR) failed: Permission denied (13) in /var/www/html/vendor/symfony/http-foundation/Session/Storage/Proxy/SessionHandlerProxy.php on line 69
PHP message: PHP Warning:  SessionHandler::write(): open(/var/lib/php/session/sess_ovf97unlsge676oeegvavo06q3, O_RDWR) failed: Permission denied (13) in /var/www/html/vendor/symfony/http-foundation/Session/Storage/Proxy/SessionHandlerProxy.php on line 77
PHP message: PHP Warning:  session_write_close(): Failed to write session data using user defined save handler. (session.save_path: /var/lib/php/session) in Unknown on line 0" while reading response header from upstream, client: 84.243.219.143, server: 104.238.181.129, request: "GET http://104.238.181.29:80/mysql/admin/ HTTP/1.0", upstream: "fastcgi://unix:/var/run/php-fpm/php-fpm.sock:", host: "104.238.181.29"
2017/09/12 04:11:55 [error] 12704#0: *8 FastCGI sent in stderr: "PHP message: PHP Warning:  SessionHandler::read(): open(/var/lib/php/session/sess_fulhnd77gbjiicgkhilj3o07vl, O_RDWR) failed: Permission denied (13) in /var/www/html/vendor/symfony/http-foundation/Session/Storage/Proxy/SessionHandlerProxy.php on line 69
PHP message: PHP Warning:  SessionHandler::write(): open(/var/lib/php/session/sess_fulhnd77gbjiicgkhilj3o07vl, O_RDWR) failed: Permission denied (13) in /var/www/html/vendor/symfony/http-foundation/Session/Storage/Proxy/SessionHandlerProxy.php on line 77*


ls -l /path/to/file

https://discuss.flarum.org/d/1951-a-flarum-installation-guide-for-dummies-and-the-truly-clueless



http://flarum.org/docs/installation/
https://stackoverflow.com/questions/41274829/php-error-the-zip-extension-and-unzip-command-are-both-missing-skipping
https://www.godaddy.com/help/build-a-lemp-stack-linux-nginx-mysql-php-centos-7-17349
https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-centos-7
https://www.hostinger.com/tutorials/how-to-install-lemp-centos7
http://wiki.crowncloud.net/?guide_for_nginx_php_on_centos_7
