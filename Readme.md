# Instalaciones de NodeJS en Digital Ocean

Node.js is an open source JavaScript runtime environment for easily building server-side and networking applications. 

# Install

sudo apt-get update
sudo apt-get install nodejs
sudo apt-get install npm
sudo apt-get install build-essential

# Directorio
Se esta creando el directorio especifico para tener una ubicacion de futuros proyectos.

/var/www/node/

# Ejemplo
Crearemos nuetro primer proyecto en nuestro servidor, nos ubicamos en la carpeta node,
dentro creamos nuestra carpeta del proyecto y nuestro archivo js.

nano hello.js
 
//
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(8080, 'localhost');
console.log('Server running at http://localhost:8080/');
//

ejecutamos node hello.js


abrimos otra consola y vemos si esta levantando correctamente.

curl http://localhost:8080


# Configuracion PM2

- sudo npm install -g pm2

sudo npm install pm2@latest -g
pm2 start hello.js


                          Runtime Edition

        PM2 is a Production Process Manager for Node.js applications
                     with a built-in Load Balancer.

                Start and Daemonize any application:
                $ pm2 start app.js

                Load Balance 4 instances of api.js:
                $ pm2 start api.js -i 4

                Monitor in production:
                $ pm2 monitor

                Make pm2 auto-boot at server restart:
                $ pm2 startup

                To go further checkout:
                http://pm2.io/



configuramos para que se inicie pm2 cada vez que se prenda el cpu
- pm2 startup systemd

List the applications currently managed by PM2:
- pm2 list

Get information about a specific application using its App name:    
pm2 info app_name
pm2 monit

# Configuracion Proxy

## instalacion Ngnix

https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04

- sudo apt-get install nginx

- sudo ufw app list



It is recommended that you enable the most restrictive profile that will still allow the traffic you've configured. Since we haven't configured SSL for our server yet, in this guide, we will only need to allow traffic on port 80.

You can enable this by typing:

- sudo ufw allow 'Nginx HTTP'

You can verify the change by typing: si esta activo cosa que sale desactivado.
buscar solucion en teoria puede ser por el ssl.

- sudo ufw status

Confirmamos como anda nuestro ngnix

- systemctl status nginx


Ngnix esta funcionando desde la su ip publica, pero aun no esta conectado a tu proyectod de node, necesitamos configurar para que apunte a otro puerto, exactamente no se que es lo que se tiene que modificar pero es un archivo.

- sudo nano /etc/nginx/sites-available/default


    location / {
        proxy_pass http://localhost:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        + el try que esta por defecto
    }

- sudo nginx -t
- sudo systemctl restart nginx

Y todo ok.

# Configuracion Domains

entramos a google domains.
IMG 1 2 3 DOMINS



# Certificado SSL

Let's Encrypt is a Certificate Authority (CA) that provides an easy way to obtain and install free TLS/SSL certificates, thereby enabling encrypted HTTPS on web servers. It simplifies the process by providing a software client, Certbot, that attempts to automate most (if not all) of the required steps. Currently, the entire process of obtaining and installing a certificate is fully automated on both Apache and Nginx.



sudo nano /etc/nginx/sites-available/default


How To Secure Nginx with Let's Encrypt on Ubuntu 14.04
OVERVIEW 
- Point domain to server
- install ngnix
- install Let's Encrypt client
- Obtain a certificate
- Configure Nginx to use cert
- Set up certificate auto-renewal

Generaremos un script que generar automaticamente nuestro ceritifacdo antes que caduque
ssh cristian@visionsecuritytech.com
ECDSA key fingerprint is SHA256:F5iaFugz+hWo6MkMKgNNRFgnww3If3ebzhWGoVMAYfA.

Sudo apt-get update

sudo apt-get install git

sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsendcrypt

obtener un certificado-----------------------------
sudo service nginx stop


Entramos a la carpeta /opt/letsendcrypt
./letsencrypt-auto certonly --standalone


Nos pedira el correo :

- mayra.vision.st@gmail.com

aceptamos los terminos

visionsecuritytech.com www.visionsecuritytech.com

 Congratulations! Your certificate and chain have been saved at:
   Your cert will expire on 2019-09-13. Congratulations!: command not found
root@VisionSecurityTech-ubuntu-s-1vcpu-1gb-nyc1-01:/opt/letsendcrypt#    /etc/letsencrypt/live/visionsecuritytech.com/fullchain.pem
-bash: /etc/letsencrypt/live/visionsecuritytech.com/fullchain.pem: Permission denied


sudo nano /etc/nginx/sites-available/default

https://www.youtube.com/watch?v=m9aa7xqX67c


-----------------------------------------

server{
        listen 80;
        server_name visionsecuritytech.com www.visionsecuritytech.com;
        return 301 https://$host$request_uri;
}

server {
#       listen 80 default_server;
#       listen [::]:80 default_server;

        listen 443 ssl;
        server_name visionsecuritytech.com www.visionsecuritytech.com;

        ssl_certificate  /etc/letsencrypt/live/visionsecuritytech.com/fullchain.pem;
        ssl_certificate_key  /etc/letsencrypt/live/visionsecuritytech.com/privkey.pem;

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;

        # SSL configuration
        #
        # listen 443 ssl default_server;


y termine de subir al server

https://www.digitalocean.com/community/tutorials/how-to-set-up-a-node-js-application-for-production-on-ubuntu-16-04


--------------------------

poner la pagina real.

pm2 start arrchivo.js