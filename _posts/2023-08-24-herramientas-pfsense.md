---
layout: single
title: Instalación pfSense - Firewall - Herramientas
excerpt: "​pfSense es una distribución personalizada de FreeBSD adaptado para su uso como Firewall y Enrutador. Se caracteriza por ser de código abierto, puede ser instalado en una gran variedad de ordenadores, y además cuenta con una interfaz web sencilla para su configuración."
date: 2023-08-24
classes: wide
header:
  teaser: /assets/images/herramientas/pfsense.png
  teaser_home_page: true
  icon: /assets/images/logo_pfsense.webp
categories:
  - firewall
 
tags:  
  - firewall
  
---

![]( /assets/images/herramientas/pfsense.png)

pfSense es una distribución personalizada de FreeBSD adaptado para su uso como Firewall y Enrutador. Se caracteriza por ser de código abierto, puede ser instalado en una gran variedad de ordenadores, y además cuenta con una interfaz web sencilla para su configuración.

## 1.  Introducción
Esta guía presenta un manual de instalación básica de Pfsense en una maquina virtual en el entorno VMware.

## 2. Descarga de Imagen ISO de Pfsense
Vamos a descargar desde el sitio oficial de Pfsense en el siguiente enlace  https://www.pfsense.org/download/, la imagen en formato ISO la versión mas reciente y estable de Pfsense.

## 3. Configuración de máquina virtual VMware
Parala instalación de Pfsensse, vamos a necesitar una maquina virtual con 2 interfaces de red para simular la red WAN y LAN, también vamos a asignarle 2 GB de Memoria RAM y 20 GB de espacio en disco duro, aunque el sistema podría trabajar con menos de eso.

![]( /assets/images/herramientas/pfsense2.png)

Luego de configurar las características de hardware y de cargar la imagen ISO que descargamos, vamos a iniciar la máquina virtual. Inicialmente debemos aceptar el copyright que nos muestra.

![]( /assets/images/herramientas/pfsense3.png)

Seleccionamos la opción de instalar, ya que cuenta con otras opciones como de recuperación en caso de una falla.

![]( /assets/images/herramientas/pfsense4.png)

Seleccionamos ZFS como sistema de archivos.

![]( /assets/images/herramientas/pfsense5.png)

Confirmamos seguir con la instalación.

![]( /assets/images/herramientas/pfsense6.png)

Escogemos el tipo de arreglo de disco, para nuestro laboratorio es suficiente la primera opcion

![]( /assets/images/herramientas/pfsense7.png)

Seleccionamos el disco virtual asignado.

![]( /assets/images/herramientas/pfsense8.png)

Confirmamos el formateo del disco donde será instalado Pfsense.

![]( /assets/images/herramientas/pfsense9.png)

Finalmente reiniciamos el sistema.

![]( /assets/images/herramientas/pfsense10.png)

Luego de iniciar el sistema, tendremos el menú donde nos muestra las direcciones ip LAN y WAN.

![]( /assets/images/herramientas/pfsense11.png)

Desde un equipo en la misma subred de la interfaz LAN de Pfsense, vamos a poder ingresar desde un navegador web, a la interfaz web de Pfsense, en donde ya podemos configurarlo a nuestra necesidad. Las credenciales por defecto son usuario “admin” y contraseña “pfsense”.

![]( /assets/images/herramientas/pfsense12.png)














