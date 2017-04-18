# Apache

sudo nano /etc/network/interfaces
### This file describes the network interfaces available on your system
### and how to activate them. For more information, see interfaces(5).

##MODIFICAMOS:
###source /etc/network/interfaces.d/*

### The loopback network interface
auto lo
iface lo inet loopback

### The primary network interface
auto enp0s3
(iface enp0s3 inet dhcp)
iface enp0s3 inet static
      address 192.168.1.230   = ip que quiere asignarle
      netmask 255.255.255.0   = mascara de red, por defecto 255.255.255.0
      gateway 192.168.1.254   = La ip re tu router
      dns-nameservers 192.168.1.230   = ip del servidor de DNS. En mi caso le pongo la misma que address porque este pc va a actuar de servidor de DNS.


## Reiniciamos la tarjeta de red:
sudo /etc/init.d/networking restart

# 2 Instalar y Configurar Servidor Dns

### 2a. Instalar Servidor Dns

Para instalar el servidor de DNS usamos el siguiente comando:
sudo apt-get install bind9
### 2b. Configurar Servidor Dns

## CONFIGURAMOS
sudo nano /etc/bind/named.conf.local
### Con la siguiente configuracion:

//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "gato.com"{
      type master;
      file "/etc/bind/rd.gato.com";
};

zone "mosquito.com"{
      type master;
      file "/etc/bind/rd.mosquito.com";
};

zone "escherichiacoli.es"{
      type master;
      file "/etc/bind/rd.escherichiacoli.es";
};

zone "chip555.org"{
      type master;
      file "/etc/bind/rd.chip555.org";
};

//resolucion inversa

zone "1.168.192.in-addr.arpa"{
      type master;
      file "/etc/bind/ri.192.168.1";
};
### CONFIGURAMOS:
sudo nano /etc/bind/rd.gato.com
### Con la siguiente configuracion:

$TTL 38400

@ IN SOA servidor01.gato.com. correoadmin.gato.com. (
        2017041000;   //num serie
        28800;        //refresco
        3600;         //reintentos
        604800;       //caducidad
        38400);       // tiempo en caché

@ IN NS servidor01.gato.com.
servidor01.gato.com. IN A 192.168.1.210
WWW IN CNAME servidor01.gato.com.

### CONFIGURAMOS:
sudo nano /etc/bind/rd.mosquito.com
### Con la siguiente configuracion:

$TTL 38400

@ IN SOA servidor01.mosquito.com. correoadmin.mosquito.com. (
        2017041000;   //num serie
        28800;        //refresco
        3600;         //reintentos
        604800;       //caducidad
        38400);       // tiempo en caché

@ IN NS servidor01.mosquito.com.
servidor01.mosquito.com. IN A 192.168.1.210
WWW IN CNAME servidor01.mosquito.com.


### CONFIGURAMOS:
sudo nano /etc/bind/rd.escherichiacoli.es

### Con la siguiente configuracion:

$TTL 38400

@ IN SOA servidor01.escherichiacoli.es. correoadmin.escherichiacoli.es. (
        2017041000;   //num serie
        28800;        //refresco
        3600;         //reintentos
        604800;       //caducidad
        38400);       // tiempo en caché

@ IN NS servidor01.escherichiacoli.es.
servidor01.escherichiacoli.es. IN A 192.168.1.210
WWW IN CNAME servidor01.escherichiacoli.es.

### CONFIGURAMOS:
sudo nano /etc/bind/rd.chip555.org
### Con la siguiente configuracion:

$TTL 38400

@ IN SOA servidor01.chip555.org. correoadmin.chip555.org. (
        2017041000;   //num serie
        28800;        //refresco
        3600;         //reintentos
        604800;       //caducidad
        38400);       // tiempo en caché

@ IN NS servidor01.chip555.org.
servidor01.chip555.org. IN A 192.168.1.210
WWW IN CNAME servidor01.chip555.org.

### CONFIGURAMOS
sudo nano /etc/bind/ri.192.168.1

### Con la siguiente configuracion:

$TTL 38400

@ IN SOA servidor01.sitioa.net. correoadmin.sitioa.net. (
  2017032100;   //num serie
  28800;        //refresco
  3600;         //reintentos
  604800;       //caducidad
  38400);       // tiempo en caché

@ IN NS servidor01.
210 IN PTR servidor01.gato.com.
210 IN PTR servidor01.mosquito.com.
210 IN PTR servidor01.escherichiacoli.es.
210 IN PTR servidor01.chip555.org.

### REINICIAMOS EL SERVICIO:

sudo /etc/init.d/bind9 restart

# 3. Instalar y Configurar Apache2

sudo apt-get install apache2

Crear una estructura de directorios que alojará los datos del sitio que vamos a proporcionar a nuestros visitantes.

sudo mkdir -p /var/www/gato.com/html
sudo mkdir -p /var/www/mosquito.com/html
sudo mkdir -p /var/www/escherichiacoli.es/html
sudo mkdir -p /var/www/chip555.org/html
Creamos los sitios web

## Creamos el index de gato.com

sudo nano /var/www/gato.com/html/index.html

<html>
        <head>
                <title>Un gatito</title>
        </head>
        <body>
                <img src="https://c24e867c169a525707e0-bfbd62e61283d807ee2359a795242ecb.ssl.cf3.rackcdn.com/imagenes/gato/etapas-clave-de-su-vida/gatitos/nuevo-gatito-en-casa/gatito-tumbado-lamiendo-sus-patitas.jpg"/>
        </body>
</html>
## Creamos el index de mosquito.com

