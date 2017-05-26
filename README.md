# Apache

## MODIFICAMOS:
~
sudo nano /etc/network/interfaces
~

## MODIFICAMOS:
~
source /etc/network/interfaces.d/*

The loopback network interface
auto lo
iface lo inet loopback

The primary network interface
auto enp0s3
(iface enp0s3 inet dhcp)
iface enp0s3 inet static
      address 192.168.1.230   = ip que quiere asignarle
      netmask 255.255.255.0   = mascara de red, por defecto 255.255.255.0
      gateway 192.168.1.254   = La ip re tu router
      dns-nameservers 192.168.1.230   = ip del servidor de DNS. En mi caso le pongo la misma que address porque este pc va a actuar de servidor de DNS.
~

## Reiniciamos la tarjeta de red:
~
sudo /etc/init.d/networking restart
~
# 2 Instalar y Configurar Servidor Dns

### 2a. Instalar Servidor Dns

Para instalar el servidor de DNS usamos el siguiente comando:
~
sudo apt-get install bind9
~
### 2b. Configurar Servidor Dns

## CONFIGURAMOS
~
sudo nano /etc/bind/named.conf.local
~
### Con la siguiente configuracion:
~
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

~
### CONFIGURAMOS:
~
sudo nano /etc/bind/rd.gato.com
~

### Con la siguiente configuracion:
~

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
~

### CONFIGURAMOS:
~
sudo nano /etc/bind/rd.mosquito.com
~

### Con la siguiente configuracion:
~

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
~

### CONFIGURAMOS:
~
sudo nano /etc/bind/rd.escherichiacoli.es
~

### Con la siguiente configuracion:

~

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
~

### CONFIGURAMOS:
~
sudo nano /etc/bind/rd.chip555.org
~
### Con la siguiente configuracion:
~

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
~

### CONFIGURAMOS
~
sudo nano /etc/bind/ri.192.168.1
~

### Con la siguiente configuracion:
~

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
~

### REINICIAMOS EL SERVICIO:
~

sudo /etc/init.d/bind9 restart
~

# 3. Instalar y Configurar Apache2
~

sudo apt-get install apache2
~

Crear una estructura de directorios que alojará los datos del sitio que vamos a proporcionar a nuestros visitantes.
~

sudo mkdir -p /var/www/gato.com/html
sudo mkdir -p /var/www/mosquito.com/html
sudo mkdir -p /var/www/escherichiacoli.es/html
sudo mkdir -p /var/www/chip555.org/html
Creamos los sitios web
~

## Creamos el index de gato.com
~
sudo nano /var/www/gato.com/html/index.html
~

<html>
        <head>
                <title>Gato</title>
        </head>
        <body>
                <img src="https://www.google.es/search?q=gato&source=lnms&tbm=isch&sa=X&ved=0ahUKEwj1--2dsI3UAhUBVhoKHQWzCyAQ_AUICigB&biw=1242&bih=602#imgrc=TZi79VCujnUptM:"/>
        </body>
</html>

## Creamos el index de mosquito.com
~
sudo nano /var/www/mosquito.com/html/index.html
~

<html>
      <head>
              <title>Mosquito</title>
      </head>
      <body>
              <img src="https://www.google.es/imgres?imgurl=http%3A%2F%2Fwww.conmishijos.com%2Fassets%2Fposts%2F7000%2F7918-mosquito.jpg&imgrefurl=http%3A%2F%2Fwww.conmishijos.com%2Focio-en-casa%2Fdibujos-para-colorear%2Fdibujos-m%2Fmosquito.html&docid=CBiX-I_0akJ7VM&tbnid=F8X6XM26tUGUYM%3A&vet=10ahUKEwj5rM7fsI3UAhVH0xoKHSyEDqIQMwg3KAAwAA..i&w=800&h=600&bih=602&biw=1242&q=mosquito%20dibujo%20&ved=0ahUKEwj5rM7fsI3UAhVH0xoKHSyEDqIQMwg3KAAwAA&iact=mrc&uact=8"/>
      </body>
</html>

## Creamos el index de escherichiacoli.es
~
sudo nano /var/www/escherichiacoli.es/html/index.html
~

<html>
        <head>
                <title>Escherichiacoli</title>
        </head>
        <body>
                <img src="https://www.google.es/search?q=escherichiacoli&source=lnms&tbm=isch&sa=X&ved=0ahUKEwjDqOPGsI3UAhVJWhoKHTSfCEwQ_AUICigB&biw=1242&bih=602#imgrc=8T7DpzrNhJaNwM:"/>
        </body>
</html>

## Creamos el index de chip555.org
~
sudo nano /var/www/chip555.org/html/index.html
~
<html>
        <head>
                <title>Chip555</title>
        </head>
        <body>
                <img src="https://www.google.es/imgres?imgurl=https%3A%2F%2Fupload.wikimedia.org%2Fwikipedia%2Fcommons%2Fthumb%2F2%2F21%2FSignetics_NE555N.JPG%2F245px-Signetics_NE555N.JPG&imgrefurl=https%3A%2F%2Fes.wikipedia.org%2Fwiki%2FCircuito_integrado_555&docid=5jxo_Gp7yM4tdM&tbnid=5wjvcVcDcuEsmM%3A&vet=10ahUKEwiygInusI3UAhXMORoKHTovDucQMwgiKAAwAA..i&w=245&h=184&bih=602&biw=1242&q=chip%20555&ved=0ahUKEwiygInusI3UAhXMORoKHTovDucQMwgiKAAwAA&iact=mrc&uact=8"/>
        </body>
</html>

Apache viene con un archivo virtual host por defecto llamado 000-default.conf. Vamos a copiarlo para crear un archivo virtual host para cada uno de nuestros dominios.
~
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/gato.com.conf
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/mosquito.com.conf
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/escherichiacoli.es.conf
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/chip555.org.conf
Configuraremos los ficheros virtual host
~

### Sitio de gato.com
~

sudo nano /etc/apache2/sites-available/gato.com.conf
~ <VirtualHost *:80> ServerAdmin admin@gato.com ServerName gato.com ServerAlias www.gato.com DocumentRoot /var/www/gato.com/html ErrorLog ${APACHE_LOG_DIR}/error.log CustomLog ${APACHE_LOG_DIR}/access.log combined ~
~

### Sitio de mosquito.com
~
sudo nano /etc/apache2/sites-available/mosquito.com.conf
~ <VirtualHost *:80> ServerAdmin admin@mosquito.com ServerName mosquito.com ServerAlias www.mosquito.com DocumentRoot /var/www/mosquito.com/html ErrorLog ${APACHE_LOG_DIR}/error.log CustomLog ${APACHE_LOG_DIR}/access.log combined ~
~

### Sitio de escherichiacoli.es
~
sudo nano /etc/apache2/sites-available/escherichiacoli.es.conf
~ <VirtualHost *:80> ServerAdmin admin@escherichiacoli.es ServerName escherichiacoli.es ServerAlias www.escherichiacoli.es DocumentRoot /var/www/escherichiacoli.es/html ErrorLog ${APACHE_LOG_DIR}/error.log CustomLog ${APACHE_LOG_DIR}/access.log combined ~
~

### Sitio de chip555.org
~
sudo nano /etc/apache2/sites-available/chip555.org.conf
~ <VirtualHost *:80> ServerAdmin admin@chip555.org ServerName chip555.org ServerAlias www.chip555.org DocumentRoot /var/www/chip555.org/html ErrorLog ${APACHE_LOG_DIR}/error.log CustomLog ${APACHE_LOG_DIR}/access.log combined ~
~

## Habilitamos los sitios web que hemos creado:
~
sudo a2ensite gato.com.conf
sudo a2ensite mosquito.com.conf
sudo a2ensite escherichiacoli.es.conf
sudo a2ensite chip555.org.conf
~
## Ahora desactivamos el sitio por defecto de apache2:
~
sudo a2dissite 000-default.conf
~
## Reiniciamos el servicio:
~
sudo /etc/init.d/apache2 restart
~
# 3.1 Acceso con usuario y contraseña a las paginas escherichiacoli.es y chip555.org

Necesitaremos crear un archivo de contraseñas. Éste archivo debería colocarlo en algún sitio no accesible mediante la Web. Para crear un archivo de contraseñas, usaremos la utilidad htpasswd que viene con Apache. Para crear el archivo:
~
sudo htpasswd -c /var/www/sitioa.com/passwords user1
sudo htpasswd -c /var/www/sitioa.com/passwords user2
sudo htpasswd  /var/www/sitioa.com/passwords user3
~

La opción '-c' se utiliza para crear el fichero.

### Añadiremos a nuestro fichero de configuracion de apache:
~
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
  ~
Si sustituimos 'Require user user1' por 'Require valid-user ', tendrán acceso todos los usuarios del fichero passwords.

## REINICIAMOS:
~
sudo /etc/init.d/apache2 restart
~

chip-banco-555-01.jpg
