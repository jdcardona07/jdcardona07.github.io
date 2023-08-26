---
layout: single
title: Investigating Windows - Forensics - TryHackMe
excerpt: "“Una máquina con Windows ha sido pirateada, es su trabajo investigar esta máquina con Windows y encontrar pistas sobre lo que podría haber hecho el hacker."
date: 2023-08-22
classes: wide
header:
  teaser: /assets/images/trehackme-forensics-investigatingwindows/investigating.png
  teaser_home_page: true
  icon: /assets/images/tryhackme.webp
categories:
  - tryhackme
  - challenges
 
tags:  
  - forensics
  - windows
  - powershell
    
---

Vamos a resolver el reto "Investigating Windows" de la categoria Forensics de la plataforma TryHackMe.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating2.png)

## Descripción del Desafio:

“Una máquina con Windows ha sido pirateada, es su trabajo investigar esta máquina con Windows y encontrar pistas sobre lo que podría haber hecho el hacker"

## 1. ¿Cuál es la versión y el año de la máquina de Windows?

Vamos abrir una consola de PowerShell y utilizamos el comando "Get-ComputerInfo".

![](/assets/images/trehackme-forensics-investigatingwindows/investigating3.png)

Luego Get-ComputerInfo -Property "Os" .

![](/assets/images/trehackme-forensics-investigatingwindows/investigating4.png)

## Respuesta:  
• Microsoft Windows Server 2016 Datacenter

## 2. ¿Qué usuario se conectó por última vez?
Primero debemos conocer la lista de usuarios locales. Para eso vamos a utilizar el comando "Get-LocalUser".
![](/assets/images/trehackme-forensics-investigatingwindows/investigating5.png)

Ahora hacemos uso del comando "net user" seguido del nombre del usuario y la palbra "Last" para que nos muestre la última conexión de ese usuario. Obviamente la última conexión la realizo el usuario con el que nos conectamos a la máquina. 
![](/assets/images/trehackme-forensics-investigatingwindows/investigating6.png)

## Respuesta:  
• Administrator

## 3. ¿Cuándo fue la última vez que John inició sesión en el sistema?
Para esta pregunta, vamos a utilizar la imagen anterior donde se muestra la última conexión del usuario John.
![](/assets/images/trehackme-forensics-investigatingwindows/investigating7.png)

## Respuesta: 
• 3/2/2019 5:48:32
