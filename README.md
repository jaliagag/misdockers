# Docker con el pelado nerd

Comandos:

- docker --version
- docker ps
- docker exec -it `<pod id>` : ejecutar un comando dentro de un pod que ya está corriendo
- docker pull `<image>` : `docker pull ubuntu`
- docker run -it `<image>` : `docker run ubuntu`
- docker ps -a : mostrar pods anteriores
- docker commit `<pod id>` `<name>`:`<version>`
- docker run `<image>`:`<version>` `<command>` `<arguments>`

Docker file: sirve para crear imágenes - se tiene que basar en una imágen.

```yaml
FROM centos

RUN yum update -y && yum install figlet -y
```
