---
layout: single
title: Instalación Security Onion - Herramientas
excerpt: "​Security Onion es una distribución de Linux abierta y gratuita para la búsqueda de amenazas, la supervisión de la seguridad y la gestión de registros. Cuenta con múltiples herramientas de seguridad en redes en un sólo sistema operativo."
date: 2022-07-30
classes: wide
header:
  teaser: /assets/images/herramientas/securityonion/securityonion.png
  teaser_home_page: true
  icon: /assets/images/herramientas/securityonion.webp
categories:
  - IDS
 
tags:  
  - ids
  
---

![]( /assets/images/herramientas/securityonion/securityonion.png)

# SECURITY ONION
Security Onion es una distribución de Linux abierta y gratuita para la búsqueda de amenazas, la supervisión de la seguridad y la gestión de registros. Cuenta con múltiples herramientas de seguridad en redes en un sólo sistema operativo. 

Cuenta con herramientas de sistemas de detección y prevención de intrusiones (IDS/IPS). también, cuenta con herramientas basada en reglas, como es Suricata, esta herramienta funcionan en base a reglas, las cuales ayudan a la detección de intrusiones para su posterior bloqueo.

También cuenta con una herramienta que se basa en análisis como Zeek. Se encarga de monitorizar la actividad en la red recogiendo registros de todas las conexiones y solicitudes DNS, SSL, HTTP, FTP, SSH, y Syslog.

Dentro de sus herramientas incorporadas también podemos encontrar.

•	TheHive

•	Playbook

•	Fleet

•	Osquery

•	CyberChef

•	Elasticsearch

•	Logstash

•	Kibana

•	Suricata

•	Zeek

•	Wazuh

Estas herramientas en conjunto permiten a Security Onion realizar la captura completa de paquetes, detección de redes y puntos finales (basada en reglas), análisis y correlación de los conjuntos de datos adquiridos.

## 1. Topología:
Para realizar el proceso de instalación vamos a trabajar con la siguiente topología de red.

![]( /assets/images/herramientas/securityonion/securityonion2.png)

## 2. Instalación:
Luego debemos descargar la imagen ISO con la versión más reciente 2.3.80 que vamos a utilizar de la página oficial https://securityonionsolutions.com/software.

Después de descargar la imagen ISO, vamos a configurar nuestra máquina virtual con las características mínimas de hardware recomendadas por el proveedor.

![]( /assets/images/herramientas/securityonion/securityonion3.png)

Luego de configurar la máquina virtual procedemos a encenderla y cargar nuestra imagen ISO. Seleccionamos la instalación de modo gráfico.

![]( /assets/images/herramientas/securityonion/securityonion4.png)

Realizamos la creación de un usuario y contraseña para el ingreso al sistema.

![]( /assets/images/herramientas/securityonion/securityonion5.png)

Esperamos que termine el proceso.

![]( /assets/images/herramientas/securityonion/securityonion6.png)

Luego se reiniciará el sistema, cuando inicie seleccionamos el primer sistema operativo.

![]( /assets/images/herramientas/securityonion/securityonion7.png)

Ingresamos el usuario y la contraseña creada previamente.

![]( /assets/images/herramientas/securityonion/securityonion8.png)

Iniciará el menú de instalación, en donde damos sí.

![]( /assets/images/herramientas/securityonion/securityonion9.png)

Seleccionamos la opción de instalación del programa.

![]( /assets/images/herramientas/securityonion/securityonion10.png)

En este menú vamos a seleccionar el tipo de instalación, vamos a instalar en este caso EVALUATION, ya que nos traen algunas reglas y programas preconfigurados.

![]( /assets/images/herramientas/securityonion/securityonion11.png)

Confirmamos estar de acuerdo con los términos de licencia.

![]( /assets/images/herramientas/securityonion/securityonion12.png)

Seleccionamos una instalación estándar para la administración. 

![]( /assets/images/herramientas/securityonion/securityonion13.png)

Nos van a recomendar las características de memoria RAM y espacio en disco duro que vamos a necesitar.

![]( /assets/images/herramientas/securityonion/securityonion14.png)

Escribimos el nombre que le vamos a dar a la máquina.

![]( /assets/images/herramientas/securityonion/securityonion15.png)

Vamos a seleccionar la interface de administracion. 

![]( /assets/images/herramientas/securityonion/securityonion16.png)

Seleccionamos el modo de direccionamiento que tendrá la interfaz, en este caso vamos a entregar direccionamiento por DHCP.

![]( /assets/images/herramientas/securityonion/securityonion17.png)

Nos arroja una advertencia por usar direccionamiento DHCP ya que esta podría estar cambiando, pero como vamos a hacerlo solo a modo de laboratorio es suficiente.

![]( /assets/images/herramientas/securityonion/securityonion18.png)

Damos en OK para confirmar. Vamos a seleccionar que la conexión se realizara sin Proxy.

![]( /assets/images/herramientas/securityonion/securityonion19.png)

Esperamos que carguen las configuraciones.

![]( /assets/images/herramientas/securityonion/securityonion20.png)

Luego nos pedirá la interfaz por donde se va a monitorizar.

![]( /assets/images/herramientas/securityonion/securityonion21.png)

Vamos a seleccionar de manera manual la forma en que se descarguen actualizaciones del sistema.

![]( /assets/images/herramientas/securityonion/securityonion22.png)

Ingresamos los direccionamientos que van a ser monitorizados, en el caso que se tenga varias redes.

![]( /assets/images/herramientas/securityonion/securityonion23.png)

Seleccionamos un tipo de instalación básica.

![]( /assets/images/herramientas/securityonion/securityonion24.png)

En esta opción vamos a trabajar con SURICATA como IDS y ZEEK para análisis de metadatos.

