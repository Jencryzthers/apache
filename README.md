# Information #

This is an application server image based on Apache (with SSL support) and PHP5. In order to use this image effectively, you'll need to mount:

- /var/www  for your app code (e.g. using "-v /home/jdoe/myapp/:/var/www "), with your document root in a directory named public/ 
- /var/log/apache2, optionally, if you want to store logfiles visibly outside the container
- /etc/ssl, optionally, if you wish to use SSL with real keys

## A note on SSL ##

As per the defaults, Apache will use the bundled "snakeoil" key when serving SSL. Obviously this isn't sufficient or advisable for production, so you'll want to mount your real keys onto /etc/ssl/. If you name them "certs/ssl-cert-snakeoil.pem" and "private/ssl-cert-snakeoil.key", you'll be able to get by with the default config. Otherwise, you'll want to include a revised site definition. If you don't want to use SSL, you can avoid forwarding port 443 when launching the container (see below).

## Simple Examples ##

Assuming you have your app code at /home/jdoe/myapp/, with a public/ directory within it suitable to act as the document root, then the below will be sufficient to serve it. Note that many Docker users encourage mounting data from a storage container, rather than directly from the filesyetem.

- "It works!": `docker run -p 80:80 -p 443:443 -d eboraas ` and browse to the host's IP address using http or https
- Serving your own app, with SSL support: `docker run -p 80:80 -p 443:443 -v /home/jdoe/myapp/:/var/www  -d eboraas `
- ... without SSL support: `docker -p 80:80 -v /home/jdoe/myapp/:/var/www  -d eboraas `
- ... using non-standard ports: `docker -p 8080:80 -p 8443:443 -v /home/jdoe/myapp/:/var/www  -d eboraas `

