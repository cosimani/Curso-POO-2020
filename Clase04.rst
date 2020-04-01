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

	class Poste  {
	    // Lista de miembros (generalmente funciones y datos)
	    // Los datos no pueden ser inicializados fuera del constructor 
	    // Si las funciones se definen fuera, se usa el operador :: 
	    // :: es el operador de acceso a ámbito
	};


.. code-block:: c

	class Poste;  // Esto es la declaración de una clase.

.. code-block:: c

	class Poste  {  // Esto es la declaración y definición de una clase.
	     
	};




**Ejemplo:**

.. code-block:: c

	#include <iostream>
	
	class Poste  {
	private:
	    // Datos miembro de la clase Poste. También llamados atributos.
	    int altura;
	    int seccion;
		
	public:
	    // Funciones miembro de la clase Poste. Llamados también métodos.
	    void getDatos( int & a, int & s );
	    void setDatos( int a, int s )  {
	        altura = a;
	        seccion = s;
	    }
	};

	void Poste::getDatos( int & a, int & s )  {
	    a = altura;
	    s = seccion;
	}

	int main()  {
	    Poste poste;
	    int x, y;  // Variables donde se copiarán los valores de poste

	    poste.setDatos( 12, 32 );
	    poste.getDatos( x, y );

	    cout << "(" << x << “, ” << y << “)” << endl;
	}
	
	// La función "setDatos()" se definió en el interior de la clase (lo haremos sólo cuando
	// la definición sea muy simple, ya que dificulta la lectura y comprensión del programa). 

**Constructor**

.. code-block:: c

	class Poste  {
	private:
	    int altura;
	    int seccion;

	public:
	    Poste( int a, int s );

	    void getDatos( int & a, int & s );
	    void setDatos( int a, int s );
	};

	Poste::Poste( int a, int s )  {
	    altura = a;
	    seccion = s;
	}

	void Poste::getDatos( int & a, int & s )  {
	    a = altura;
	    s = seccion;
	}

	void Poste::setDatos( int a, int s )  {
	    altura = a;
	    seccion = s;
	}

**Cuestiones sobre declaraciones**

.. code-block:: c

	Poste poste;  // Llama al constructor sin parámetros. En esta última versión 
	              // de Poste, esto no serviría, ya que no hay constructor sin parámetros. 
	              // Si no se especifica un constructor, el compilador crea uno. 
	              // Por lo tanto, esta declaración sirve para una clase Poste 
	              // donde el programador no escriba constructor, o escriba uno sin recibir parámetros.

	Poste poste();  // Se entiende como el prototipo de una función sin parámetros que 
	                // devuelve un objeto Poste. Es decir, no sirve para instanciar un 
					// objeto con el contructor sin parámetros de Poste.

	Poste poste1( 12, 43 );  // Válido
	Poste poste2( 45, 34 );  // Válido


**Inicialización de objetos**

.. code-block:: c

	// Lo siguiente se permite y funciona casi siempre, (salvo cuando usemos const, que
	// veremos más adelante). Hay que tener presente que aquí, primero se reserva lugar 
	// en memoria para altura y seccion conteniendo basura y luego se le asignan los 
	// valores que vienen en los parámetros del constructor.
	Poste( int a, int s )  {
	    altura = a;
	    seccion = s;
	}

	// La siguiente sería la manera más correcta de inicializar los atributos de un 
	// objeto. En este caso, altura y seccion nunca contienen basura, sino que 
	// directamente se crean en memoria con el valor que vienen en los parámetros del constructor.
	Poste::Poste( int a, int s ) : altura( a ), seccion( s )  {  }

	Poste::Poste() : a( 0 ), b( 0 )  {  }

**El puntero this**

- Es un puntero que ya se exite dentro del ámbito de una clase y apunta al propio objeto instanciado.
- Se utiliza para acceder a los atributos y métodos.

.. code-block:: c

	class Poste  {
	private:
	    int altura;
	    int seccion;

	public:
	    Poste( int altura, int seccion );

	    void getDatos( int & altura, int & seccion );
	    void setDatos( int altura, int seccion );
	};

	Poste::Poste( int altura, int seccion ) : altura( altura ), seccion( seccion )  {  
	}

	void Poste::getDatos( int & altura, int & seccion )  {
	    altura = this->altura;
	    seccion = this->seccion;
	}

	void Poste::setDatos( int altura, int seccion )  {
	    this->altura = altura;
	    this->seccion = seccion;
	}


**Destructor**

.. code-block:: c

	Poste::~Poste()  {
	    altura = 0;  
	    seccion = 0;
	}
	


Convenciones para escribir nuestro código
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Los nombres de las clases, structs y enum comienzan con mayúsculas (usando ``UpperCamelCase``).
- Nombres de variables, funciones y métodos comienzan con minúsculas (usando ``lowerCamelCase`` y con palabras separadas con guión bajo).

- Ejemplos para nombres de clases: ``Persona`` - ``PrimeraClase`` - ``Ventana``
- Ejemplos para nombres de variables y funciones: ``velocidad`` - ``sumarNumeros`` - ``alto_imagen`` - ``anchoImagen``

**CamelCase**: Es escribir con la forma de jorobas de camello con las mayúsculas y minúsculas.

UpperCamelCase: La primera letra de cada palabra es mayúscula. Ejemplo: ``EjemploDeUpperCamelCase``.
lowerCamelCase: Igual a UpperCamelCase con excepción de la primer palabra. Ejemplo: ``ejemploDeLowerCamelCase``


**Ejercicio**

- Explicación de este ejercicio en un `Video tutorial en Youtube ( https://youtu.be/R3K7rQ4DSX8 ) <https://youtu.be/R3K7rQ4DSX8>`_
- Crear una clase Jugador con atributos ``int velocidad``, ``int fuerza`` y ``std::string nombre``
- Usar constructor inicializando de la manera recomendada la velocidad y fuerza en 0 y nombre "sin nombre" 
- Crear los métodos setter y getter para setear y obtener los valores de los atributos
- Incluir el destructor
- En la función main crear un ``std::vector< Jugador >`` e insertar 10 jugadores distintos
- Por último, publicar los datos de cada uno de los jugadores con ``std::cout``


**Ejercicio 2**

- Crear un vector de 100 números enteros.
- Los valores serán aleatorios y positivos menores o iguales a 10.
- Utilizar un algoritmo para ordenar de menor a mayor estos números.







