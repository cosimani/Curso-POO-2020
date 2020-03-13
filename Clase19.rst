.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 19 - POO 2019
===================
(Fecha: 31 de mayo)

:Tarea para Clase 20:
	Se tomará un mini examen en computadora para resolver en 30 minutos

	QtDesigner, señales propias, QFileDialog, QTimer, clase propia en QtDesigner, QFile, const

	Funciones virtuales de QWidget para: mouseDoubleClickEvent, mouseMoveEvent, mousePressEvent, mouseReleaseEvent, keyPressEvent, keyReleaseEvent, resizeEvent, moveEvent, closeEvent, hideEvent, showEvent y paintEvent

	Es decir, clases 16, 17 y 18.


Enumeraciones
^^^^^^^^^^^^^

- Es un tipo especial de variable
- Sus valores son constantes enteras
- Estos valores pueden ser autogenerados (0, 1, 2, 3, ...)

.. code-block:: c	

	enum los_dias { DOM, LUN, MAR, MIE, JUE, VIE, SAB } dia;

	enum los_dias { DOM = 7, LUN = 1, MAR, MIE, JUE = 0, VIE, SAB };

- Las variables de este tipo pueden adoptar sólo valores DOM, LUN, ...
- Es decir, la variable "dia" puede tomar DOM o LUN o MAR ...
- Las enumeraciones declaradas dentro de una clase tiene la visibilidad de la clase

.. code-block:: c	

	class Dia  {
	public:
	    enum los_dias { LUN, MAR, MIE, JUE, VIE };
	    int un_dia;
	};

	int main( int argc, char ** argv )  {
	    Dia d1;
	    d1.un_dia = Dia::LUN;
	}


**Ejemplo**

.. code-block:: c	

	// figura.h
	class Figura : public QWidget  {
	    Q_OBJECT

	public:
	    enum Forma { CIRCULO, CUADRADO };

	    Figura( QWidget * parent = 0 );

	    void dibujar( Forma forma );

	protected:
	    void paintEvent( QPaintEvent * );

	private:
	    Forma forma;
	};


	// figura.cpp
	Figura::Figura( QWidget * parent ) : QWidget( parent ), forma( CIRCULO )  {  }

	void Figura::dibujar( Forma forma )  {
	    this->forma = forma;
	    this->repaint();
	}

	void Figura::paintEvent( QPaintEvent * )  {
	    QPainter pincel( this );
	    
	    switch( forma )  {
	    case CIRCULO:
	        // dibujar circulo
	        break;

	    case CUADRADO:
	        // dibujar cuadrado
	        break;

	    default:;
	    }
	}

	// main.cpp
	int main( int argc, char ** argv )  {
	    QApplication a( argc, argv );

	    Figura figura;
	    figura.dibujar( Figura::CUADRADO );
	    figura.show();

	    return a.exec();
	}


Herencia múltiple
^^^^^^^^^^^^^^^^^

- La clase derivada hereda todos los datos y funciones de todas las clases base
- Puede suceder que en la clases base existan funciones con igual nombre
- Los casos de ambigüedad se solucionan con el nombre completo
- Otra solución sería redefinir en la derivada la función ambigua.

.. code-block:: c	

	#include <QApplication>
	#include <QDebug>

	class ClaseA  {
	public:
	    ClaseA( int a ) : valorA( a )  {  }
	    int verValor()  {  return valorA;  }

	protected:
	    int valorA;
	};

.. code-block:: c	

	class ClaseB  {
	public:
	    ClaseB() : valorB( 20 )  {  }
	    int verValor()  {  return valorB;  }

	protected:
	    int valorB;
	};

.. code-block:: c	

	class ClaseC : public ClaseA, public ClaseB  {
	public:
	    ClaseC( int c ) : ClaseA( c ), ClaseB()  {  }
	    int verValor()  {  return ClaseA::verValor();  }
	};

.. code-block:: c	

	int main( int argc, char ** argv )  {
	    QApplication a( argc, argv );

	    ClaseC c( 10 );
	    qDebug() << c.verValor();  
	    qDebug() << c.ClaseB::verValor();  

	    return 0;
	}


**Ejemplo**

.. code-block:: c	

	class Persona  {
	private:
	    int edad;
	    int x, y;

	public:
	    Persona( int edad = 0 ) : edad( edad ), x( 0 ), y( 0 )  {  }

	    void move( int x, int y )  {
	        this->x = x;
	        this->y = y;
	    }
	};

	class Jugador : public Persona, public QWidget  {
	private:
	    int id;

	public:
	    Jugador() : Persona( 18 ), QWidget(), id( 0 )  {  }

	    void mudarse( int x, int y )  {
	        this->id++;
	        this->Persona::move( x, y );  // Se requiere especificar de esta manera
	    }
	};
	    



**Ejercicio 24**
 
- Crear un proyecto Qt Widget Application con el QWidget principal en la clase Ventana
- Crear una clase Boton que hereda de QWidget
- Redefinir paintEvent en Boton y usar fillRect para dibujarlo de algún color
- Definir el siguiente método en Boton:

.. code-block:: c

	Boton * boton = new Boton;
	boton->colorear( Boton::Azul );

	// Este método recibe como parámetro una enumeración que puede ser:
	// Boton::Azul  Boton::Verde  Boton::Magenta

- Usar QtDesigner para Ventana y Boton. Es decir, Designer Form Class
- Definir la enumeración en Boton
- Abrir el designer de Ventana y agregar 5 botones (objetos de la clase Boton). Promocionarlo
- Que esta Ventana con botones quede lo más parecido a la siguiente imagen:

.. figure:: images/clase19/botones.png

- Usar para Ventana grid layout, usar espaciadores y usar todos los recursos posibles del QtDesigner
- Dibujar un fondo agradable con paintEvent y drawImage
- Que Boton tenga la señal signal_clic()




**Ejercicio 25** 

- Crear una clase base llamada Instrumento y las clases derivadas Guitarra, Bateria y Teclado.  
- La clase base tiene una función virtual pura llamada ``sonar()``. 
- Defina una función virtual ``verlo()`` que publique la marca del instrumento. Por defecto todos los instrumentos son de la marca Yamaha. 
- Utilice en la función ``main()`` un ``std::vector`` para almacenar punteros a objetos del tipo Instrumento. Instancie 5 objetos y agréguelos al ``std::vector``.
- Publique la marca de cada instrumento recorriendo el vector.
- En las clases derivadas agregue los datos miembro "``int cuerdas``", "``int teclas``" e "``int tambores``" según corresponda. Por defecto, guitarra con 6 cuerdas, teclado con 61 teclas y batería con 5 tambores.
- Haga que la clase ``Teclado`` tenga herencia múltiple, heredando además de una nueva clase ``Electrico``. Todos los equipos del tipo "``Electrico``" tienen por defecto un voltaje de 220 volts. Esta clase deberá tener un destructor que al destruirse publique la leyenda "Desenchufado".
- Al llamar a la función ``sonar()``, se deberá publicar "Guitarra suena...", "Teclado suena..." o "Batería suena..." según corresponda.
- Incluya los métodos ``get`` y ``set`` que crea convenientes.

**Ejercicio 26** 

- Definir dos QWidgets (una clase Login y una clase Ventana).
- El Login validará al usuario contra una base SQLite
- La ventana Ventana sólo mostrará un QPushButton para "Volver" al login.
- Crear solamente un objeto de Ventana y uno solo de Login.













