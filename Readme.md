Agregar dominio a digital ocean

agregar dns a Gogole domaint y copiamos los dns de diagital ocean luego esperamos.

INSTALAR NODEJS

sudo apt-get update
sudo apt-get install nodejs

sudo apt-get install npm

sudo apt install build-essential

--------------------
nano hello.js
ejeplo con express


instalamos pm2 
sudo npm install pm2@latest -g
pm2 start hello.js



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
pm2 startup systemd


List the applications currently managed by PM2:
pm2 list

Get information about a specific application using its App name:    
pm2 info app_name
pm2 monit

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

sudo apt-get install nginx

y ya desde la url se puede ver el ngnix

sudo apt-get install git

sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsendcrypt

obtener un certificado-----------------------------
sudo service nginx stop


Entramos a la carpeta /opt/letsendcrypt
./letsencrypt-auto certonly --standalone


correo mayrast 

y visionsecuritytech.com y www.vissssss.com

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