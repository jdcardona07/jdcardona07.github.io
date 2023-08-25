---
layout: single
title: OSINT - Challenges - Vulnmachines
excerpt: Vamos a resolver los desafíos de la categoría OSINT, que correpsponden a la busqueda información en internet para encontrar las flag."
date: 2023-08-24
classes: wide
header:
  teaser: /assets/images/vm-challenges-osintt/vulnmachines.png
  teaser_home_page: true
  icon: /assets/images/vulnmachines.webp
categories:
  - Vulnmachines
  - challenges
 
tags:  
  - osint
  - aws
  - jenkins
    
    
---

![](/assets/images/vm-challenges-osintt/vulnmachines.png)

# OSINT Lab1

## Descripcion del desafio 1:

“El primer desafío de la serie OSINT es que debe realizar una evaluación de inteligencia de código abierto en los activos externos de la organización objetivo, OSINTOrg . Conozca los perfiles de la organización en las cuentas de redes sociales y sus empleados.”

Para este primer desafio, debemos buscar en redes sociales una cuenta asociada a la empresa objetivo. Vamos a utilizar la sguiente herramienta en linea.

•	https://www.namecheckr.com/

![](/assets/images/vm-challenges-osintt/vulnmachines2.png)

Encontramos que la empresa tiene una cuenta en Twitter.

![](/assets/images/vm-challenges-osintt/vulnmachines3.png)

Identificamos que 3 de sus seguidores hacen referencia a la empresa, probablemente sean empeados.

![](/assets/images/vm-challenges-osintt/vulnmachines4.png)

Realizamos la revisión de cada uno de ellos, don encontramos nuestra primera flag en un comentario realizado por uno de ellos.

![](/assets/images/vm-challenges-osintt/vulnmachines5.png)


# OSINT Lab2

## Descripcion del desafio 2:

“Del desafío anterior, tienes información básica sobre la organización y los empleados. Ya sabes que la empresa ha comenzado su viaje de desarrollo. Realice una enumeración pasiva en la plataforma de gestión de código fuente identificada que utiliza la empresa.”

Ahora para este desafio vamos a realizar una busqueda de infromacion en repositorios como GitHub, GitLab. Encontramos un repositorio de la empresa en GitHub, adicionalmente encontramos 2 usuarios vinculados que correposnden a las interaciones que vimos en Twitter.

![](/assets/images/vm-challenges-osintt/vulnmachines6.png)

Realziamos una búsqueda de cada uno de ellos en difernetes plataforma como  (GitHub, gist y GitLab) y encontramos el siguiente usuario de gist. 

•	https://gist.github.com/emilyosintorg/

![](/assets/images/vm-challenges-osintt/vulnmachines7.png)

Revisando los códigos publicados, encontramos nuestra segunda bandera en los comentarios, adicionalmente obtenemos información relacionada con una instancia de Jenkins que podría ser útil para nuestro siguiente reto.

![](/assets/images/vm-challenges-osintt/vulnmachines8.png)


# OSINT Lab3

## Descripcion del desafio 3:

“Genial, ha recopilado toda la información de los activos externos, analice la información recopilada de la plataforma de administración de código fuente. Ahora, encuentre un indicador del servicio alojado y realice una enumeración adicional.”

En el comentario obtenido anteriormente vemos lo siguiente.

• “La URL de Jenkins ahora está configurada en: osintorg-{emilyosintorg}-prod.domain-name”

Luego de reconstruir la URL Jenkins, tenemos lo siguiente.

•	http://osintorg-emilyosintorg-prod.osintorg.com/

Pero al intentar acceder por web no obtenemos nada.

![](/assets/images/vm-challenges-osintt/vulnmachines9.png)

Vamos a intentar descubrir si la URL es accesible mediante un puerto especifico. Para ello realizamos un NMAP sobre la URL y tenemos el puerto 8080.

![](/assets/images/vm-challenges-osintt/vulnmachines10.png)

Ahora si podemos ingresar a la aplicación, la cual no cuenta con un control de acceso.

![](/assets/images/vm-challenges-osintt/vulnmachines11.png)

Realizando una búsqueda sobre el sitio web, vemos que podemos acceder a las credenciales almacenadas y obtenemos nuestra flag.

![](/assets/images/vm-challenges-osintt/vulnmachines12.png)

# OSINT Lab4

## Descripcion del desafio 1:

“Encontró un servicio alojado abierto utilizado por la organización. En un análisis y una enumeración adicionales, obtendrá información relacionada con los activos de la nube. Ahora debe realizar la enumeración de los activos en la nube de OSINTOrg.”

Revisando los registros de implementacion en la instancia de Jenkins, enumeramos la salida porconsola del proyecto “prod_maven” y encontramos un nombre de un dispositivo en AWS.

![](/assets/images/vm-challenges-osintt/vulnmachines13.png)

Ahora vamos a utilizar AWS CLI para intentar listar el contenido del equipo, vemos un archivo llamado FLAG.txt.

La opcion “--no-sign-request” se utiliza para omitir el proceso de firma en los casos donde el acceso publico esta hablitado.

![](/assets/images/vm-challenges-osintt/vulnmachines14.png)

Ahora vamos a copiar el archvi “FLAG.txt” a nuestra maquina e intentamos leerla.

![](/assets/images/vm-challenges-osintt/vulnmachines15.png)