![]( /assets/images/herramientas/securityonion/securityonion25.png)

Vamos a seleccionar la opción ETOPEN para que se descargue y se configuren reglas predeterminada.

![]( /assets/images/herramientas/securityonion/securityonion26.png)

Escogemos instalar algunas aplicaciones de ayuda que mas adelante utilizaremos.

![]( /assets/images/herramientas/securityonion/securityonion27.png)

Luego creamos una direccion de correo para el ingreso al panel de administracion de los diferentes servicios y creamos una contraseña.

![]( /assets/images/herramientas/securityonion/securityonion28.png)

Seleccionamos el modo para ingresar a los servicios web, en este caso seleccionamos por medio de la dirección IP.

![]( /assets/images/herramientas/securityonion/securityonion29.png)

Nos pide si quieres configurar un servicio de NTP, luego nos arroja por defecto la dirección de un NTP server.

![]( /assets/images/herramientas/securityonion/securityonion30.png)

Confirmamos las opciones de configuracion que seleccionamos.

![]( /assets/images/herramientas/securityonion/securityonion31.png)

Esperamos el proceso de instalación y al finalizar vamos a reiniciar el sistema.

![]( /assets/images/herramientas/securityonion/securityonion32.png)

Luego del reinicio vamos a ingresar con las credenciales creadas.

![]( /assets/images/herramientas/securityonion/securityonion33.png)

Luego vamos a realizar la instalación del modo grafico como usuario root, ejecutando el archivo SO-ANALYSTS-INSTALL ubicado en la la carpeta Security Onion que esta en el escritorio del usuario.

![]( /assets/images/herramientas/securityonion/securityonion34.png)

Luego de terminar la instalación, se reiniciará el sistema.

![]( /assets/images/herramientas/securityonion/securityonion35.png)

Ahora ya podemos ver un entorno gráfico, ingresamos con las credenciales creadas.

![]( /assets/images/herramientas/securityonion/securityonion36.png)

Ahora ya podemos ver el entorno grafico de nuestra aplicación.

![]( /assets/images/herramientas/securityonion/securityonion37.png)

Ahora vamos a un navegador instalado ya en la maquina e ingresamos nuestra dirección IP que obtuvo por DHCP, luego ingresamos con el correo y contraseña creadas en la instalación.

![]( /assets/images/herramientas/securityonion/securityonion38.png)

Nos dirigimos en el menú Alerts para observar el panel de alertas generados, podemos ver el tipo de alerta, el módulo que la identifico (Suricata, Ossec), el nivel de gravedad entre otros datos más.

![]( /assets/images/herramientas/securityonion/securityonion39.png)

Ahora vamos a explorar el menú Hunt que permite visualizar alertas NIDS de Suricata , alertas HIDS de Wazuh , registros de metadatos de protocolo de Zeek.

![]( /assets/images/herramientas/securityonion/securityonion40.png)

También podemos filtrar tráfico desde la barra de búsqueda con diferentes parámetros.

![]( /assets/images/herramientas/securityonion/securityonion41.png)

## Herramientas:

### Kibana:
Kibana, permite analizar rápidamente los tipos de datos que genera Security Onion, incluye alertas no solo de NISD / HIDS, sino también los registros de Zeek y registros del sistema recopilado a través de syslog.

Ingresamos con la cuenta de correo y contraseña creadas en la instalación.

![]( /assets/images/herramientas/securityonion/securityonion42.png)

Luego veremos la página inicial en donde se logra ver diferente información relacionada con el tráfico de red.

![]( /assets/images/herramientas/securityonion/securityonion43.png)

Podemos filtrar para observar por tipos de protocolos, IP, puertos o tipos de alerta.

![]( /assets/images/herramientas/securityonion/securityonion44.png)

### Grafana:
En la aplicación Grafana vamos a poder observar un estado de salud del sistema, porcentajes de CPU, memoria, tráfico de red, entre otros indicadores.

![]( /assets/images/herramientas/securityonion/securityonion45.png)

### Playbook:
Playbook es una aplicación web que le permite crear una guía de detección, que a su vez consta de jugadas individuales. Estos juegos son completamente autónomos y describen los diferentes aspectos en torno a la estrategia de detección particular.

![]( /assets/images/herramientas/securityonion/securityonion46.png)

### CyberChef:
CyberChef permite decodificar, descomprimir y analizar paquetes ip. Las alertas , la búsqueda y el PCAP le permiten enviar datos de forma rápida y sencilla a CyberChef para su posterior análisis.

![]( /assets/images/herramientas/securityonion/securityonion47.png)

### TheHive:
TheHive es una interfaz de gestión de casos, mientras se esta trabajando con algunas alertas en los módulos Hunt o Kibana, se podría presentar algunas alertas o registros que se consideren enviar a TheHive y posteriormente crear casos.

![]( /assets/images/herramientas/securityonion/securityonion48.png)

Luego estos casos serán gestionados por personal creado anteriormente.

![]( /assets/images/herramientas/securityonion/securityonion49.png)

### Fleet:
Fleet, se usa para administrar la implementación de Osquery el cual utiliza comandos SQL básicos para aprovechar un modelo de datos relacional para describir un dispositivo, ingresamos con las credenciales creadas en la instalación. 

![]( /assets/images/herramientas/securityonion/securityonion50.png)

Luego de ingresar podemos ver la interfaz de inicio en donde podremos agregar equipos y realizar consultas por comandos SQL sobre la infraestructura de nuestros Sistemas operativos, también permite por ejemplo hacer consultas sobre los procesos en ejecución, usuarios, cambios en contraseñas, dispositivos USB, puerto abierto, entre otras consultas.

![]( /assets/images/herramientas/securityonion/securityonion51.png)


