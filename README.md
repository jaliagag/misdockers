# Docker con el pelado nerd

Comandos:

- docker --version
- docker ps
- docker exec -it `<pod id>` : ejecutar un comando dentro de un pod que ya está corriendo
- docker pull `<image>` : `docker pull ubuntu`
- docker run -ti `<image>` : `docker run ubuntu`
- docker ps -a : mostrar pot anteriores
- docker commit `<pod id>` `<name>`:`<version>`
- docker run `<image>`:`<version>` `<command>` `<arguments>`
