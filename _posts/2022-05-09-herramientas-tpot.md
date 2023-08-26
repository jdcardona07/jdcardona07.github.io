---
layout: single
title: Instalación T-Pot en Azure - Herramientas
excerpt: "​T-Pot es una suit multiuso que despliega una variedad de contenedores docker y docker-compose específicos de protocolo que emulan servicios explotables comunes."
date: 2022-05-09
classes: wide
header:
  teaser: /assets/images/herramientas/tpot/tpot.png
  teaser_home_page: true
  icon: /assets/images/herramientas/tpot.webp
categories:
  - HoneyPot
 
tags:  
  - honeypot
  - tpot
  - azure
  
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


## 1. Topología:
Para realizar el proceso de instalación vamos a trabajar con la siguiente topología de red.

![]( /assets/images/herramientas/securityonion/securityonion2.png)
