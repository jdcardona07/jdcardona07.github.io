---
layout: single
title: Instalación HoneyDrive - HoneyPot - Herramientas
excerpt: "​HoneyDrive es una distribución Linux orientada a la seguridad TI y, en particular, al despliegue y control de herramientas de tipo Honeypot. HoneyDrive ha sido creada por  Bruteforce Lab. La distribución se basa en una máquina virtual Xubuntu en formato OVA (Open Virtual Appliance) que puede descargarse desde la página oficial de SourceForge.."
date: 2022-06-04
classes: wide
header:
  teaser: /assets/images/herramientas/honeydrive.png
  teaser_home_page: true
  icon: /assets/images/honeypot.webp
categories:
  - HoneyPot
 
tags:  
  - honeypot
  - honeydrive
  
---

![]( /assets/images/herramientas/honeydrive.png)

# HoneyDrive
HoneyDrive es una distribución Linux orientada a la seguridad TI y, en particular, al despliegue y control de herramientas de tipo Honeypot. HoneyDrive ha sido creada por  Bruteforce Lab. La distribución se basa en una máquina virtual Xubuntu en formato OVA (Open Virtual Appliance) que puede descargarse desde la página oficial de SourceForge..

Dentro de las herramientas que podemos encontrar dentro de HoneyDrive estas.

•	Kippo: es un honeypot que emula un servicio ssh.

•	Tiny: Es un honeypot que hace uso de Iptables para hacer decisiones sobre como redireccionar el tráfico TCP/IP entrante.

•	Wordpot Wordpot: es un honeypot que emula la aplicación web Worpress.

•	Glastopf Glastopf: es un honeypot de aplicación web de baja interacción que puede emular cientos de vulnerabilidades, con el fin de obtener datos de ataques que tienen como objetivo aplicaciones web.

•	Dionea: es un honeypot el cual tiene como objetivo obtener una copia del malware que intenta propagarse por la red al brindar servicios que pretenden ser vulnerables. En este artículo se describirá el proceso de dicha captura.

Entre otros tipos de honeypot como Amun, Kojoney, LaBrea sticky y otras herramientas como nmap y para análisis de tráfico.

## 1. Introducción
En esta practica nos vamos a centrar en la instalación, configuración y funcionamiento de los honeypot Kippo y Dionea.

## 2. Topología
Para la siguiente implementación vamos a utilizar 3 máquinas virtualizadas en el entorno VMware.

![]( /assets/images/herramientas/honeydrive2.png)

Lista de sistemas operativos y roles con los que vamos a trabajar.

![]( /assets/images/herramientas/honeydrive3.png)

## 3. Instalación HoneyDrive

Vamos a descargar de la Url https://sourceforge.net/projects/honeydrive/ la imagen ova del HoneyDrive y luego vamos a cargarla desde nuestro entorno VMware, también se puede cargar desde VirtualBox.

Luego de la descarga, vamos a importar la imagen. No hay que definir los parámetros de la máquina virtual. Estos vienen predefinidos en la appliance. Tras el despliegue, procedemos a arrancarla.

![]( /assets/images/herramientas/honeydrive4.png)

Luego que inicie nuestra imagen vamos a ver el inicio de sesión de la siguiente manera, es un entorno muy similar a el escritorio de ubuntu.

![]( /assets/images/herramientas/honeydrive5.png)

En el fichero README.txt se muestran varios directorios y ficheros relacionados con las diferentes herramaientas que tiene HoneyDrive, proporciona los diferentes comandos necesarios para iniciar los servicios.

## 4. Instalación Kippo

Kippo es un honeypot SSH de interacción media diseñado para registrar ataques de fuerza bruta y, lo que es más importante, toda la interacción de shell realizada por el atacante. Kippo emula un servicio SSH con el objetivo de obtener las credenciales usadas por los atacantes contra este tipo de servicios.

Abrimos el fichero README.txt que contiene los comandos necesarios.

![]( /assets/images/herramientas/honeydrive6.png)

Nos ubicamos en el servicio Kippo y copiamos la ruta donde esta almacenado.

![]( /assets/images/herramientas/honeydrive7.png)

Existe el fichero /opt/kippo/kippo.cfg donde se pueden configurar varios parámetros como puerto, IP, hostname, banner a mostrar, etc.

Vamos a arrancar Kippo desde el directorio /honeydrive/kippo/start.sh.

![]( /assets/images/herramientas/honeydrive8.png)

Esperamos que inicie, luego desde nuestra maquina Kali Linux vamos a hacer un nmap para visualizar si el puerto de ssh (22) está abierto.

![]( /assets/images/herramientas/honeydrive9.png)

