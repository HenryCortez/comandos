# comandos
CONFIGURACIÓN WORDPRESS DISTRIBUIDO
W BD y MV: RED NAT – FISEI
Ping desde la máquina virtual al servidor WordPress
Ping desde WordPress a BD
No desde el WP a la MV
Wordpress
//Instalar Apache
yum install httpd -y 
service httpd start 
service httpd status
systemctl enable httpd
//Habilitar puertos: 80/443
firewall-cmd –permanent –zone=public –add-service=http 
firewall-cmd –permanent –zone=public –add-service=https
firewall-cmd –reload
firewall-cmd --zone=public --add-port=3306/tcp --permanent 
firewall-cmd --reload
Setenforce 0
//Instalación de PHP
//instalar php
yum install php -y
dnf install php -y
dnf install php-cli php-fpm php-curl php-mysqlnd php-gd php-opcache php-zip php-intl php-common php-bcmath php-imagick php-xml
dnf install php-devel
systemctl restart php-fpm
systemctl restart httpd
//Instalación de módulo adicional para conexión mysql-servidor web: 
yum install php-mysqlnd -y
//Instalación de Wordpress en la carpeta
cd /var/www/html/
BD
yum install mysql mysql-server -y 
systemctl start mysqld
systemctl status mysqld
systemctl enable mysqld
mysql_secure_installation
firewall-cmd --permanent --zone=public --add-service=http 
firewall-cmd --reload
firewall-cmd --zone=public --add-port=3306/tcp --permanent 
firewall-cmd --reload
Setenforce 0
mysql -u root -p
show databases;
RENAME USER 'root'@'localhost' TO 'root'@'%'; 

SERVIDOR PROXY
Proxy y MV: RED NAT – RED FISEI
yum install epel-release -y
yum install squid -y
squid -v
systemctl start squid
systemctl enable squid
yum install nano -y
cd /etc/squid
nano palabras-bloqueadas
nano paginas-bloqueadas
ls
cp /etc/squid/squid.conf{,bak}
acl paginas dstdomain “/etc/squid/paginas-bloqueadas”
acl palabras url_regex “/etc/squid/palabras-bloqueadas”
http_access deny paginas
http_access deny palabras
service squid restart
firewall-cmd --add-service=squid –permanent
firewall-cmd --reload
systemctl status squid
//Activar proxy en la MV con la ip del servidor proxy y el puerto 3128