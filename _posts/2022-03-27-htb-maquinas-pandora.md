---
layout: single
title: MonitorsTwo - Machines - Hack The Box
excerpt: "MonitorsTwo es una máquina de dificultad fácil en la plataforma de HTB. Para acceder debemos explotar una vulnerabilidad en Cacti, accederemos a un contenedor en el que tendremos que elevar privilegios mediante un binario SUID, conseguiremos acceso a la máquina principal crackeando un hash obtenido mediante la enumeración de la base de datos MySQL. Para escalar privilegios en la máquina principal, encontraremos una vulnerabilidad en Docker en la cual podremos ejecutar comandos del contenedor en la máquina principal obteniendo así root gracias a la bash con permisos SUID.."
date: 2023-08-20
classes: wide
header:
  teaser: /assets/images/htb-machines-monitorstwo/monitorstwo.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - hackthebox
  - machines
 
tags:  
  - cve-2022-46169
  - rce
  - hashcat
  - limpeas
  - gtfobins
  
---

![](/assets/images/htb-machines-pandora/pandora.png)

## 1. Enumeración:
Iniciamos con una fase de reconocimiento, identificando los servicios soportados por el servidor. Con Nmap vamos a listar los puertos TCP y la versión de los servicios que están utilizando.

![](/assets/images/htb-machines-pandora/pandora2.png)

Ahora vamos a descubrir los puertos UDP desplegados por el servidor.

![](/assets/images/htb-machines-pandora/pandora3.png)

Al final listamos los puertos encontrados como abiertos.

•	22-TCP, puerto SSH

•	80-TCP, servidor web

•	161-UDP, servidor snmp

## 2. Búsqueda de Vulnerabilidades: 
Realizamos una búsqueda en metasploit de exoloit para el servicio SNMP.

![](/assets/images/htb-machines-pandora/pandora4.png)

Vamos a utilizar el módulo auxiliar de metasploit.

•	“auxiliary/scanner/snmp/snmp_enum” e ingresamos la IP del servidor.

![](/assets/images/htb-machines-pandora/pandora5.png)

Este módulo auxiliar nos arrojara información del servidor como los servicios que están corriendo, así como los puertos entre otra información. Algo interesante que nos arrojo fue un servicio en donde se ve en claro unas credenciales de un usuario.

![](/assets/images/htb-machines-pandora/pandora6.png)

## 3. Explotación:
Utilizamos estas credenciales para conectarnos por ssh al servidor.

![](/assets/images/htb-machines-pandora/pandora7.png)

Ahora iniciaremos un servidor web local para cargar una utilidad “linpeas.sh” la cual es una herramienta que busca posibles rutas de escalada de privilegios locales que podría explotar.

![](/assets/images/htb-machines-pandora/pandora8.png)

Descargamos la herramienta y asignamos permisos de ejecución.

![](/assets/images/htb-machines-pandora/pandora9.png)

Ejecutamos la herramienta.

![](/assets/images/htb-machines-pandora/pandora10.png)

Se logra identificar un servicio que está corriendo por el puerto 80 y solo es accesible localmente.

![](/assets/images/htb-machines-pandora/pandora11.png)

Vamos a realizar un reenvió de puerto con ssh, para que podamos ingresar al servicio web del puerto 80. De esta manera estamos reenviando el puerto 80 de la víctima al puerto 80 de nuestra máquina.

![](/assets/images/htb-machines-pandora/pandora12.png)

Ahora ingresamos al servicio web en donde se tiene un inicio de sesión.

![](/assets/images/htb-machines-pandora/pandora13.png)

Vamos a buscar exploit aplicados a pandora FMS.

![](/assets/images/htb-machines-pandora/pandora14.png)

Nos aprovecharemos de la vulnerabilidad SQL Injection (CVE-2021-32099), buscamos un exploit y encontramos la siguiente URL con la que podremos acceder a la aplicación.

•	 “http://localhost:8000/pandora_console/include/chart_generator.php?session_id=a' UNION SELECT 'a',1,'id_usuario|s:5:"admin";'  as data FROM tsessions_php DONDE '1'='1”

Pegar la URL en nuestro navegar.

![](/assets/images/htb-machines-pandora/pandora15.png)

Luego de cargar de nuevo, evidenciamos como estamos dentro como administrador sin autenticarnos.

![](/assets/images/htb-machines-pandora/pandora16.png)

Ahora vamos a descargar un script para php y que nos genere una shell reversa.

•	 git clone https://github.com/pentestmonkey/php-reverse-shell.git

![](/assets/images/htb-machines-pandora/pandora17.png)

Modificamos los datos de nuestra máquina, así como el puerto de escucha del script.

![](/assets/images/htb-machines-pandora/pandora18.png)

Cargamos el script en la página web.

![](/assets/images/htb-machines-pandora/pandora19.png)

Dejamos el equipo atacante a la escucha en el puerto configurado en el script de php.

![](/assets/images/htb-machines-pandora/pandora20.png)

Ejecutamos el script cargado desde el navegador.

![](/assets/images/htb-machines-pandora/pandora21.png)

Ahora ya tenemos una shell al servidor y podemos leer nuestra flag.

![](/assets/images/htb-machines-pandora/pandora22.png)

## 4. Elevación de Privilegios:
Luego de estar en el servidor, vamos a intentar conectarnos por shh, para ello vamos a crear el fichero donde se almacenará las claves de ssh y crearemos un fichero en donde vamos a copiar una clave valida.

![](/assets/images/htb-machines-pandora/pandora23.png)

Desde nuestra máquina, vamos a generar un par de claves (pública y privada) para la autenticación de ssh.

![](/assets/images/htb-machines-pandora/pandora24.png)

Copiamos el contenido de la clave publica generada.

![](/assets/images/htb-machines-pandora/pandora25.png)

Ingresamos ese valor dentro del fichero creado anteriormente.

![](/assets/images/htb-machines-pandora/pandora26.png)

Ahora ya podremos conectarnos por ssh con el usuario matt.

![](/assets/images/htb-machines-pandora/pandora27.png)

Dentro de los ficheros encontrados, vemos uno que se ejecuta con permisos 4000, además al ejecutarlo, se puede ver que usa el binario “tar” para descomprimirse.

![](/assets/images/htb-machines-pandora/pandora28.png)

Ahora vamos a crear un archivo “tar” en la carpeta /tmp/tar.

![](/assets/images/htb-machines-pandora/pandora29.png)

Agregamos la carpeta /tmp como una ruta y al ejecutar de nuevo el archivo, vamos a tener una sesión como root.

![](/assets/images/htb-machines-pandora/pandora30.png)
