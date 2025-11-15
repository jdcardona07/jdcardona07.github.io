---
layout: single
title: Comprometiendo el controlador de dominio - Active Directory
excerpt: "De la Explotación Web al Control del Directorio: El Camino Completo para un Ataque Golden Ticket"
date: 2025-11-15
classes: wide
header:
  teaser: /assets/images/activedirectory/dc_1.png
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

![](/assets/images/activedirectory/dc_1.png)


## Descripción:

Este ejercicio simula un escenario de penetración en una infraestructura corporativa con una red altamente segmentada y una jerarquía de Active Directory (dominios Padre e Hijo). El ejercicio comienza con el acceso inicial a un sistema perimetral, para luego requerir el pivoteo de red a través de un túnel para acceder a la red interna y sus sistemas críticos. El objetivo final es la escalada de privilegios a nivel de dominio, aplicando técnicas de post-explotación como la extracción de credenciales (LSA Dump), el movimiento lateral (Pass the Hash), y el forjado de tickets de autenticación maestra para el control completo del Active Directory (Golden Ticket Attack).

## Diagrama de red:

Visualización de la arquitectura de red con dos segmentos (externo e interno), el punto de pivote en el servidor inicial, y el objetivo final: el Controlador de Dominio Padre.

![](/assets/images/activedirectory/dc_2.png)

## Descubrimiento:

Se utiliza nmap para escanear el rango de la red externa (192.168.80.0/24) y descubrir hosts activos.

• nmap -sn 192.168.80.0/24

![](/assets/images/activedirectory/dc_2.png)

Se realiza un escaneo detallado al host 192.168.80.10, revelando que el puerto 80 (HTTP) y el 22 (SSH) están abiertos.

• nmap -sC -sV 192.168.80.10  

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01603.png)

Se accede al servidor web en 192.168.80.10 y se identifica la página de registro (Sign Up), necesaria para buscar el punto de entrada de la explotación.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

Tras registrar y autenticarse con credenciales aleatorias, se accede exitosamente al sitio el cual es una tienda.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

Un campo interesante que encontramos fue el campo de correo electrónico del boletín.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

Se utiliza Burp Suite para interceptar la solicitud POST del formulario y revisar el parámetro EMAIL.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

## Explotación:

Al inyectar el comando cat /etc/passwd, el servidor ejecuta el RCE y devuelve el contenido del archivo, revelando el usuario privilege para el acceso inicial.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

Iniciamos sesión por ssh en la máquina, con las credenciales descubiertas en el Archivo “/etc/passwd”.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

Dentro del servidor se obseran dos interfaces de red, confirmando acceso a una red interna (192.168.98.0/24).

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

Se comprueba la existencia del directorio .mozilla/firefox en la máquina comprometida para buscar bases de datos de historial y marcadores que puedan contener credenciales.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

Usaremos sqlite3 para acceder a la base de datos de Firefox.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

Encontramos algunas credenciales interesantes en la base de datos de marcadores de Mozilla.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

## Pivote

Tenemos que realizar el pivote ya que 192.168.98.0/24 no es accesible directamente de la red VPN. Utilizaremos ligalo-ng para lo mismo.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

Se transfiere y se ejecuta el agente de Ligolo-ng en el servidor, estableciendo la conexión y el túnel hacia la máquina atacante

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

Se ejecuta el proxy de Ligolo-ng en la máquina atacante y se confirma que el agente se ha conectado exitosamente, listando la nueva sesión de túnel a través de 192.168.80.10.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)

Se confirma que el túnel de Ligolo-ng está operativo al poder hacer ping a la red 192.168.98.0/24.

![](/assets/images/vulnerabilidades/cve-2014-0160/cve-2014-01604.png)








## Referencias:

• https://heartbleed.com/

• https://www.welivesecurity.com/la-es/2014/04/09/5-cosas-debes-saber-sobre-heartbleed/
