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

## 1. ¿Cuál es la versión y el año de la máquina de Windows?.

Vamos abrir una consola de PowerShell y utilizamos el comando "Get-ComputerInfo".

![](/assets/images/trehackme-forensics-investigatingwindows/investigating3.png)

Luego Get-ComputerInfo -Property "Os" .

![](/assets/images/trehackme-forensics-investigatingwindows/investigating4.png)

## Respuesta:  
• Microsoft Windows Server 2016 Datacenter

## 2. ¿Qué usuario se conectó por última vez?.
Primero debemos conocer la lista de usuarios locales. Para eso vamos a utilizar el comando "Get-LocalUser".

![](/assets/images/trehackme-forensics-investigatingwindows/investigating5.png)

Ahora hacemos uso del comando "net user" seguido del nombre del usuario y la palabra "Last" para que nos muestre la última conexión de ese usuario. Claramentee la última conexión la realizó el usuario con el que nos conectamos a la máquina. 

![](/assets/images/trehackme-forensics-investigatingwindows/investigating6.png)

## Respuesta:  
• Administrator

## 3. ¿Cuándo fue la última vez que John inició sesión en el sistema?.
Para esta pregunta, vamos a utilizar la imagen anterior donde se muestra la última conexión del usuario John.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating7.png)

## Respuesta: 
• 3/2/2019 5:48:32

## 4. ¿A qué IP se conecta el sistema cuando se inicia por primera vez?.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating8.png)

## Respuesta: 
• 10.34.2.3

## 5. ¿Qué dos cuentas tenían privilegios administrativos (aparte del usuario administrador)?.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating9.png)

## Respuesta: 
• Jenny, Guest

## 6. ¿Cuál es el nombre de la tarea programada que es maliciosa?.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating10.png)

## Respuesta: 
• Clean file system

## 7. ¿Qué archivo intentaba ejecutar la tarea a diario?.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating11.png)

## Respuesta: 
• nc.ps1

## 8. ¿Qué puerto escuchó localmente este archivo?.

## Respuesta: 
• 1348

## 9. ¿Cuándo fue la última vez que Jenny inició sesión?.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating12.png)

## Respuesta: 
• Never

## 10. ¿En qué fecha se produjo el compromiso?.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating13.png)

## Respuesta: 
• 02/03/2019

## 11. Durante el compromiso, ¿a qué hora asignó Windows por primera vez privilegios especiales a un nuevo inicio de sesión?.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating14.png)

## Respuesta: 
• 03/02/2019 4:04:46 PM

## 12. ¿Qué herramienta se utilizó para obtener las contraseñas de Windows?.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating15.png)
![](/assets/images/trehackme-forensics-investigatingwindows/investigating16.png)

## Respuesta: 
• mimikatz

## 13. ¿Cuál era la IP de los servidores de comando y control externo de los atacantes?.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating17.png)

## Respuesta: 
• 76.32.97.132

## 14. ¿Cuál era el nombre de la extensión del shell cargado a través del sitio web del servidor?.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating18.png)

## Respuesta: 
• .jsp

## 15. ¿Cuál fue el último puerto que abrió el atacante?.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating19.png)

## Respuesta: 
• 1337

## 16. Verifique el envenenamiento de DNS, ¿qué sitio fue el objetivo?.

![](/assets/images/trehackme-forensics-investigatingwindows/investigating20.png)

## Respuesta: 
• google.com
