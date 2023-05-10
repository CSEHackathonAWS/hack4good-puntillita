# Hackathon For Good: La Región de AWS en España al Servicio de la Sociedad

Proyecto : Puntillita
Reto : ONCE

## Descripción del Proyecto

Para la gestión más autónomas de la gente invidentes o parcialmente invidentes proponemos una aplicación para ayudar a los potenciales clientes de supermercados. Nos apoyamos a que todos los productos estan indicados con precios al principio de cada repisa con una etiqueta física, dicha etiqueta podría acompañarse de un tag-nfc. El contenido de dicho tag debería es el código de barras. 

Una vez identificado todos los productos, la información que puede aportarse es nombre, descripción, precio, localización de todos los productos, etc. 

El potencial, sabiendo que estoy leyendo: 
 - Identificar el producto elegido.
 - Puedo buscar otros productos e indicarte donde ir, desde donde estoy (ultima lectura).
 - Podemos crear una lista de productos elegidos.

## Diagrama de Arquitectura

![Arquitectura](/arq.jpg arquitectura)

## Descripción Técnica

La BBDD para este POC he utilizado una SQLite que me abstrae de tener un motor y siendo un fichero local, la latencia se minimiza. Esta alojada en un S3 y se descarga cada vez que arranca el aplicativo. Futurible es un cambio a cualquier otro sistema, el que mejor se adapte a la volumetría de productos y supermercados. El poblado de la BBDD ha sido realizada haciendo un scrapper parcial sobre la API de Mercadona.

Un microservicio REST esta alojado en una instancia EC2 ARM - graviton. Utiliza spring-boot-3 con java 17, en un intento de optimizar el producto se ha intentado generar el modo nativo (GraalVM). No ha sido posible por la cantidad de recursos consumida en la compilación (debe ser dentro de la misma arquitectura) y la falta de tiempo para validar problemas de compilación. 

Para acceder desde el cloud a esta arquitectura comendamos con un servicio no-ip, por simplificar. Más tarde, agregamos un api-gateway, una VPC, load-balancer y group-instances para poder utilizar una DNS que sea agnóstica a la ip externa que varía cada vez que se arranca la instancia.

Para el consumo de este servicio, una aplicación nativa java. Realiza una lectura de tags-NFC con el número del código de barras. Este lanzará una consulta a la API y se retorna un JSON con todos los metadatos relacionados con el producto seleccionado. Ahora esta implementado : nombre, precio, pasillo. La aplicación ahora "canta" el nombre y el precio, con el Text-To-Speech nativo de Android.

[Código API](https://github.com/CSEHackathonAWS/puntillita-api)
[Código APP](https://github.com/CSEHackathonAWS/puntillita-app)
[Código Utiles](https://github.com/CSEHackathonAWS/puntillita-tools)

## Demo Vídeo

[![Video Demo](https://img.youtube.com/vi/XXX/0.jpg)](https://www.youtube.com/watch?v=XX)


## Team Members
- Víctor J. Bellés [email](mailto:victor-juan.belles@soprasteria.com)

Agradecimientos a:
- Ricard Rubio [email](mailto:ricard.rubio@soprasteria.com) (Mentoring)
