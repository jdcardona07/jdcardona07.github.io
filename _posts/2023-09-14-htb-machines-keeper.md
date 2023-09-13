---
layout: single
title: MKeeper - Machines - Hack The Box
excerpt: "Keeper es una máquina basada en Linux de nivel facil que se centra en explotar vulnerabilidades en el gestor de contraseñas KeePass."
date: 2023-09-14
classes: wide
header:
  teaser: /assets/images/htb-machines-keeper/keeper.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - hackthebox
  - machines
 
tags:  
  - cve-2023-32784 
  - keepass
    
---

![](/assets/images/htb-machines-keeper/keeper.png)

Keeper es una máquina basada en Linux de nivel facil que se centra en explotar vulnerabilidades en el gestor de contraseñas KeePass.

## 1. Enumeración 

Realizamos un escaneo de puertos con Nmap y evidenciamos dos puertos abiertos.

•	SSH (22)

•	HTTP (80)

![](/assets/images/htb-machines-keeper/keeper2.png)

Realizamos un curl sobre la IP y el puerto 80 nos redirecciona al dominio.

•	tickets.keeper.htb/rt

![](/assets/images/htb-machines-keeper/keeper3.png)

Vamos a agregar este dominio a nuestro archivo "etc/hosts".

![](/assets/images/htb-machines-keeper/keeper4.png)

## 2.	Acceso Inicial

Vamos a ingresar al dominio identificado mediante navegador web.

![](/assets/images/htb-machines-keeper/keeper5.png)

Identificamos un portal de inicio de sesión de RT el cual es una plataforma de flujo de trabajo y seguimiento de problemas de código abierto. Vamos a buscar si cuenta con credenciales predeterminadas para el acceso.

![](/assets/images/htb-machines-keeper/keeper6.png)

Luego de identificar las credenciales de acceso, observamos que podeos tener acceso a la plataforma.

![](/assets/images/htb-machines-keeper/keeper7.png)

Luego de buscar sobre todo los módulos del sitio, identificamos que en el menú “Admin/User” observamos la creación de un usuario junto a la contraseña inicial.

•	Usuario lnorgaard

•	Contraseña: Welcome2023!

![](/assets/images/htb-machines-keeper/keeper8.png)

Probamos esas credenciales de acceso vía SSH sobre la maquina y obtenemos acceso.

![](/assets/images/htb-machines-keeper/keeper9.png)

Ahora simplemente obtenemos nuestra primera flag.

![](/assets/images/htb-machines-keeper/keeper10.png)

## 3. Escalada de privilegios

Vamos a intentar escalar privileios en la maquina, para eso listamos los archvios y observamos un archivo compmrido que llama la atencion. 

![](/assets/images/htb-machines-keeper/keeper11.png)

Vamos a descargarlo a nuestra maquina atacante.

![](/assets/images/htb-machines-keeper/keeper12.png)

Al descomprimirlo, obtenemos dos archivos, uno es un volcado de memoria de la aplicación y el otro es la base de datos de la aplicación Keepass el cual es un gestor de contraseñas.

![](/assets/images/htb-machines-keeper/keeper13.png)

Realizando la búsqueda en internet, encontramos un exploit que explota la vulnerabilidad CVE-2023-32784 de Keepass.

•	https://github.com/CMEPW/keepass-dump-masterkey

Este exploit nos permite recuperar la contraseña maestra en texto sin formato de un volcado de memoria.

![](/assets/images/htb-machines-keeper/keeper14.png)

Ahora vamos a utilizar un sistema Windows para instalar Keepass e intentar iniciar sesión con la contraseña obtenida, pero no fue posible.

![](/assets/images/htb-machines-keeper/keeper14.1.png)

Realizando la búsqueda en internet sobre la contraseña obtenida sin los caracteres especiales en el volcado, nos encontramos con un indicio que podríamos utilizar.

![](/assets/images/htb-machines-keeper/keeper15.png)

Efectivamente es la contraseña de acceso a la base de datos de Keepass.

![](/assets/images/htb-machines-keeper/keeper16.png)

Dentro de la base de datos encontramos credenciales del usuario root pero al utilizarlas para iniciar sesión por SSH no funcionan.

Adicionalmente encontramos un archivo Putty PPK el cual vamos a copiar y guardar.

![](/assets/images/htb-machines-keeper/keeper17.png)

Ahora vamos a iniciar sesión por SSH pero desde la aplicación Putty cargando el archivo PPK.

![](/assets/images/htb-machines-keeper/keeper18.png)

Efectivamente nos funciona para tener acceso a la maquina como rrot. Ahora solo es tomar nuestra segunda flag.

![](/assets/images/htb-machines-keeper/keeper19.png)

