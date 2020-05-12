.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 17 - POO 2019
===================
(Fecha: 24 de mayo)




:Tarea para Clase 17:
	Se tomará un mini examen en computadora para resolver en 30 minutos

	Traer un programa con un login que valide usuario con la base de datos

	Será necesario: JADE para creación de archivos .sqlite, creación de tablas, carga de datos, etc.

	También: MD5, material de las clases 13 y 14, Ejercicios 10 y 11, QSqlQuery, QSqlRecord, QSqlError, Registrar eventos (logs), INSERT INTO.







Funciones virtuales
^^^^^^^^^^^^^^^^^^^

- Puede ser interesante llamar a la función de la derivada (en polimorfismo).
- Al declarar una función como virtual en la clase base, si se superpone en la derivada, al invocar usando el puntero a la clase base, se ejecuta la versión de la derivada.

.. code-block:: c

	class Persona  {
	public:
	    Persona( QString nombre ) : nombre( nombre )  {  }
	    virtual QString verNombre()  {  return "Persona: " + nombre;  }  // Y si no fuera virtual?

	protected:  
	    QString nombre;
	};

	class Empleado : public Persona  {
	public:
	    Empleado( QString nombre ) : Persona( nombre )  {  }
	    QString verNombre()  {  return "Empleado: " + nombre;  }
	};


	#include <QApplication>
	#include "personal.h"
	#include <QDebug>

	int main( int argc, char** argv )  {
	    QApplication a( argc, argv) ;

	    {
	    Persona *carlos = new Empleado( "Carlos" );

	    qDebug() << carlos->verNombre();  // Qué publica?

	    delete carlos;
	    }

	    return a.exec();
	}




Función virtual pura y clase abstracta
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- No necesita ser definida, sólo se declara.
- Será definida en las clases derivadas

.. code-block:: c

	virtual void verValor( int a ) = 0;

- Algunos pueden decir que no es muy elegante igualar a cero una función:

.. code-block:: c

	#define abstracta =0

	// entonces podemos usar:
	virtual void verValor( int a ) abstracta;

- Una clase con al menos una función virtual pura la convierte en clase abstracta.
- Una clase abstracta no puede ser instanciada.
- Si en la clase derivada no se define la función virtual pura, significa que esta clase derivada también es abstracta.

.. code-block:: c

	#define abstracta =0

	class Persona  {
	public:
	    Persona( QString nombre ) : nombre( nombre )  {  }
	    virtual QString verNombre() abstracta;

	protected:  
	    QString nombre;
	};

	class Empleado : public Persona  {
	public:
	    Empleado( QString nombre ) : Persona( nombre )  {  }
	    QString verNombre()  {  return "Empleado: " + nombre;  }
	};

	int main( int argc, char** argv )  {
	    QApplication a( argc, argv );

	    {
	    Persona * carlos = new Empleado( "Carlos" );

	    qDebug() << carlos->verNombre();

	    delete carlos;
	    }

	    return a.exec();
	}



**Ejercicio 12**

- Diseñar una aplicación para una galería de fotos
- Debe tener una base con una tabla 'imagenes' que tenga las URLs de imágenes
- Un botón >> y otro << para avanzar o retroceder en la galería de fotos
- Se podrá navegar sobre las fotos que se descargarán desde internet
	
	
**Para independizar del SO**

.. code-block:: c

	AdminDB adminDB;
	QString nombreSqlite;

	#ifdef __APPLE__
	    nombreSqlite = "/home/cosimani/db/test";
	#elif __WIN32__
	    nombreSqlite = "C:/Qt/db/test";
	#elif __linux__
	    nombreSqlite = "/home/cosimani/db/test";
	#else
	    nombreSqlite = "/home/cosimani/db/test";
	#endif

	if ( adminDB.conectar( nombreSqlite ) )
	    qDebug() << "Conexion exitosa";


**Algunos argentinos que también explican como los mexicanos** 

- Clic sobre los GIF para abrir los videos 

**Crear base de datos**

|ImageLink|_ 

.. |ImageLink| image:: /images/clase12/crearBase.gif
.. _ImageLink: https://www.youtube.com/watch?v=U9iE6pM0bxM

**Crear tabla**

|ImageLink2|_ 

.. |ImageLink2| image:: /images/clase12/crearTabla.gif
.. _ImageLink2: https://www.youtube.com/watch?v=_-hKca2k784

**Insertar registro**

|ImageLink3|_ 

