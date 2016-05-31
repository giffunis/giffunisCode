---
layout: post
title:  "Instalación de Node, Express y Ruby en Debian 8"
date:   2016-05-31 11:01:17 +0100
categories: Node Express Ruby
---

## Antes de todo. Instalación del comando sudo y agregar nuestro usuario como un usuario con privilegios.
Debido a que acabo de instalar Debian, me veo obligado a instalar y configurar el comando sudo, debido a la comodidad que este supone.

Para instalarlo debemos introducir los siguientes comandos:

    $ su # Para entrar como root
    $ apt-get update # Para actualizar los repositorios.
    $ apt-get install sudo # Instalar el comando sudo.

Una vez instalado, debemos agregar el usuario que queramos para que pueda usar el comando. Para ello, modificamos el siguiente que se encuentra en la siguiente ruta `/etc/sudoers`:

    $ nano /etc/sudoers # Para abrirlo con el editor nano

Añadimos la siguiente línea:

    # User privilege specification
    root    ALL=(ALL:ALL) ALL
    usuario    ALL=(ALL:ALL) ALL # Esta es la línea que añadimos

¡Listo! Guardamos con, ctrl + o, y salimos con, ctrl + x

Ya tenemos nuestro usuario como administrador del sistema.

## Instalación de build-essential

Basta con introducir el siguiente comando:

    $ sudo apt-get install build-essential

## Creación de una carpeta compartida entre una máquina anfitriona (Mac) y una invitada (Debian) en VirtualBox.

Creamos la carpeta a compartir en la máquina anfitriona y luego nos dirigimos a la configuración de la máquina virtual, a la pestaña de Carpetas Compartidas.

En mi caso:

![Imagen de mi caso]({{ site.baseurl }}/img/2016/05/31/VB-img-1.png)

A continuación debemos arrancar la máquina virtual e instalar las Guest-Additions.

### Instalando las Guest-Additions

- Montamos el Cd de las Guest-Additions, Devices -> Insert Guest Additions Image...
- Montamos el cd dentro de la máquina virtual:
      $ mount /dev/cdrom
- Introducimos el comando df para conocer dónde se ha montado el cd:

![/media/cdrom0]({{ site.baseurl }}/img/2016/05/31/VB-img-2.png)

En mi caso fue en ``/media/cdrom0`, entramos a la ruta:

    $ cd /media/cdrom0
    $ ls # Para ver el contenido
    $ sudo sh VboxLinuxAdditions.run # Para instalarlas
    # Si por alguna razón os da error con este estilo:
    # "unable to find the sources of your current Linux kernel.",
    # instalad the Kernel Headers Package con el siguiente comando:
    # $ sudo apt-get install linux-headers-$(uname -r)

A continuación, desmontamos la unidad:

    $ cd
    $ umount /media/cdrom0

Después, añadimos la siguiente línea en el fichero `/etc/fstab` como sudo (Podeís usar vi o nano):

    Nombre_Carpeta_sist_Anf  ruta_donde_montar  vboxsf  uid=1000,gid=1000   0   0

En mi caso he creado una carpeta con el nombre de CarpetaCompartida en la raíz de mi usuario:

    Debian_8_DapsServer  /home/giffunis/CarpetaCompartida  vboxsf  uid=1000,gid=1000   0   0

Por último, debemos introducir la siguiente línea, en `/etc/modules` , para hacer que el módulo vboxsf, se cargue al arrancar el sistema:

    vboxsf

Guardamos y reiniciamos. ¡Listo, ya tenemos la carpeta compartida!

## Instalación de NodeJS, Express y Git
Es tan fácil como escribir:

    $ sudo apt-get install nodejs
    $ sudo apt-get install npm
    $ sudo apt-get install git

Si quieres la última versión de Node y Express, sigue este [link](https://nodejs.org/en/download/package-manager/)

## Instalación de Ruby

Yo uso el gestor RVM y en este [link](https://rvm.io/rvm/install), tenéis la información al respecto de como instalar la última versión estable de Ruby.

Y esto ha sido todo, sé que he instalado más cosas de las que había puesto en el título, pero aprovechando que las necesitaba, quise agregarlo también.