Kippo nos ofrece un servicio web el cual podremos visualizar las diferentes conexiones que se hacen por ssh, podremos ver las ip de origen, los usuarios usados y contraseñas.

![]( /assets/images/herramientas/honeydrive10.png)

## 5. Ataque a SSH

Vamos a realizar un ataque de fuerza bruta al servicio ssh, para eso vamos a utilizar un diccionario que contiene kali linux (rockou.txt).

![]( /assets/images/herramientas/honeydrive11.png)

Con la herramienta Medusa, ejecutamos un ataque al servicio ssh del honeypot, obtenemos la contraseña del usuario root.

![]( /assets/images/herramientas/honeydrive12.png)

Vamos a iniciar sesión al servicio ssh.

![]( /assets/images/herramientas/honeydrive13.png)

Ahora vamos como en la web de Kippo nos registró la i del atacante.

![]( /assets/images/herramientas/honeydrive14.png)

En la pestaña KIPPO-GEO podemos ver el numero de conexiones por ip.

![]( /assets/images/herramientas/honeydrive15.png)

También podemos ver las diferentes contraseñas que fueron usadas o el top de las más usadas.

![]( /assets/images/herramientas/honeydrive16.png)

Podremos visualizar gráficamente los usuarios mas usados para conexiones, así como combinaciones entre usuarios y contraseñas usadas.

![]( /assets/images/herramientas/honeydrive17.png)


## 6. Instalación Dionaea

Dionaea es un honeypot de baja interacción que captura cargas útiles de ataque y malware. Incrustando python como lenguaje de scripting, usando libemu para detectar códigos de shell, soportando ipv6 y tls.
Dionaea ofrece servicios vulnerables, accesibles en la red, con el fin de obtener una copia del malware que intente aprovecharse de los mismos. Es capaz de emular los siguientes protocolos:

•	SMB: Es el protocolo principal que implementa dionaea, debido a su larga trayectoria de vulnerabilidades, y la fácil explotación.

•	HTTP Y HTTPS: Establece tanto conexiones Http como Https y las cierra haciendo uso de los datos obtenidos en estos puertos. Para Https se genera un certificado al inicio de la aplicación 

•	TFTP Dionea ofrece una implementación que permite la descarga de ficheros, aunque no se tiene constancia de que este protocolo sea objetivo de ataques automatizados, si es conocida la existencia de vulnerabilidades para este protocolo. 

•	FTP: Dionaea ofrece un servidor básico de ftp en el puerto 21, que permite crear carpetas y bajar y subir archivos. Al igual que tftp, no se tiene constancia de que algo interesante ocurra con respecto a este servicio en lo que a ataques de malware automatizados se refiere.
Ahora vamos al archivo README.txt y vamos a la sección de Dionaea para ver las rutas de configuración y ejecución.

![]( /assets/images/herramientas/honeydrive18.png)

Ejecutamos /opt/dionaea/bin/dionaea para iniciar el servicio. Luego vamos a iniciar el servicio web para visualizar los eventos.

![]( /assets/images/herramientas/honeydrive19.png)

Ingresamos desde un explorador a la ip local 10.10.10.254:8080 para ver la interfaz gráfica, en donde vamos a visualizar todos los eventos relacionados con los servicios de Dianaea.

![]( /assets/images/herramientas/honeydrive20.png)

Adicionalmente vamos a tener que modificar el archivo de configuración de Dionaea para simular muchos mas servicios como el de ftp y smb.

![]( /assets/images/herramientas/honeydrive21.png)

En ese archivo debemos colocar la IP de nuestra máquina para que esos servicios puedan iniciar.

![]( /assets/images/herramientas/honeydrive22.png)

Finalmente ejecutamos un Nmap a nuestro honeypot y vamos a ver mas servicios abiertos en esa ip entre ellos el servicio SMB puerto 445 el cual vamos a atacar.

![]( /assets/images/herramientas/honeydrive23.png)

## 7. Ataque a SMB

Vamos a realizar un ataque al honeypot al servicio de smb 445 que simula un servicio de windows.

Para eso utilizaremos la herramienta metasploit de kali linux, usaremos el exploit/windows/smb/ms06_040_netapi y el payload windows/shell/bin_tcp.

![]( /assets/images/herramientas/honeydrive24.png)

Vamos a dirigirnos a la web de DIonaea y visualizaremos como registro el ataque.

![]( /assets/images/herramientas/honeydrive25.png)

Podemos observar los puertos que utilizo el atacante.

![]( /assets/images/herramientas/honeydrive26.png)

También vamos a poder ver las conexiones que se tuvo, dirección ip de origen puerto utilizado, fecha, protocolo entre otros datos relevantes.

![]( /assets/images/herramientas/honeydrive27.png)