.. |ImageLink3| image:: /images/clase12/insertarRegistro.gif
.. _ImageLink3: https://www.youtube.com/watch?v=RggFhFZnCPU

**Consultar datos**

|ImageLink4|_ 

.. |ImageLink4| image:: /images/clase12/consultarDatos.gif
.. _ImageLink4: https://www.youtube.com/watch?v=8emd37mvN2E


Registrar eventos (logs)
^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: c

	bool AdminDB::registrar( QString evento )  {
	    QSqlQuery query( db );

	    bool exito = query.exec( "INSERT INTO registos (evento) VALUES ('" + evento + "')" );

	    qDebug() << query.lastQuery();
	    qDebug() << query.lastError();  // Devuelve un objeto de QSqlError

	    return exito;
	}


**Armando la clase AdminDB**

.. code-block:: c

	#ifndef ADMINDB_H
	#define ADMINDB_H

	#include <QObject>
	#include <QSqlDatabase>

	class AdminDB : public QObject
	{
	    Q_OBJECT
	public:
	    explicit AdminDB( QObject *parent = 0 );
	    ~AdminDB();

	    bool conectar( QString archivoSqlite );
	    QSqlDatabase getDB();
	    bool isConnected();
	    void mostrarTabla( QString tabla );

	private:
	    QSqlDatabase db;
	};

	#endif // ADMINDB_H

.. code-block:: c

	#include "admindb.h"
	#include <QDebug>
	#include <QSqlQuery>
	#include <QSqlRecord>

	AdminDB::AdminDB( QObject * parent ) : QObject( parent )  {
	    qDebug() << "Drivers disponibles:" << QSqlDatabase::drivers();

	    db = QSqlDatabase::addDatabase( "QSQLITE" );
	}

	AdminDB::~AdminDB()  {
	    if ( db.isOpen() )
	        db.close();
	}

	bool AdminDB::conectar( QString archivoSqlite )  {
	    db.setDatabaseName( archivoSqlite );

	    return db.open();
	}

	QSqlDatabase AdminDB::getDB()  {
	    return db;
	}

	bool AdminDB::isConnected()  {
	    return db.isOpen();
	}

	void AdminDB::mostrarTabla( QString tabla )  {
	    if ( this->isConnected() )  {
	        QSqlQuery query = db.exec( "SELECT * FROM " + tabla );

	        if ( query.size() == 0 || query.size() == -1 )
	            qDebug() << "La consulta no trajo registros";

	        while( query.next() )  {
	            QSqlRecord registro = query.record();  // Devuelve un objeto que maneja un registro (linea, row)
	            int campos = registro.count();  // Devuleve la cantidad de campos de este registro

	            QString informacion;  // En este QString se va armando la cadena para mostrar cada registro
	            for ( int i = 0 ; i < campos ; i++ )  {
	                informacion += registro.fieldName( i ) + ":";  // Devuelve el nombre del campo
	                informacion += registro.value( i ).toString() + " - ";
	            }
	            qDebug() << informacion;
	        }
	    }
	    else
	        qDebug() << "No se encuentra conectado a la base";
	}






Uso de Qt Designer
..................

- Nuevo proyecto -> Qt GUI Application
- Utilizar el puntero ``ui`` para acceder a los objetos del diseño
- Tener en cuenta que los métodos virtuales de QWidget para eventos se pueden usar:

.. code-block:: c	

	virtual void mousePressEvent( QMouseEvent * event );
	virtual void resizeEvent( QResizeEvent * event );
	virtual void moveEvent( QMoveEvent * event );
	...

**Ejemplo**

.. code-block:: c	
	
	// ventana.h
	#ifndef VENTANA_H
	#define VENTANA_H

	#include <QWidget>

	namespace Ui {
	    class Ventana;
	}

	class Ventana : public QWidget  {
	    Q_OBJECT

	public:
	    explicit Ventana( QWidget * parent = 0 );
	    ~Ventana();

	private:
	    Ui::Ventana *ui;
	};

	#endif // VENTANA_H

.. code-block:: c

	// ventana.cpp
	#include "ventana.h"
	#include "ui_ventana.h"

	Ventana::Ventana( QWidget * parent ) : QWidget( parent ), ui( new Ui::Ventana )  {
	    ui->setupUi( this );
	}

	Ventana::~Ventana()  {
	    delete ui;
	}


Clase QTimer
^^^^^^^^^^^^

- Permite programar tareas de una sola ejecución o tareas repetitivas. 
- Conectamos la señal ``timeout()`` con algún slot.
- Con ``start()`` comenzamos y la señal ``timeout()`` se emitirá al terminar.


