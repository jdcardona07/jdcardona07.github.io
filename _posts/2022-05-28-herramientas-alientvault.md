---
layout: single
title: Instalación Alient Vault OSSIM - Herramientas
excerpt: "AlienVault OSSIM es una gestión de eventos e información de seguridad (SIEM) de código abierto y rica en funciones que incluye recopilación, normalización y correlación de eventos."
date: 2022-05-28
classes: wide
header:
  teaser: /assets/images/herramientas/alientvault.png
  teaser_home_page: true
  icon: /assets/images/alientvault.webp
categories:
  - SIEM
 
tags:  
  - siem

---

![]( /assets/images/herramientas/alientvault/alientvault.png)

# AlientVault OSSIM
AlienVault OSSIM es una gestión de eventos e información de seguridad (SIEM) de código abierto y rica en funciones que incluye recopilación, normalización y correlación de eventos.

AlienVault OSSIM se compone de los siguientes módulos:

•	Descubrimiento de activos

•	Evaluación de vulnerabilidad

•	Detección de intrusiones

•	Monitoreo del comportamiento

•	Correlación de eventos SIEM


Entre otros tipos de honeypot como Amun, Kojoney, LaBrea sticky y otras herramientas como nmap y para análisis de tráfico.

## 1. Instalación AlíentVault OSSIM
Vamos a descargar la imagen ISO de la página oficial y la cargamos a nuestro entorno virtual. Luego de iniciarla vamos a seleccionar la instalación de OSSIM.

•	https://cybersecurity.att.com/products/ossim/download

![]( /assets/images/herramientas/alientvault/alientvault2.png)

Vamos ahora a seleccionar el lenguaje del sistema.

![]( /assets/images/herramientas/alientvault/alientvault3.png)

Seleccionamos la ubicación geográfica.

![]( /assets/images/herramientas/alientvault/alientvault4.png)

Seleccionamos el idioma del teclado.

![]( /assets/images/herramientas/alientvault/alientvault5.png)

Esperamos que carguen los componentes y asignamos el direccionamiento IP local.

![]( /assets/images/herramientas/alientvault/alientvault6.png)

 
Configuramos la máscara de red.

![]( /assets/images/herramientas/alientvault/alientvault7.png)

Configuramos la dirección IP del Gateway.

![]( /assets/images/herramientas/alientvault/alientvault8.png)

Ahora agregamos nuestro servidor de DNS, en nuestro caso es el Gateway.

![]( /assets/images/herramientas/alientvault/alientvault9.png)

Asignamos una clave para el superusuario.

![]( /assets/images/herramientas/alientvault/alientvault10.png)

Esperamos que finalice el proceso de instalación y al finalizar la instalación, ingresamos por el navegador de un pc en la misma subred y con la IP asignada a AlientVault a la interfaz web para terminar de configurarlo.

![]( /assets/images/herramientas/alientvault/alientvault11.png)

Luego de terminar la configuración, ya podemos ingresar a la interfaz web.

![]( /assets/images/herramientas/alientvault/alientvault12.png)

Ahora ingresaremos con usuario root desde la terminal.

![]( /assets/images/herramientas/alientvault/alientvault13.png)

Seleccionamos la opción 3.

![]( /assets/images/herramientas/alientvault/alientvault14.png)

Iniciamos el servicio ossec desde la ruta /etc/init.d.

![]( /assets/images/herramientas/alientvault/alientvault15.png)

Luego desde la ruta /var/ossec/bin ejecutamos el comando manage_agents y a continuación vamos a agregar nuestro primer agente ossec Ubuntu.

![]( /assets/images/herramientas/alientvault/alientvault16.png)

Posteriormente agregamos el agente ossec Windows.

![]( /assets/images/herramientas/alientvault/alientvault17.png)

## 2. Instalación Agente OSSEC en Ubuntu
Luego de descargar y descomprimir el archivo de ossec, vamos a ejecutar el instalador.

•	 https://www.ossec.net/ossec-downloads/

![]( /assets/images/herramientas/alientvault/alientvault18.png)

Seleccionamos la opción de agente y agregamos la IP del servidor Ossec.

![]( /assets/images/herramientas/alientvault/alientvault19.png)

Luego exportamos la clave del servidor Ossec y dentro del ubuntu ejecutamos el manage_agente e importamos la clave para terminar la configuracion. 

![]( /assets/images/herramientas/alientvault/alientvault20.png)

Vamos al servicio web de AlientVault y vamos a poder observar que ya fue agregado nuestro agente ossec.

![]( /assets/images/herramientas/alientvault/alientvault21.png)

## 3. Instalación Agente OSSEC Windows

Ejecutamos el agente para Windows .exe descargado.

•	 https://www.ossec.net/ossec-downloads/

![]( /assets/images/herramientas/alientvault/alientvault22.png)

Aceptamos los términos de licencia.

![]( /assets/images/herramientas/alientvault/alientvault23.png)

Seleccionamos los componentes a instalar.

![]( /assets/images/herramientas/alientvault/alientvault24.png)

Seleccionamos la ubicación en donde se instalará.

![]( /assets/images/herramientas/alientvault/alientvault25.png)

Finalizamos la instalacion.

![]( /assets/images/herramientas/alientvault/alientvault26.png)

Luego de finalizar la instalación, vamos a ingresar la IP del servidor ossec y la clave que exportamos.

![]( /assets/images/herramientas/alientvault/alientvault27.png)

Aceptamos la configuración Y Esperamos hasta que el servicio empiece a correr.

![]( /assets/images/herramientas/alientvault/alientvault28.png)

Al revisar en el servidor ossec podemos ver como ya está conectado como agente.

![]( /assets/images/herramientas/alientvault/alientvault29.png)
