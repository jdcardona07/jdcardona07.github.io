---
layout: single
title: Instalación T-Pot en Azure - Herramientas
excerpt: "​T-Pot es una suit multiuso que despliega una variedad de contenedores docker y docker-compose específicos de protocolo que emulan servicios explotables comunes."
date: 2022-05-09
classes: wide
header:
  teaser: /assets/images/herramientas/tpot/tpot.png
  teaser_home_page: true
  icon: /assets/images/tpot.webp
categories:
  - HoneyPot
 
tags:  
  - honeypot
  - tpot
  - azure
  - honeytrap
  - rdpy
  - suricata
  - dionaea
  - cowrie
  - 
  
---

![]( /assets/images/herramientas/tpot/tpot.png)

# T-POT
T-Pot es una suit multiuso que despliega una variedad de contenedores docker y docker-compose específicos de protocolo que emulan servicios explotables comunes.

Se ejecuta en Debian e incluye versiones dockerizadas de los siguientes honeypots adbhoney, ciscoasa, conpot, cowrie, dionaea, elasticpot, glastopf, glutton, heralding, honeypy, honeytrap, mailoney, medpot, rdpy, snare, tanner.

Además, T-Pot incluye las siguientes herramientas

•	Cockpit: monitoreo de rendimiento en tiempo real y terminal web.

•	Cyberchef: una aplicación web para el cifrado, codificación, compresión y análisis de datos.

•	Pila ELK: para visualizar todos los eventos capturados por T-Pot.

•	Elasticsearch Head: es un front-end web para navegar e interactuar con un clúster de Elastic Search.

•	Fatt: es un script basado en pyshark para extraer metadatos de red y huellas dactilares de archivos pcap y tráfico de red en vivo.

•	Spiderfoot: herramienta de automatización de inteligencia de código abierto.

•	Suricata: es un motor de monitoreo de seguridad de red.


## 1. Instalación:
Desde el portal de Azure vamos a realizar la creación de nuestro recurso para el montaje del laboratorio.

Puede obtener una cuenta gratuita visitando https://azure.microsoft.com/en-us/free/ y registrándose.

### Algunas características minimas del equipo:
•	Sistema operativo: Debian 10.11
•	Memoria Ram: 16 GB
•	Procesador: 4vcp

![]( /assets/images/herramientas/tpot/tpot2.png)

Este tipo de honeypot debe ser compatible con el sistema operativo Debian. Vamos a utilizar la versión 10 “Buster”.

![]( /assets/images/herramientas/tpot/tpot3.png)

Información de la maquina creada como direccionamiento o características, también debemos crear algunas reglas en el firewall de Azure para permitir el tráfico hacia el honeypot y restringirlo en los puertos de administración.

![]( /assets/images/herramientas/tpot/tpot4.png)

Luego de que nuestro recurso se cree, vamos a conectarnos por SSH al servidor, luego actualizamos el sistema.

![]( /assets/images/herramientas/tpot/tpot5.png)

Realizamos la instalación de git.

![]( /assets/images/herramientas/tpot/tpot6.png)

Clonamos el repositorio de tpot.

•	sudo git clon https://github.com/dtag-dev-sec/tpotce

![]( /assets/images/herramientas/tpot/tpot7.png)

Luego de la descarga, ejecutamos el comando de instalación.

•	sudo tpotce/iso/installer/install.sh --type=user

![]( /assets/images/herramientas/tpot/tpot8.png)

Seleccionamos el tipo de instalación. Para nuestro caso vamos a usar el modo standard.

![]( /assets/images/herramientas/tpot/tpot9.png)

Creamos un usuario administrador y asignamos una contraseña.

![]( /assets/images/herramientas/tpot/tpot10.png)

Esperamos que el proceso de instalación terminé.

![]( /assets/images/herramientas/tpot/tpot11.png)

Se recomienda aplicar las siguientes reglas en el Firewall de Azure luego de la instalaciòn, con el fin de restringir el acceso a puertos de administraciòn y permitir el trafico hacia los servicios de HoneyPot.

1.	REGLA-SSH - Permitir puerto 64295, Protocolo: TCP, Fuente: "Su IP pública", Destino: Cualquiera
2.	REGLA-ADMIN-WEB - Permitir puerto 64297, Protocolo: TCP, Origen: "IP pública", Destino: any.
3.	REGLA-ADMIN - Permitir puerto 64294, Protocolo: TCP, Origen: "IP pública", Destino: any.
4.	TPOT-ALL - Permitir puertos 0-64293,64298-65535, Protocolo: TCP, Origen: any, Destino: any.

Luego de terminar el sistema se reiniciará. Vamos a poder ingresar de nuevo desde SSH pero desde el puerto 64295.

![]( /assets/images/herramientas/tpot/tpot12.png)

Desde la web, vamos a poder ingresar con la IP publica de la maquina y en el puerto 64297. Esta interfaz nos presenta diferentes herramientas útiles, entre ellas Kibana en donde visualizaremos los ataques en tiempo real que está recibiendo.

![]( /assets/images/herramientas/tpot/tpot13.png)

Ingresamos a Kibana y damos en el dasboard de t-pot.

![]( /assets/images/herramientas/tpot/tpot14.png)

Al pasar algunos minutos vemos como ya van 7 ataques al honeypot Dionaea.

![]( /assets/images/herramientas/tpot/tpot15.png)

Alguna información relevante de los ataques que nos presenta el servicio web son los tipos de protocolos atacados, países de origen puertos, entre otros datos.

![]( /assets/images/herramientas/tpot/tpot16.png)

Luego de un par de horas ya teníamos 3.548 ataques recibidos desde los diferentes honeypot.

![]( /assets/images/herramientas/tpot/tpot17.png)

Mas debajo de la interfaz nos lista las direcciones IPs de los atacantes, así como algunas alertas generadas desde el IDS Suricata y vulnerabilidades explotadas.

![]( /assets/images/herramientas/tpot/tpot18.png)

En tiempo real podemos visualizar los diferentes tipos de ataques y sus orígenes.

![]( /assets/images/herramientas/tpot/tpot19.png)

Los diferentes binarios capturados en los honeypot estarán disponibles en la ruta /data. Por ejemplo para el caso del honeypot dionaea almacena los binarios en la ruta /data/dionaea/binaries.

![]( /assets/images/herramientas/tpot/tpot21.png)

Desde la misma IP y en el puerto 64294 podremos visualizar el rendimiento del servidor, los contenedores desplegados, el estado general de la máquina.

![]( /assets/images/herramientas/tpot/tpot20.png)

