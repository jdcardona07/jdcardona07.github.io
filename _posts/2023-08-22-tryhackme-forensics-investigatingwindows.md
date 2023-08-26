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
