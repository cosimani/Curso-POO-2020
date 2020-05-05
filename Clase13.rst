.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 13 - POO 2020
===================
(Fecha: 29 de abril)


Clase QNetworkAccessManager
^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Permite enviar y recibir solicitudes a la red
- Se obtiene un objeto ``QNetworkReply`` con toda la información recibida

.. code-block:: c

	QNetworkAccessManager* manager = new QNetworkAccessManager;

	connect(manager, SIGNAL(finished(QNetworkReply*)), this, SLOT(slot_respuesta(QNetworkReply*)));

	manager->get(QNetworkRequest(QUrl("http://mi.ubp.edu.ar")));

- Para poder utilizar las clases de network hay que agregar en el .pro

.. code-block:: c

	QT += network  // Esto agrega al proyecto el módulo network

- Por defecto, el módulo 'gui' y el módulo 'core' están incluidos.
- Para utilizar HTTPS, Qt utiliza OpenSSL https://www.openssl.org/source
	- Se puede descargar desde https://slproweb.com/products/Win32OpenSSL.html
	- Por ejemplo, para 64 bits elegir `Win64 OpenSSL v1.1.1g <https://slproweb.com/download/Win64OpenSSL-1_1_1g.exe>`_

Clase QIODevice
^^^^^^^^^^^^^^^

.. figure:: images/clase08/qiodevice.png 

- Clase base de los dispositivos de I/O
- Algunos métodos:

.. code-block:: c

	QByteArray readAll()  		 // Lee todos los datos disponibles.
	QByteArray read(qint64 max)  // Lee hasta max datos disponibles.
	QByteArray readLine()  		 // Lee una linea.


