## Instalación clásica sobre Debian 10

#### Paquetes para instalar:
apache2 libapache2-mod-php7.4 php7.4 php7.4-curl php7.4-mysql mariadb-server

#### Módulos de php exigidos por WP
php7.4-bcmath php7.4-curl php7.4-imagick php7.4-gd php7.4-mbstring php7.4-xml php7.4-zip

##### Para el php 7.4 es necesario actualizar los repos:
sudo nano /etc/apt/sources.list.d/php-sury.org.list
deb http://packages.sury.org/php/ buster main
sudo wget -O /etc/apt/trusted.gpg.d/php-sury.org.gpg https://packages.sury.org/php/apt.gpg

### Configuración de iptables para http y https:
sudo ufw allow http
sudo ufw allow https

### Instalación por defecto de MySQL:
sudo mysql_secure_installation

### Se descarga y se copia el contenido del tar.gz descomprimido de WP al /var/www/html
La última versión suele estar en https://es.wordpress.org/latest-es_ES.tar.gz (comprobar)
#### El tar.gz incluye un directorio 'wordpress', que quedará como directorio del sitio web:
sudo tar xf latest-es_ES.tar.gz -C /var/www/html

### Se cambia la propiedad del sitio web /var/www/html/wordpress al demonio de apache:
sudo chown www-data:www-data /var/www/html/wordpress/ -R

### Se crea base de datos y usuario para WP, y se le da un GRANT ALL:
create database wordpress charset utf8mb4 collate utf8mb4_unicode_ci;
create user wpadmin@localhost identified by '1234';
grant all privileges on wordpress.* to wpadmin@localhost;
flush privileges;
exit;

### Carga de módulo Rewrite de apache:
sudo a2enmod rewrite

### Fichero de configuración de Apache para WP:
sudo vim /etc/apache2/conf-available/wordpress.conf
#### Contenido:
    <Directory /var/www/html/wordpress>
          AllowOverride all
    </Directory>

### Se activa la nueva configuración para WP y se rebota Apache:
sudo a2enconf wordpress
sudo service apache2 restart
