# practica8
La url de la practica 8 es la siguiente:
      https://github.com/antoniobm1/practica8.git

## Maquinas para esta practica
En esta practica vamos a crear 4 maquinas:
      -1 back-end con servidor de base de datos de MYSQL
      -2 Front-end con servidores web de Apache
      -1 Balanceador con servidor web de Apache con proxy inverso
      
## Puertos
Al crear las instancias abriremos los siguientes puertos:
      -ssh que viene por defecto
      -http 
      -https
      -mysql/aurora
      -nfs
      
# Instalacion de Back-end con MYSQL
Crearemos back-end con MYSQL como lo hemos echo anteriormente mediante su script.
Lo unico que cambiaremos sera el nombre, usuario y la contraseña de la base de datos que lo llamaremos wordpress.

# Instalacion de Front-end 1 con Apache servidor
Lo instalamos como hemos echo normalmente con los frontales Apache y despues, descargamos wordpress y lo descomprimimos.
## Concederemos permisos a wordpress
```
chown www-data:www-data * -R
```

## Modificamos el archivo wp-config-example.php 
en el cual pondremos el nombre, usuario y contraseña wordpress, en localhost añadiremos la direccion ip de nuestro back-end para enlazarlo.
Instalamos el servidor nfs-kernel-server

## Editamos el archivo /etc/exports y añadimos la siguiente linea
```
"/var/www/html/wordpress/wp-content      ipfrontalcliente(rw,sync,no_root_squash,no_subtree_check)" > /etc/exports
```
## Configurar wp-config.php
Añadimos las siguientes lineas en el archivo que se encuentra en /var/www/html/worpress
```
Dirección de WordPress (WP_SITEURL): http://IP_BALANCEADOR/wordpress
Dirección del sitio (WP_HOME): http://IP_BALANCEADOR
```
## Descargamos el archivo creado .htaccess que tendra el siguiente contenido
#BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
#END WordPress

## Modificamos el siguiente arcihvo /var/www/html/wordpress/index.php
Cambiamos wp-blog-header.php por wordpress/wp-blog-header.php

# Instalacion Front-end 2 con Apache cliente
Lo instalamos como hemos echo normalmente con los frontales Apache y despues, descargamos wordpress y lo descomprimimos.
## Concederemos permisos a wordpress
```
chown www-data:www-data * -R
```

## Modificamos el archivo wp-config-example.php 
en el cual pondremos el nombre, usuario y contraseña wordpress, en localhost añadiremos la direccion ip de nuestro back-end para enlazarlo.
Instalamos el servidor nfs-common

## Creamos el punto de montaje en el cliente NFS
sudo mount ipfrontalserver:/var/www/html/wordpress/wp-content /var/www/html/wordpress/wp-content

## Editamos el archivo /etc/fstab
Añadimos la siguiente linea
```
ipfrontal1:/var/www/html/wp-content /var/www/html/wp-content  nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
```
## Entramos en el wp-config.php
Se encuentra en la siguiente carpeta:
/var/www/html/wordpress/
Añadimos las siguientes lineas:
Dirección de WordPress (WP_SITEURL): http://IP_BALANCEADOR/wordpress
Dirección del sitio (WP_HOME): http://IP_BALANCEADOR

## Modificamos el siguiente arcihvo /var/www/html/wordpress/index.php
Cambiamos wp-blog-header.php por wordpress/wp-blog-header.php

## Descargamos el archivo creado .htaccess que tendra el siguiente contenido
#BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
#END WordPress

## Entramos en el wp-congif.conf
Borramos las keys ya que son largas y complicadas de aprender

## Añadimos las keys nuevas mediante un generador de keys online
Creamos una variable en la cual metemos la url del generador online y metemos las keys generadas dentro del archivo wp-config.conf

# Instalacion del balanceador
Lo instalamos igual que el balanceador de la practica 5. 

## Editamos el archivo 000-default.conf
Lo editamos y ponemos las ips de los 2 frontales que queremos que balance la carga.
