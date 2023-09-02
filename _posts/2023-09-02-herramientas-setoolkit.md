---
layout: single
title: Captura de credenciales mediante Hotspot y Setoolkit - Herramientas
excerpt: "Se configurara un portal cautivo (Hotspot) mediante el cual se busca realizar la autenticación de usuarios a una red abierta , en este se ofrecerá un servicio de acceso a internet (Wifi gratis). luego de ello mediante herramientas y técnicas de hacking, se buscará obtener información confidencial de los usuarios autenticados"
date: 2023-09-02
classes: wide
header:
  teaser: /assets/images/herramientas/Setoolkit/set.png
  teaser_home_page: true
  icon: /assets/images/mikrotik.webp
categories:
  - Herramientas
 
tags:  
  - setoolkit
  - phishing
  - socialengineer
  - hotspot
  - mikrotik
  
---

![](/assets/images/herramientas/Setoolkit/set.png)

## Portal Cautivo (Hotspot)

Un portal cautivo es un espacio en el que se ofrece acceso a internet de manera pública. Para ello se dispone de una red inalámbrica con uno o varios puntos de acceso y de un dispositivo enrutador que se encarga de realizar la conexión con el proveedor de internet.

## Settoolkit

Es una completísima suite dedicada a la ingeniería social, que nos permite automatizar tareas que van desde el de envío de SMS (mensajes de texto) falsos, con los que podemos suplantar el número telefónico que envía el mensaje, a clonar cualquier página web y poner en marcha un servidor para hacer phishing en cuestión de segundos.

El kit de herramientas SET está especialmente diseñado para realizar ataques avanzados contra el elemento humano. Originalmente, este instrumento fue diseñado para ser publicado con el lanzamiento de http://www.social-engineer.org y rápidamente se ha convertido en una herramienta estándar en el arsenal de los pentesters. SET fue escrito por David Kennedy (ReL1K) con un montón de ayuda de la comunidad en la incorporación de los ataques nunca antes vistos en un juego de herramientas de explotación.

## Laboratorio

Para este laboratorio vamos a manejar sistemas virtualizados (Mikrotik, Kali Linux, Windows 7) 

• Usaremos el Mikrotik como Gateway, Servidor DNS y Servidor del portal cautivo. 

• Kali Linux será el equipo atacante con las herramientas de phishing. 

• El Windows 7 será la víctima que se conecta a la red.

## Desmostración 
https://github.com/jdcardona07/jdcardona07.github.io/assets/98642593/5dea6532-e031-4812-9fe5-ec1c762be745

[![](https://markdown-videos.deta.dev/youtube/YHLuVMuRelE)](https://youtu.be/YHLuVMuRelE?si=Ia53rqXL9xFV98NE)

![](/assets/images/herramientas/Setoolkit/Setoolkit.mp4)

## Referencias

•	https://soporte.syscom.mx/es/articles/1476146-mikrotik-hotspot-server-con-portal-cautivo 

•	https://mikrotikthemes.airpoint.club/

•	https://blog.wifire.me/como-configurar-hotspot-mikrotik-usando-wifire/

•	https://www.trustedsec.com/tools/the-social-engineer-toolkit-set/
