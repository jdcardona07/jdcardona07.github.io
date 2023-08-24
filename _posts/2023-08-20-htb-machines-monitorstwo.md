---
layout: single
title: MonitorsTwo - Machines - Hack The Box
excerpt: "MonitorsTwo es una máquina de dificultad fácil en la plataforma de HTB. Para acceder debemos explotar una vulnerabilidad en Cacti, accederemos a un contenedor en el que tendremos que elevar privilegios mediante un binario SUID, conseguiremos acceso a la máquina principal crackeando un hash obtenido mediante la enumeración de la base de datos MySQL. Para escalar privilegios en la máquina principal, encontraremos una vulnerabilidad en Docker en la cual podremos ejecutar comandos del contenedor en la máquina principal obteniendo así root gracias a la bash con permisos SUID.."
date: 2023-08-23
classes: wide
header:
  teaser: /assets/images/htb-machines-monitorstwo/monitorstwo.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - hackthebox
  - machines
 
tags:  
  - burpsuite
  - Command Injection
  - python
  - RCE
  
---

![](/assets/images/htb-machines-monitorstwo/monitorstwo.png)
