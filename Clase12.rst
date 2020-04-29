.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 12 - POO 2020
===================
(Fecha: 28 de abril)


Web Service
^^^^^^^^^^^

- Para intercambiar datos entre aplicaciones
- Generalmente a través del protocolo HTTP
- La info puede viajar en XML, JSON, etc.
- Fomenta y facilita el uso y desarrollo de APIs Web
- https://es.wikipedia.org/wiki/Servicio_web

**Algunas APIs disponibles**

- Twitter - https://dev.twitter.com
- Facebook - https://developers.facebook.com
- Amazon - https://developer.amazonservices.es
- Spotify - https://developer.spotify.com/web-api
- MercadoLibre - http://developers.mercadolibre.com
- Google - https://developers.google.com
	- Youtube
	- Traductor
	- Google+
	- Maps
	- Street View

**QUrl**

- Para manipular una url ingresada por el usuario 

.. code-block:: c
	
	// URL ejemplo: http://www.yahoo.com.ar/documento/info.html
		
	// El método path() devuelve /documento/info.html
	// El método host() devuelve www.yahoo.com.ar
	
	QUrl url("http://www.yahoo.com.ar/documento/info.html");
	qDebug() << url.host();
	qDebug() << url.path();

