.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 04 - POO 2020
===================
(Fecha: 25 de marzo)


Para responder las preguntas sobre los videos
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Descargar y compilar `https://github.com/cosimani/Opcionables <https://github.com/cosimani/Opcionables>`_
- Ejecutar y loguearse con su nombre de usuario de MiUBP y la clave es su número de legajo.



Clases
======

.. code-block:: c

	class ClaseEjemplo  {
	    // Lista de miembros (generalmente funciones y datos)
	    // Los datos no pueden ser inicializados (es una declaración)
	    // Si las funciones se definen fuera, se usa el operador :: 
	    // :: es el operador de acceso a ámbito
	};

**Ejemplo:**

.. code-block:: c

	#include <iostream>
	using namespace std;

	class Punto  {
	private:
	    // Datos miembro de la clase "Punto"
	    int a, b;
		
	public:
	    // Funciones miembro de la clase "Punto"
	    void getDatos( int &a2, int &b2 );
	    void setDatos( int a2, int b2 )  {
	        a = a2;
	        b = b2;
	    }
	};

	void Punto::getDatos( int &a2, int &b2 )  {
	    a2 = a;
	    b2 = b;
	}

	int main()  {
	    Punto punto1;
		int x, y;  // Variables donde se copiarán los valores de punto1

	    punto1.setDatos( 12, 32 );
	    punto1.getDatos( x, y );

	    cout << "(" << x << “, ” << y << “)” << endl;
	}
	
	// La función "setDatos()" se definió en el interior de la clase (lo haremos sólo cuando
	// la definición sea muy simple, ya que dificulta la lectura y comprensión del programa). 

**Constructor**

.. code-block:: c

	class Punto  {
	public:
	    Punto( int a2, int b2 );

	    void getDatos( int &a2, int &b2 );
	    void setDatos( int a2, int b2 );
		
	private:
	    // Datos miembro de la clase "Punto"
	    int a, b;
	};

	Punto::Punto( int a2, int b2 )  {
	    a = a2;
	    b = b2;
	}

	void Punto::getDatos( int &a2, int &b2 )  {
	    a2 = a;
	    b2 = b;
	}

	void Punto::setDatos( int a2, int b2 )  {
	    a = a2;
	    b = b2;
	}

**Cuestiones sobre declaraciones**

.. code-block:: c

	Punto punto1;  // Llama al constructor sin parámetros. En esta última versión 
	               // de Punto, esto no serviría, ya que no hay constructor sin parámetros. 
				   // Si no se especifica un constructor, el compilador crea uno (igual que 
				   // en Java). Por lo tanto, esta declaración sirve para una clase Punto 
				   // donde el programador no escriba constructor.

	Punto punto1();  // Se entiende como el prototipo de una función sin parámetros que 
	                 // devuelve un objeto Punto. Es decir, no sirve para instanciar un 
					 // objeto con el contructor sin parámetros de Punto.

	Punto punto1( 12, 43 );  // Válido
	Punto punto2( 45, 34 );  // Válido


**Inicialización de objetos**

.. code-block:: c

	Punto( int a2, int b2 )  {
	    a = a2;
	    b = b2;
	}

	// O también se permite:

	Punto::Punto( int a2, int b2 ) : a( a2 ), b( b2 )  {  }

	Punto::Punto() : a( 0 ), b( 0 )  {  }

**El puntero this**

.. code-block:: c

	#include <iostream>
	using namespace std;

	class Punto  {
	public:
	    // Constructor
	    Punto( int a2, int b2 )  {  }
	
	    // Funciones miembro de la clase "Punto"
	    void getDatos( int &a2, int &b2 )  {  }
	    void setDatos( int a2, int b2 );
	
	private:
	    // Datos miembro de la clase "Punto"
	    int a, b;
	};

	void Punto::setDatos( int a2, int b2 ) {
	    a = a2;
	    b = b2;
	}

	// O lo podemos hacer con this:

	void Punto::setDatos( int a2, int b2 ) {
	    this->a = a2;
	    this->b = b2;
	}


**Destructor**

.. code-block:: c

	ClaseA::~ClaseA()  {
	    a = 0;
	    b = 0;
	}
	


**Ejercicio 2**

- Crear un vector de 100 números enteros.
- Los valores serán aleatorios y positivos menores o iguales a 10.
- Utilizar un algoritmo para ordenar de menor a mayor estos números.







