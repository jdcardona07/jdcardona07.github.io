---
layout: single
title: Campaña de phishing con Gophish y Zphisher  - Herramientas
excerpt: "Campañas de phishing vía correo electrónico simulando promoción de plataformas de streaming y posterior captura de credenciales de usuario"
date: 2023-09-02
classes: wide
header:
  teaser: /assets/images/herramientas/gophish/gophish.png
  teaser_home_page: true
  icon: /assets/images/zphisher.webp
categories:
  - Herramientas
 
tags:  
  - gophish
  - zphisher
  - phishing
  - socialengineer    
  
---

![](/assets/images/herramientas/gophish/gophish.png)

## Gophish

Es una herramienta de código abierto que permite a los usuarios realizar campañas de ingeniería social con el fin de recopilar credenciales de autenticación y otros datos sensibles (phishing). Esta herramienta está diseñada para ser utilizada por los investigadores de seguridad para mejorar la seguridad de la red. Gophish fue creado en 2016 por Jordan Wright, un investigador de seguridad con sede en San Francisco, EE. UU.

Los destinatarios reciben los simulacros de phishing y deben tomar medidas para evitar caer en la trampa. Por ejemplo, algunos destinatarios pueden verificar la dirección de correo electrónico para asegurarse de que proviene de una fuente confiable. Si un destinatario cae en la trampa, Gophish registra esta información.

## Zphisher

Es una herramienta automatizada que puede usarse para crear páginas de phishing y enviar a la víctima para robar la información confidencial.

Tiene 37 plantillas de página de phishing; incluyendo Facebook, Twitter y Paypal. También tiene 4 herramientas de reenvío de puertos.

## Laboratorio

En este caso se estaría suplantando una publicidad de “Netflix” ofreciéndola a un descuento del 80%, como se muestra en la imagen y así obtener información del correo del usuario y clave que pueda estar utilizando actualmente en alguna de las plataformas que utiliza, en este caso si tiene la cuenta de Netflix obtenemos su clave y con esta podemos intentar enviar correos si es la misma en su cuenta de correo.

![](/assets/images/herramientas/gophish/gophish2.png)

También se podría realizar ingeniería social si esta persona trabajar en una empresa que se quiere vulnerar y en la que conocemos el formato de correo podemos intentar usar esta cuenta y clave para obtener acceso. Luego de colocar los datos de Usuario y Clave, se redirige al sitio original de Netflix para no levantar sospechas y se utilizará la página de olvido de contraseña de Netflix. A continuación, las pantallas de la campaña.

A continuación, la pantalla que le aparece a la persona al dar clic en el enlace se puede observar que nos redirige al sitio “directory-positions-blogging-lace.trycloudflare.com”

![](/assets/images/herramientas/gophish/gophish3.png)

Al colocar los datos que nos piden en la pantalla y luego de darle al botón “Sign In” y que hemos capturado los datos, se enviara a la pantalla de Clave Olvidada de Netflix.

![](/assets/images/herramientas/gophish/gophish4.png)

## Descargo de responsabilidad

Este tutorial se ofrece solamente con propósito educativos y de información. Asimismo, en ningún caso este blog se hace responsable por daños o perjuicios ocasionados por las herramientas o sitios web recomendados en los tutoriales.


## Desmostración 
[![Alt text](https://img.youtube.com/vi/2o0k5g3E5_0/0.jpg)](https://www.youtube.com/watch?v=2o0k5g3E5_0)

## Referencias

• https://getgophish.com/

• https://github.com/gophish/gophish/releases

• https://github.com/htr-tech/zphisher

