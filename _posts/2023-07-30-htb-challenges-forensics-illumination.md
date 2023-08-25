---
layout: single
title: Illumination - Challenges - Forensics - Hack The Box
excerpt: "Un desarrollador junior acaba de cambiarse a una nueva plataforma de control de código fuente. ¿Puedes encontrar la ficha secreta?"
date: 2023-07-30
classes: wide
header:
  teaser: /assets/images/htb-challenges-forensics-illumination/illumination.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - hackthebox
  - challenges
 
tags:  
  - forensics
    
---

![](/assets/images/htb-challenges-forensics-illumination/illumination.png)

## Descripción del Desafio:

Un desarrollador junior acaba de cambiarse a una nueva plataforma de control de código fuente. ¿Puedes encontrar la ficha secreta?

Descargamos el archivo y lo descomprimimos, luego vamos a listar su contenido.

![](/assets/images/htb-challenges-forensics-illumination/illumination2.png)

Accedemos al directorio oculto “.git”. 

![](/assets/images/htb-challenges-forensics-illumination/illumination3.png)

Vamos a observar los logs. En uno de los comentarios encontramos el siguiente enunciado.

•	“Gracias a los colaboradores, eliminé el token único porque era un riesgo de seguridad. ¡Gracias por informar con responsabilidad!”.

![](/assets/images/htb-challenges-forensics-illumination/illumination4.png)

Procedemos a leer que hizo en ese registro y observamos el token.

![](/assets/images/htb-challenges-forensics-illumination/illumination5.png)

Ahora guardamos el token y lo vamos a decodificar para obtener la flag.

![](/assets/images/htb-challenges-forensics-illumination/illumination6.png)
