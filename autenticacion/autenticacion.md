---
layout: default
title: Autenticación
nav_order: 2
has_children: true
permalink: /autenticacion
---

Todas las llamadas al API son hechas vía SSL en el protocolo HTTPS. 

El método de autenticación utilizado es OAuth 2.0, ya que provee un nivel de seguridad superior, que protege las credenciales y la firma en la comunicación. 

El método OAuth requiere que antes de cualquier operación al API, se solicite un token de acceso, a través de un ClientId y Secret. El token que se obtiene es utilizado como un Bearer que autentica todas las llamadas subsecuentes.  Para más información de OAuth visite https://es.wikipedia.org/wiki/OAuth