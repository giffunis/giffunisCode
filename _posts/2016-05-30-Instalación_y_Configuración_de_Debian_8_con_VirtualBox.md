# Instalación y Configuración de Debian 8 con VirtualBox.

## Introducción.

Esta es la Primera de una serie de entradas, que utilizaré para llevar a cabo un seguimiento de mi Trabajo Final de Grado.

En esta ocasión, se trata de la instalación de Debian 8 en VirtualBox, en un sistema anfitrión Mac. Doy por echo que la herramienta de virtualización, VirtualBox, es conocida y que la sabeis usar, no obstante, es un tutorial que si lo seguís podreís completarlo sin previo conocimiento.

## Requisitos.
1. Imagen Iso de la distribución Debian. En este caso, usaremos la *NetInst x64* que podemos descargar pinchando [aquí](https://www.debian.org/distrib/)
2. Virtualbox. [Página oficial](https://www.virtualbox.org/).
3. Paciencia, ganas de aprender y un poco de cafe a mano ;).

## Creación de la máquina virtual.
Vamos a crear una máquina virtual básica y en la que haremos una instalación limpia de Debian 8, luego, esta máquina la utilizaremos como template y la clonaremos, así, si luego necesitaremos un nuevo entorno para otro proyecto, no necesitaremos perder tiempo instalando Debian de nuevo. Empecemos.

1. Clic en Nuevo, he introducimos el nombre y la versión de linux a instalar:
![Imagen 1]({{ site.base-url }}/img/VB-imagen-1.png)

2. Configuramos la máquina virtual con 512mb de Ram y creamos un disco virtual(VDI), reservado dinamicamente de 40GB.
3. Arrancamos la máquina virtual y seleccionamos la ruta a la imagen ISO que hemos descargado previamente.
![Imagen 2]({{ site.base-url }}/img/VB-imagen-2.png)

4. Iniciamos la instalación de manera normal (En mi caso escogí la opción por defecto, no la instalación gráfica). La configuración es a gusto del lector.
5. Cuando nos aparezca la pantalla de elección de los programas a instalar, elegimos unicamente: SSH Server y las utilidades estandar del sistema. Para elegir o eliminar una opción utilizaremos la barra espaciadora. para aceptar usaremos la tecla enter. Debería quedar así (El puntero en rojo es indiferente):
![Imagen 3]({{ site.base-url }}/img/VB-imagen-3.png)

6. Una vez instalado el sistema operativo, apagamos la máquina virtual para clonarla.

## Clonación de la máquina virtual.
Es tan fácil como hacer clic con el botón derecho del ratón y elegir: clonar, reinicializar las mac de las tarjetas de red y elegir una clonación completa.
![Imagen 4]({{ site.base-url }}/img/VB-imagen-4.png)

## Configuración del reenvío de puertos y del fichero Config de la máquina cliente.

Elegimos la máquina virtual que nos ha clonado y accedemos a configuración, nos vamos al apartado de red, hacemos click en avanzado y en reenvío de puertos.

![Imagen 5]({{ site.base-url }}/img/VB-imagen-5.png)

Ahí, configuramos los puertos 22 para la conexión SSH y el 3000 para el servidor node.

![Imagen 6]({{ site.base-url }}/img/VB-imagen-6.png)

El siguiente paso es el crear un fichero config, dentro de la carpeta ssh que se encuentra en la raíz del usuario: `Directorio raíz del usuario/.ssh/config` para así poder conectarnos mediante una terminal, y poder copiar y pegar comandos, además de ser mucho más cómodo con el tiempo.

Dentro del fichero `config` añadimos nuestra máquina virtual, con la siguiente estructura:

	#DAPS SERVER - VIRTUAL MACHINE  (Es un comentario)
	Host daps #Nombre de la máquina que queramos y que usaremos para conectarnos con ssh. Ej: ssh daps
	User usuario #Nombre del usuario de la máquina.
	Hostname localhost #Al tratarse de una conexión NAT con el reenvío de puertos.
	Port 2222 	#Puerto por el que conectarse, en este caso, el configurado previamente.

Arrancamos nuestra máquina virtual e iniciamos con el usuario que creamos en la instalación, a continuación escribimos el siguiente comando, que nos mostrará si el servidor ssh está escuchando por el puerto 22:

	$ netstat -tuna

Si el servicio está activado, devería aparecer una fila como esta:

![Imagen 7]({{ site.base-url }}/img/VB-imagen-7.png)

Ahora, podemos ejecutar en una terminal de nuestra máquina anfitrina el siguiente comando, para conectarnos a la máquina virtual:

	$ ssh nombre_host_que_aparece_en_el_fichero_config
	En mi caso sería así:
	$ ssh daps

Si hemos realizado la configuración correcta nos pedirá confirmar la autenticidad del host. y pondremos nuestra contraseña. ¡Y tenemos nuestro terminal conectado a nuestra máquina invitada!

![Imagen 8]({{ site.base-url }}/img/VB-imagen-8.png)