sudo nano /var/www/mosquito.com/html/index.html
<html>
      <head>
              <title>Un mosquito</title>
      </head>
      <body>
              <img src="https://previews.123rf.com/images/shock77/shock770906/shock77090600003/5002033-divertidos-dibujos-animados-de-mosquitos-Foto-de-archivo.jpg"/>
      </body>
</html>
## Creamos el index de escherichiacoli.es

sudo nano /var/www/escherichiacoli.es/html/index.html
<html>
        <head>
                <title>escherichiacoli</title>
        </head>
        <body>
                <img src="http://st2.depositphotos.com/4184747/7821/v/950/depositphotos_78210262-stock-illustration-the-structure-of-escherichia-coli.jpg"/>
        </body>
</html>
## Creamos el index de chip555.org

sudo nano /var/www/chip555.org/html/index.html
<html>
        <head>
                <title>Un chip555</title>
        </head>
        <body>
                <img src="https://userscontent2.emaze.com/images/93557bbf-b41d-4c3c-b5aa-a75fa9cf9a00/5726ee35-ce52-4705-880c-04ae2a4ab3de.jpg"/>
        </body>
</html>

Apache viene con un archivo virtual host por defecto llamado 000-default.conf. Vamos a copiarlo para crear un archivo virtual host para cada uno de nuestros dominios.

sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/gato.com.conf
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/mosquito.com.conf
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/escherichiacoli.es.conf
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/chip555.org.conf
Configuraremos los ficheros virtual host

Sitio de gato.com
sudo nano /etc/apache2/sites-available/gato.com.conf
~~~ <VirtualHost *:80> ServerAdmin admin@gato.com ServerName gato.com ServerAlias www.gato.com DocumentRoot /var/www/gato.com/html ErrorLog ${APACHE_LOG_DIR}/error.log CustomLog ${APACHE_LOG_DIR}/access.log combined ~~~

Sitio de mosquito.com
sudo nano /etc/apache2/sites-available/mosquito.com.conf
~~~ <VirtualHost *:80> ServerAdmin admin@mosquito.com ServerName mosquito.com ServerAlias www.mosquito.com DocumentRoot /var/www/mosquito.com/html ErrorLog ${APACHE_LOG_DIR}/error.log CustomLog ${APACHE_LOG_DIR}/access.log combined ~~~

Sitio de escherichiacoli.es
sudo nano /etc/apache2/sites-available/escherichiacoli.es.conf
~~~ <VirtualHost *:80> ServerAdmin admin@escherichiacoli.es ServerName escherichiacoli.es ServerAlias www.escherichiacoli.es DocumentRoot /var/www/escherichiacoli.es/html ErrorLog ${APACHE_LOG_DIR}/error.log CustomLog ${APACHE_LOG_DIR}/access.log combined ~~~

Sitio de chip555.org
sudo nano /etc/apache2/sites-available/chip555.org.conf
~~~ <VirtualHost *:80> ServerAdmin admin@chip555.org ServerName chip555.org ServerAlias www.chip555.org DocumentRoot /var/www/chip555.org/html ErrorLog ${APACHE_LOG_DIR}/error.log CustomLog ${APACHE_LOG_DIR}/access.log combined ~~~

Habilitamos los sitios web que hemos creado con los siguientes commandos
sudo a2ensite gato.com.conf
sudo a2ensite mosquito.com.conf
sudo a2ensite escherichiacoli.es.conf
sudo a2ensite chip555.org.conf

Ahora desactivamos el sitio por defecto de apache2 con el siguiente comando
sudo a2dissite 000-default.conf

Por ultimo reiniciamos el servicio de apache2 con uno de estos comandos:
sudo /etc/init.d/apache2 restart
sudo service apache2 restart

3.1 Acceso con usuario y contraseña a las paginas escherichiacoli.es y chip555.org

Necesitaremos crear un archivo de contraseñas. Éste archivo debería colocarlo en algún sitio no accesible mediante la Web. Para crear un archivo de contraseñas, usaremos la utilidad htpasswd que viene con Apache. Para crear el archivo:

sudo htpasswd -c /var/www/sitioa.com/passwords user1
sudo htpasswd -c /var/www/sitioa.com/passwords user2
sudo htpasswd  /var/www/sitioa.com/passwords user3
La opción '-c' se utiliza para crear el fichero.

Añadiremos a nuestro fichero de configuracion de apache:
sudo nano /etc/apache2/apache2.conf

  <Directory /var/www/escherichiacoli.es/html>
      AuthType Basic
      AuthName "ACCESO RESTRINGIDO."
      AuthUserFile /var/www/escherichiacoli.es/passwords
      Require user user01
      Options Indexes FollowSymLinks MultiViews
      AllowOverride  none
      Order Allow,deny
      allow from all
  </Directory>

  <Directory /var/www/chip555.org/html>
      AuthType Basic
      AuthName "ACCESO RESTRINGIDO."
      AuthUserFile /var/www/chip555.org/passwords
      Require valid-user
      Options Indexes FollowSymLinks MultiViews
      AllowOverride  none
      Order Allow,deny
      allow from all
  </Directory>
Si sustituimos 'Require user user1' por 'Require valid-user ', tendrán acceso todos los usuarios del fichero passwords.

Reiniciamos el demonio y sólo tendrá acceso el user1 a la pagina escherichiacoli.es y a la pagina chip555.org tendran acceso todos los usuarios del archivo passwords situado en /var/www/chip555.org/passwords para reiniciar el servicio/demonio uno de estos comandos.
sudo /etc/init.d/apache2 restart
sudo service apache2 restart
