# Docker con el pelado nerd

Comandos:

- docker --version
- docker ps
- docker exec -it `<pod id>` : ejecutar un comando dentro de un pod que ya está corriendo
- docker run: ejecuta un contenedor nuevo
- docker pull `<image>` : `docker pull ubuntu`
- docker run -it `<image>` : `docker run ubuntu`
- docker ps -a : mostrar pods anteriores
- docker commit `<pod id>` `<name>`:`<version>`
- docker run `<image>`:`<version>` `<command>` `<arguments>`
- docker build -t `<dockerfile>`:`<version>` .
- NGINX: docker run --detach --publish 80:80 --name webserver nginx --> una vez descargada la imagen y el contenedor corriento, ir a <http://localhost>.
- Detener el webserver: docker container stop webserver
- Eliminar contenedores: docker container rm `<nombre>`

Docker file: sirve para crear imágenes - se tiene que basar en una imágen.

```yaml
FROM centos

RUN yum update -y && yum install figlet -y
```

Buena práctica: especificar la versión.

## Volúmenes

En docker podés montar un directorio o un archivo que está en el host y meterlo dentro del contenedor. Si modificas el archivo desde tu máquina esos cambios se ven dentro del contenedor.

Poner el archivo en read only, es una buena práctica para que los procesos del contenedor no escriban en el archivo.

Mandar un archivo al nginx: `docker run -v C:\Users\jaliaga\dockerFiles\index.html:/usr/share/nginx/html/index.html:ro -d nginx:1.15.7`

## Puertos

Para exponer el puerto de nginx al mundo usamos el flag `-p`. -p 8080:80 --> que el puerto 8080 de mi máquina apunte al contenedor

-d --> dejarlo corriendo en el background. `docker run -d nginx:1.15.7`. Para que esta flag funcione, tenemos que correr un comando que deje un servicio corriendo, no que devuelva algo y listo.

## Conectar contenedores

- MySQL como DB
- Wordpress

No se suele construir de cero una imagen; se suele descargar una imagen configurada y se le pasan variables de entorno la configuración que necesitamos.

-e: crea una variable de entorno. Cuando se inicia el contenedor, lee las variables de entorno y se fija si existe esta variable de entorno. Si la encuentra, va a tomar el valor de esa variable de entorno y va a configurar la clave de root con ese valor.

Es buena práctica pasar las configuraciones fuera del contenedor en forma de variables de entorno

-v: agregar un volumen. La base de datos se va a guardar ahí. Si no especifico este volumen el contenedor, cuando se muera, va a borrar todos los datos; de esta manera, los datos van a ser persistentes.

```docker
docker run -e MYSQL_ROOT_PASSWORD=miclave -e MYSQL_DATABASE=midb -v ~/mysql-data:/var/lib/mysql -d mysql:8.0.13
```

Para que no quede todo en una línea, usamos dockercompose. Este archivo contiene todas las configuraciones necesarias para crear contenedores

```yaml
version: '1.0'

services:
	wordpress:
		image: wordpress:php7.2-apache
		ports:
			- 8080:80
		environment:
			WORDPRESS_DB_HOST: mysql
			WORDPRESS_DB_USER: root
			WORDPRESS_DB_PASSWORD: root
			WORDPRESS_DB_NAME: wordpress
		links:
			- mysql:mysql

	mysql:
		image: mysql:8.0.13
		command: --default-authentication-plugin=mysql_native_password
		environment:
			MYSQL_DATABASE: wordpress
			MYSQL_ROOT_PASSWORD: root
		volumes:
			- ~/docker/mysql-data:/var/lib/mysql
```

Levantar contenedores en un docker compose

Levantar servidor NGINX en docker:

- `<archivo .yaml>` ejemplo docker-compose up -d

1. docker run -v `<archivo index.html o folder>`:/usr/share/nginx/html/index.html:ro -p 8080:80 -d nginx:1.15.7

What is a webserver: a piece of software that runs on a server and puts all the files belonging to a webpage/API (stored on the server the webserver is running on) together and sends it to your browser.

When entering an URL, the browser sends a request over the internet to another machine that has a webserver running which in turn returns and renders the page in the browser.

- APACHE: Power and flexibility and do a variety of things
- NGINX: fast
