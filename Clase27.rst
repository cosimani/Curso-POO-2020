.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 27 - POO 2018 (No preparado aún)
===================
(Fecha: 26 de junio)


Recuperatorio, entrega de prácticos y coloquio
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^



API de Google Street
^^^^^^^^^^^^^^^^^^^^

- Permite descargar una vista
- Puede utilizarse con una clave de API para la aplicación
	- Acceder a https://code.google.com/apis/console y loguearse
	- Administración de las API - Google Street View Image API
	- Habilitar el servicio
	- Credenciales - Crear credenciales - Clave de Servidor o Clave de navegador

- Parámetros de la URL:
	- https://developers.google.com/maps/documentation/streetview

- Parámetros obligatorios
	- size - Imagen en píxeles. Por ejemplo, ``size=600x400`` (máximo 640x640)
	- location - Texto (universidad blas pascal) o lat./long. (40.457375,-80.009353)
	- sensor - Si el dispositivo dispone de GPS "true" o "false"

- Ejemplo: http://maps.googleapis.com/maps/api/streetview?size=400x400&location=donato%20alvarez%20380&sensor=false

- Opcionales:
	- heading - Rotación entre 0 y 360 (heading=45)
	- fov (field of view) - zoom (aprox. entre 10 y 120 - valor predeterminado 90)
	- pitch - Ángulo de inclinación (predeterminado 0 - entre -90 y 90)
	- key: Clave de API (ver https://code.google.com/apis/console)

**Ejercicio 21**

- Con la misma idea que la clase Mapa, hacer ahora la clase ``StreetView``. 
- En un QLineEdit ingresar el domicilio a buscar.
- Con sólo movimientos del mouse horizontales, girar la rotación entre 0 y 360.

**Ejercicio 22**

- Agregar a ``StreetView`` lo siguiente:
- Agregar un QSlider para controlar el zoom.
- Además del QSlider, controla el zoom con dobleclic derecho para aumentarlo y con el izquierdo para disminuirlo.
- Actualizar también la posición del QSlider luego de los dobleclics.
- Almacenar todas las direcciones buscadas en la tabla ``logs`` de la base de datos		




**Google Maps**

- URL para su uso: https://developers.google.com/maps/documentation/staticmaps
- Ejemplo: http://maps.googleapis.com/maps/api/staticmap?center=rondeau+100+cordoba&zoom=15&size=500x300&maptype=roadmap&sensor=false
- Descripción de los parámetros en: https://developers.google.com/maps/documentation/staticmaps/#URL_Parameters
- Pueden habilitar otros servicios en https://code.google.com/apis/console


**Ejercicio 12** 

- Hacer una aplicación para buscar una dirección en Google Maps
- Definir la clase Mapa. Será el QWidget donde se dibujará el mapa de google.
- Definir la clase Ventana para contener al layout.
- Ese layout tendrá:
	- QLineEdit para ingresar un domicilio
	- QPushButton para buscar ese domicilio
	- Mapa
	- QSlider vertical para aumentar y disminuir el zoom





Un par de memes antes del examen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. figure:: images/clase10/meme2.jpg

.. figure:: images/clase10/meme4.jpg





Levantar base de datos a QTableView
===================================

- Colocar con el QtDesigner un QTableView

.. code-block:: c

	QSqlRelationalTableModel * tableModelAlumnos;
	tableModelAlumnos = new QSqlRelationalTableModel( this, adminDB->getDB() ); 

	tableModelAlumnos->setTable( "alumnos" );  // Tabla de la base

	// Para modificar como una planilla de excel
	tableModelAlumnos->setEditStrategy( QSqlTableModel::OnManualSubmit ); 

	// Otra relación. En lugar de mostrar el id_carrera que muestre el nombre de la carrera.
	tableModelAlumnos->setRelation( 5, QSqlRelation( "carreras", "id", "nombre" ) );

	tableModelAlumnos->select();  // Hace la consulta.

	// Títulos de las columnas en el widget.
	tableModelAlumnos->setHeaderData( 1, Qt::Horizontal, "Legajo" );
	tableModelAlumnos->setHeaderData( 2, Qt::Horizontal, "Nombre" );
	tableModelAlumnos->setHeaderData( 3, Qt::Horizontal, "Apellido" );
	tableModelAlumnos->setHeaderData( 4, Qt::Horizontal, "Mail" );
	tableModelAlumnos->setHeaderData( 5, Qt::Horizontal, "Carrera" ); 

	// Seteamos el QSqlTableModel sobre el QTableView
	ui->tableViewAlumnos->setModel( tableModelAlumnos );

	// Lista desplegable con el nombre de la carrera, esto cuando se modifique la celda.
	ui->tableViewAlumnos->setItemDelegate( new QSqlRelationalDelegate( ui->tableViewAlumnos ) );

	// Ocultamos la columna id de la tabla alumnos.
	ui->tableViewAlumnos->setColumnHidden( 0, true );

	// Ajusta el ancho de la celda con el texto en su interior. Para todas las columnas.
	ui->tableViewAlumnos->resizeColumnsToContents(); 
	
.. code-block:: c

	void Principal::slot_guardarCambios()  {    // Guada todos los cambios 
	    tableModelAlumnos->submitAll();
	}

	void Principal::slot_deshacer()  {  // Deshace todos los cambios que hizo el usuario.
	    tableModelAlumnos->revertAll();
	}

**Ejercicio 27**

- Hacerlo funcionar mostrando la tabla usuarios y su relación con tabla carreras
- Tabla alumnos: id, legajo, nombre, apellido, mail, id_carrera
- Tabla carreras: id, nombre
- Usar QtDesigner
		