---
layout: single
title: TrueSecrets - Challenges - Forensics - Hack The Box
excerpt: Nuestra unidad de ciberdelincuencia ha estado investigando a un conocido grupo APT durante varios meses. El grupo ha sido responsable de varios ataques de alto perfil contra organizaciones corporativas. Sin embargo, lo interesante de ese caso es que han desarrollado un servidor de comando y control personalizado propio. Afortunadamente, nuestra unidad pudo asaltar la casa del líder del grupo APT y tomar una captura de memoria de su computadora mientras aún estaba encendida. Analiza la captura para intentar encontrar el código fuente del servidor."
date: 2023-08-25
classes: wide
header:
  teaser: /assets/images/htb-challenges-forensics-truesecrets/truesecrets.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - hackthebox
  - challenges
 
tags:  
  - forensics
  - volatility
  - AES
  - veracrypt
    
---

![](/assets/images/htb-challenges-forensics-truesecrets/truesecrets.png)

## Descripción del Desafio:

Nuestra unidad de ciberdelincuencia ha estado investigando a un conocido grupo APT durante varios meses. El grupo ha sido responsable de varios ataques de alto perfil contra organizaciones corporativas. Sin embargo, lo interesante de ese caso es que han desarrollado un servidor de comando y control personalizado propio. Afortunadamente, nuestra unidad pudo asaltar la casa del líder del grupo APT y tomar una captura de memoria de su computadora mientras aún estaba encendida. Analiza la captura para intentar encontrar el código fuente del servidor.

Descargamos y descomprimimos el archivo. Encontramos un archivo con extensión “.raw” el cual es una imagen del volcado de memoria RAM.

![](/assets/images/htb-challenges-forensics-truesecrets/truesecrets2.png)

Descargamos la herramienta Volatility 2.6 para Windows en el siguiente enlace.

•	https://www.volatilityfoundation.org/releases

Leemos el archivo “.raw” el cual nos indica que perfil utilizar al ejecutar volatility.

•	volatility_2.6_win64_standalone>volatility_2.6_win64_standalone.exe -f TrueSecrets.raw imageinfo

![](/assets/images/htb-challenges-forensics-truesecrets/truesecrets3.png)

Identificamos el perfil “Win7SP1x86_23418” el cual será utilizado para localizar en el volcado los diferentes elementos que estructuran la memoria del sistema analizado.

Ahora vamos a intentar encontrar archivos importantes, debido a que se trata de un desarrollo de un servidor, podemos probar buscar la palabra “development” utilizando el perfil obtenido.

•	volatility_2.6_win64_standalone.exe -f TrueSecrets.raw --profile=Win7SP1x86_23418 dumfiles filescan | find "development".

![](/assets/images/htb-challenges-forensics-truesecrets/truesecrets4.png)

Encontramos 2 archivos interesantes, vamos a probar con el “backup_development.zip”. Procedemos a descargar el archivo con el siguiente comando, indicando la ubicación de memoria del archivo y la ubicación de descarga, junto al perfil obtenido.

•	volatility_2.6_win64_standalone.exe -f TrueSecrets.raw --profile=Win7SP1x86_23418 dumfiles dumpfiles --physoffset=0x000000000bbf6158 -u -n --dump-dir=C:\Users\are\Downloads

![](/assets/images/htb-challenges-forensics-truesecrets/truesecrets5.png)

Vamos a descomprimir los archivos descargados y vamos a encontrar el archivo “development.tc” el cual hace referencia a un archivo de la aplicación TrueCrypt que permite cifrar y ocultar datos.

![](/assets/images/htb-challenges-forensics-truesecrets/truesecrets6.png)

Para poder abrir y ver el contenido del archivo, vamos a descargar la aplicación VeraCrypt para Windows desde el siguiente link.

•	https://www.veracrypt.fr/en/Downloads.html

Luego de instalar nuestra aplicación e intentar abrir el archivo, debemos proporcionar una contraseña.

![](/assets/images/htb-challenges-forensics-truesecrets/truesecrets7.png)

Encontramos que Volatility cuenta con el complemento “truecryptpassphrase” el cual sirve para obtener las credenciales en caliente de la memoria RAM de  TrueCrypt. Vamos a intentar obtener la contraseña con el comando.

•	volatility_2.6_win64_standalone.exe -f TrueSecrets.raw --profile=Win7SP1x86_23418 truecryptpassphrase

![](/assets/images/htb-challenges-forensics-truesecrets/truesecrets8.png)

Ingresamos la contraseña obtenida en la aplicación para ver el contenido del archivo.

![](/assets/images/htb-challenges-forensics-truesecrets/truesecrets9.png)

Luego de poder leer el contenido del archivo, nos encontramos la carpeta “malware_agent” y 4 archivos adicionales.

1.	Un scrip (AgentServer.cs)

![](/assets/images/htb-challenges-forensics-truesecrets/truesecrets10.png)

3.	Tres archivos cifrados.

![](/assets/images/htb-challenges-forensics-truesecrets/truesecrets11.png)

Luego de revisar el script, identificamos que los archivos al parecer están cifrados por este script con el algoritmo de cifrado DES, Adicionalmente encontramos lo que puede ser la clave y el vector de inicialización.

![](/assets/images/htb-challenges-forensics-truesecrets/truesecrets12.png)

Procedemos a utilizar la herramienta online https://devtoolcafe.com/tools/des junto con a la contraseña para intentar descifrar los 3 archivos.

Luego de varios intentos logramos descifrar lo que sería nuestra flag con el tercer archivo.

![](/assets/images/htb-challenges-forensics-truesecrets/truesecrets13.png)



