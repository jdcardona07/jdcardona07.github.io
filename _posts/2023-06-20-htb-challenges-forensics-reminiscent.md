---
layout: single
title: Reminiscent - Challenges - Forensics - Hack The Box
excerpt: Se detectó tráfico sospechoso desde la PC virtual de un reclutador. Se capturó un volcado de memoria de la máquina virtual infractora antes de que se eliminara de la red para la creación de imágenes y el análisis. Nuestro reclutador mencionó que recibió un correo electrónico de alguien con respecto a su currículum. Se recuperó una copia del correo electrónico y se proporciona como referencia. Encuentre y decodifique la fuente del malware para encontrar la bandera."
date: 2023-06-20
classes: wide
header:
  teaser: /assets/images/htb-challenges-forensics-reminiscent/reminiscent.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - hackthebox
  - challenges
 
tags:  
  - forensics
  - volatility
  - base64
    
---

![](/assets/images/htb-challenges-forensics-reminiscent/reminiscent.png)

## Descripción del Desafio:

Se detectó tráfico sospechoso desde la PC virtual de un reclutador. Se capturó un volcado de memoria de la máquina virtual infractora antes de que se eliminara de la red para la creación de imágenes y el análisis. Nuestro reclutador mencionó que recibió un correo electrónico de alguien con respecto a su currículum. Se recuperó una copia del correo electrónico y se proporciona como referencia. Encuentre y decodifique la fuente del malware para encontrar la bandera.

Descargamos y descomprimimos el archivo.

![](/assets/images/htb-challenges-forensics-reminiscent/reminiscent2.png)

Leemos el archivo “imageinfo.txt” el cual nos indica que perfil utilizar al ejecutar volatilIty.

![](/assets/images/htb-challenges-forensics-reminiscent/reminiscent3.png)

Vamos a iniciar con el plugin “windows.pslist” para obtener la lista de procesos ejecutados en la maquina al momento del volcado de memoria.

•	Python3 vol.py -f “RUT DEL ARCHIVO” windows.pslist”

![](/assets/images/htb-challenges-forensics-reminiscent/reminiscent4.png)

Observamos “poweshell.exe” ejecutándose con PID 2752.

![](/assets/images/htb-challenges-forensics-reminiscent/reminiscent5.png)

Vamos a utilizar el plugin “Windows.netscan” para obtener las conexiones realizadas en el momento del volcado de memoria del equipo.

•	Python3 vol.py -f “RUT DEL ARCHIVO” windows.netscan

Llama la atención conexiones por el puerto 80 usando powershell.exe.

![](/assets/images/htb-challenges-forensics-reminiscent/reminiscent6.png)

Ahora utilizamos el plugin “Windows.cmdline” para obtener los comandos que se usaron para ejecutar el proceso en el momento del volcado de memoria del equipo.

•	Python3 vol.py -f “RUT DEL ARCHIVO” windows.cmdline –pid 2752

Observamos que se ejecutó powershell con -enc el cual usa la codificación base64.

![](/assets/images/htb-challenges-forensics-reminiscent/reminiscent7.png)

Procedemos a decodificar el archivo.

•	echo -n “CODIGO EN BASE64” I base64 -d  

Observando la salida, podemos identificar nuestra flag.

![](/assets/images/htb-challenges-forensics-reminiscent/reminiscent8.png)