**Ejemplo (repetitivo):** Temporizador que cada 1000 mseg llamará a ``slot_update()``


.. code-block:: c

	QTimer * timer = new QTimer( this );
	connect( timer, SIGNAL( timeout() ), this, SLOT( slot_update() ) );
	timer->start( 1000 );
 

**Para una sola ejecución**

- Para temporizador de una sola ejecución usar ``setSingleShot(true)``
- El método estático ``QTimer::singleShot()`` nos permite la ejecución.


**Ejemplo:** Luego de 200 mseg se llamará a ``slot_update()``:


.. code-block:: c

	QTimer::singleShot( 200, this, SLOT( slot_update() ) );
	// donde this es el objeto que tiene definido el slot_update().
	


Métodos virtuales de QWidget para capturar eventos
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Algunos de ellos:


.. code-block:: c

	virtual void mouseDoubleClickEvent( QMouseEvent * event );
	virtual void mouseMoveEvent( QMouseEvent * event );
	virtual void mousePressEvent( QMouseEvent * event );
	virtual void mouseReleaseEvent( QMouseEvent * event );
	virtual void keyPressEvent( QKeyEvent * event );
	virtual void keyReleaseEvent( QKeyEvent * event );
	virtual void resizeEvent( QResizeEvent * event );
	virtual void moveEvent( QMoveEvent * event );
	virtual void closeEvent( QCloseEvent * event );
	virtual void hideEvent( QHideEvent * event )
	virtual void showEvent( QShowEvent * event )
	virtual void paintEvent( QPaintEvent * event )


- Estos métodos pueden ser reimplementados en una clase derivada para recibir los eventos.

**Ejercicio 13**

- Usar QtDesigner
- Definir la clase Ventana que herede de QWidget
- Buscar una imagen de un fútbol con formato PNG (para usar transparencias).
- Ventana tendrá un formulario que pide al usuario:
	- Diámetro del fútbol (píxeles):
	- Velocidad (mseg para ir de lado a lado):
	- QPushButton para actualizar el estado.
- El fútbol irá golpeando de izquierda a derecha en Ventana.


Clase QFileDialog
^^^^^^^^^^^^^^^^^

- Permite abrir un cuadro de diálogo para buscar un archivo en disco

.. code-block:: c	

	QString file = QFileDialog::getOpenFileName( this, "Abrir", "./", "Imagen (*.png *.jpg)" );

**Ejercicio 14**

- Elegir un archivo de imagen del disco con ``QFileDialog`` y dibujarlo en un ``QWidget``.
- Agregar un botón "Iniciar rotación" que genere la rotación de la imagen sobre su centro.


**Ejercicio 15** Al ingresar la URL de una imagen deberá mostrarla como en la figura

.. figure:: images/clase10/imagenes.png  
 
- Al hacer clic sobre una de estas imágenes, deberá ocultarse la misma. 
- Cuando se oculta la segunda imagen, cerrar la aplicación.












Señales propias
^^^^^^^^^^^^^^^

- Si necesitamos enviar una señal se utiliza la palabra reservada ``emit``.

.. code-block:: c	

	int i = 5;
	emit signal_enviarEntero( i );


- La función ``enviarEntero( int a )`` debe estar declarada con el modificador de acceso ``signals``

.. code-block:: c	

	signals:
	    void signal_enviarEntero( int );


- No olvidarse de la macro ``Q_OBJECT`` para permitir a esta clase usar signals y slots.
- Las signals deben ser compatibles en sus parámetros con los slots a los cuales se conecten.
- Solamente se declara esta función (Qt se encarga de definirla).


**Ejercicio 16** 

- Crear un login con un QLabel que funcione como un QPushButton
- Para esto incorporar al QLabel la señal ``void signal_clic()``


**Ejercicio 17** 

- Incorporar a un Login una señal que se emita cada vez que un usuario se valide exitosamente
- Que la señal se llame ``void signal_usuarioLogueado( QString )``
- El QString que envía es el nombre de usuario


**Ejercicio 18**

- Diseñar una aplicación con un login inicial que valide contra la base
- Almacenar sólo el hash en MD5 de las contraseñas
- Si el usuario es válido mostrar cualquier otra ventana cualquiera
- Registrar en la tabla 'logs' los intentos fallidos de logueo. No registrar las claves.
- Utilizar la señal creada en el Login del ejercicio anterior



