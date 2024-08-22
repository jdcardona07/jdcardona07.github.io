---
layout: single
title: GreenHorn - Machines - Hack The Box
excerpt: "greenhorn es una máquina de nivel facil, con sistema operativo Linux que tiene una vulnerabilidad de shell remoto en una aplicación web Pluck que lleva a la toma total del control del sistema."
date: 2024-08-14
classes: wide
header:
  teaser: /assets/images/htb-machines-greenhorn/greenhorn1.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - hackthebox
  - machines
 
tags:  
  - CrackStation
  - Depix
  - Pluck
  - cve-2023-50564
     
---

![](/assets/images/htb-machines-greenhorn/greenhorn1.png)

greenhorn es una máquina de nivel facil, con sistema operativo Linux que tiene una vulnerabilidad de shell remoto en una aplicación web Pluck que lleva a la toma total del control del sistema.

## 1. Enumeración 

Iniciamos con un NMAP para buscar puertos y servicios abiertos en el objetivo. Identificamos dos puertos abiertos (80 y 3000).

•	HTTP (80)

•	HTTP (3000)

![](/assets/images/htb-machines-greenhorn/greenhorn2.png)

Al ingresar al servicio por el puerto 80, nos redirige al dominio “greenhorn.htb”.

![](/assets/images/htb-machines-greenhorn/greenhorn3.png)

Vamos a agregar este dominio a nuestro archivo "etc/hosts".

![](/assets/images/htb-machines-greenhorn/greenhorn4.png)

Ahora intentamos ver el sitio web, ya podemos visualizar el contenido. Ahora vamos a explorar el sitio. Vemos un menú admin.

![](/assets/images/htb-machines-greenhorn/greenhorn5.png)

## 2.	Acceso Inicial

Luego de darle clic nos lleva a un sitio de login el cual está ejecutando Pluck en versión 4.7.18.

![](/assets/images/htb-machines-greenhorn/greenhorn6.png)

Realizando una búsqueda en internet, encontramos que Pluck 4.7.18 sufre una vulnerabilidad de shell remoto.
•	https://packetstormsecurity.com/files/173640/Pluck-4.7.18-Remote-Shell-Upload.html?source=post_page-----e87e1cc07864--------------------------------
Para poder explotar esta vulnerabilidad, primero necesitamos una contraseña. Vamos a explorar el servicio que se ejecuta en el puerto 3000.

![](/assets/images/htb-machines-greenhorn/greenhorn7.png)

Encontramos que dentro de la ruta “/explore” tenemos un repositorio “GreenAdmin”.

![](/assets/images/htb-machines-greenhorn/greenhorn8.png)

Explorando un poco el repositorio, observamos un archivo “pass.php” el cual contiene un hash de una contraseña.

![](/assets/images/htb-machines-greenhorn/greenhorn9.png)

Intentamos romper el hash con el servicio en línea CkarckStation.

• https://crackstation.net/

![](/assets/images/htb-machines-greenhorn/greenhorn10.png)

Ahora con la contraseña en texto claro, probamos en el menú de login.

![](/assets/images/htb-machines-greenhorn/greenhorn11.png)

Ahora, nuestro objetivo es encontrar un lugar donde podamos explotar la vulnerabilidad. Observamos que para la vulnerabilidad de Pluck, debemos intentar instalar un módulo el cual nos permite cargar únicamente un archivo ZIP.

![](/assets/images/htb-machines-greenhorn/greenhorn12.png)

Ahora lo que necesitamos es crear un archivo ZIP que contenga un shell reverse.

![](/assets/images/htb-machines-greenhorn/greenhorn13.png)

## 3.	Explotación

Vamos a utilizar el siguiente exploit que encontramos, el cual explota la vulnerabilidad de Pluck.

• https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php?source=post_page-----e87e1cc07864--------------------------------

Creamos un archivo PHP copiando el exploit del link.

![](/assets/images/htb-machines-greenhorn/greenhorn14.png)

Modificamos el archivo, ingresando nuestra dirección IP (VPN) de la máquina virtual.

![](/assets/images/htb-machines-greenhorn/greenhorn15.png)

Como solo carga archivos .zip, vamos a cambiar el tipo de archivo.

![](/assets/images/htb-machines-greenhorn/greenhorn16.png)

Adicionalmente vamos a poner nuestra maquina virtual a la escucha por el mismo puerto del exploit con Netcat para recibir la conexión.

![](/assets/images/htb-machines-greenhorn/greenhorn17.png)

Intentamos cargar nuestro archivo exploit.

![](/assets/images/htb-machines-greenhorn/greenhorn18.png)

Cargamos la página en donde se almacenas los archivos cargados con el objetivo de que nuestro exploit se active.

![](/assets/images/htb-machines-greenhorn/greenhorn19.png)

Hemos recibido una Shell, pero tenemos acceso restringido.

![](/assets/images/htb-machines-greenhorn/greenhorn20.png)

Vamos a cambiar por una Shell mas interactiva con el comando.

• /usr/bin/python3 -c 'import pty; pty.spawn("/bin/bash")'

![](/assets/images/htb-machines-greenhorn/greenhorn21.png)

Con la contraseña obtenida en los pasos anteriores intentamos autenticarnos con el usuario “junior” identificado para poder capturar nuestra primera bandera de usuario estándar.

![](/assets/images/htb-machines-greenhorn/greenhorn22.png)

## 3. Escalada de privilegios

Necesitamos escalar privilegios para poder llegar a la bandera del usuario “root”. Vamos a identificar archivos ocultos.

![](/assets/images/htb-machines-greenhorn/greenhorn23.png)

Encontramos un archivo PDF que vamos a transferir a nuestra máquina virtual para verlo con más detalles. Levantamos un servidor web.

![](/assets/images/htb-machines-greenhorn/greenhorn24.png)

Desde nuestra maquina virtual vamos a transferirlo.

![](/assets/images/htb-machines-greenhorn/greenhorn25.png)

Visualizamos el PDF en donde se encuentra una contraseña, pero pixelada.

![](/assets/images/htb-machines-greenhorn/greenhorn26.png)

Vamos a hacer uso de una herramienta para convertir el documento pixelado en una imagen.

• https://tools.pdf24.org/en/extract-images?source=post_page-----e87e1cc07864--------------------------------#s=1723656608236

![](/assets/images/htb-machines-greenhorn/greenhorn27.png)

Ahora con el trozo de la imagen pixelada vamos a trabajar con la herramienta Depix para despixelar imágenes.

• https://github.com/spipm/Depix?source=post_page-----e87e1cc07864--------------------------------
Necesitamos clonar el repositorio e ingresamos a la carpeta descargada.

![](/assets/images/htb-machines-greenhorn/greenhorn28.png)

Luego vamos a ejecutar el comando que se especifica en el Git con el cual intentaremos despixelar la imagen, apuntando a la ruta de la imagen y en donde se guardaría la nueva imagen despixelada.

![](/assets/images/htb-machines-greenhorn/greenhorn29.png)

Finalizo la herramienta y nos genera la siguiente imagen, en ella identificamos el siguiente texto.

• side------------------------------------------------side

![](/assets/images/htb-machines-greenhorn/greenhorn30.png)

Usamos la contraseña para autenticarnos como root y capturar nuestra segunda bandera.

![](/assets/images/htb-machines-greenhorn/greenhorn31.png)
