---
layout: single
title: Instalación Splunk - SIEM - Herramientas
excerpt: "​Splunk es la solución SIEM (Security Information & Event Management) que permite monitorizar y analizar todo el big data de la empresa (en aplicaciones, sistemas e infraestructuras) a través de una interfaz web."
date: 2021-12-24
classes: wide
header:
  teaser: /assets/images/herramientas/splunk.png
  teaser_home_page: true
  icon: /assets/images/herramientas/logo_splunk.webp
categories:
  - SIEM
 
tags:  
  - siem
  
---

![]( /assets/images/herramientas/splunk8.png)

Splunk es la solución SIEM (Security Information & Event Management) que permite monitorizar y analizar todo el big data de la empresa (en aplicaciones, sistemas e infraestructuras) a través de una interfaz web.

## 1. Introducción
Esta guía realiza una breve descripción para la instalación de Splunk en un sistema Ubuntu 20.04 LTS.

## 2. Descarar instalador de Splunk
Para iniciar el proceso de instalacion de SPlunk, sea necesario el registro en el siguiente enlace https://www.splunk.com/en_us/download/splunk-enterprise.html, esto con el fin de solicitar el periodo de prueba de SPlunk Entreprise gratis por 60 días.

![]( /assets/images/herramientas/splunk2.png)

## 3. Instalar Splunk
Luego del registro y de la descarga del software, debemos descomprimirlo.

![]( /assets/images/herramientas/splunk3.png)

Luego de descomprimir el paquete, vamos a cambiar de propietario con el comando chown al usuario splunk. Finalmente ingresamos al directorio splunk/bin y ejecutamos el binario, aceptando los terminas de licencia e iniciándolo con el usuario splunk que anteriormente asignamos como propietario del directorio. Asignamos un usuario y contraseña de acceso.

![]( /assets/images/herramientas/splunk4.png)

Luego de unos minutos, nos informara que ya está ejecutándose la interfaz web de Splunk por el puerto 8000.

![]( /assets/images/herramientas/splunk5.png)

Desde un navegar, ingresamos la dirección ip local de la maquina seguido del puerto 8000. Ingresamos usuario y contraseña las cuales fueron creados en el proceso de instalación.

![]( /assets/images/herramientas/splunk6.png)

Ahora ya podemos empezar a configurar a nuestra necesidad Splunk.

![]( /assets/images/herramientas/splunk7.png)
