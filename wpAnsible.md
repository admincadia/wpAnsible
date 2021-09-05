# Pruebas de despliegue de Wordpress con Ansible (junio de 2021)

## Primero instalación clásica sobre RHEL/CentOS

#### Paquetes para instalar:
php-fpm httpd mariadb mariadb-server

### Módulos exigidos por WP:
php-mysqlnd php-opcache php-gd php-xml php-mbstring

#### Se instalan y se reinician los tres servicios

#### Se hace la instalación segura de mysql:
mysql_secure_installation

### Se reinician los servicios de httpd y php-fpm

#### Se hace la excepción permanente en el firewall para http y https:
firewall-cmd --zone=public --permanent --add-service=http
firewall-cmd --zone=public --permanent --add-service=https
firewall-cmd --reload

### Se descarga y se copia el contenido del tar.gz descomprimido de WP al /var/www/html
