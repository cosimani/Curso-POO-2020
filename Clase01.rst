.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 01 - POO 2020
===================
(Fecha: 13 de marzo)

    :Autor: César Osimani
    :Mail: cesarosimani@gmail.com
    :Fecha: 13 de marzo de 2010
    :Regularidad: 
	    - 2 parciales 
	    - Parcialitos, ejercicios y cuestionarios (3er nota)
	    - Los ejericios son algunos de los que están en las clases
	    - Los ejercicios, si están como pide el enunciado se tiene un 8, para llegar al 10 tiene que ser sobresaliente, con agregados extras originales.
    :Proyecto integrador final: 
	    - Un trabajo integrador individual se puede presentar para el final (tema a elección)
    :Proyecto integrador parcial: 	   
	    - Trabajo individual se puede presentar en lugar de los dos parciales tradicionales
	    - Se van entregando avances del trabajo en las fechas de los parciales
	    - Si las entregas de avances del trabajo están para promocionar la materia, igualmente el trabajo tiene que estar finalizado durante el cursado. De lo contrario se entrega en la fecha del examen final rindiendo normalmente.
    :Trabajos propuestos: 
        - **Spotify y control de luces de ambiente**: Hay un trabajo ya iniciado y funcionando de un alumno que utiliza la API de Spotify para reproducir canciones y controlar las luces Philips Hue. La propuesta sería modificar el proyecto para controlar cualquier tipo de luces a través de alguna interfaz electrónica (Arduino o Raspberry) y que la intensidad de la iluminación esté definida por la intensidad o dinámica de la canción. Como alternativa se puede controlar una tira de leds RGB.
        - **Mapa de calor de las fotos publicadas**: Trabajo iniciado por alumnos que utilizan la API de Facebook o Instagram para extraer información de la ubicación de cada foto para hacer un mapa de calor sobre un mapa de Google con la cantidad de fotos. La propuesta es redefinir la interfaz gráfica de usuario.
        - **Opcionables**: Continuar el desarrollo de la aplicación Opcionables para redefinir la GUI.
    :Temas principales: 
		- Espacio de nombres
		- inline y const
		- std, vector, string
		- Aritmética de punteros
		- Funciones genéricas
		- Herencia y herencia múltiple
		- Polimorfismo
		- Funciones virtuales
		- Clases abstractas
		- Modificador friend
		- Biblioteca Qt
			- Uso de documentación
			- slots y signals
			- QtNetwork
			- Interfaz gráfica de usuario (widgets, layouts, etc.)
			- QPainter y captura de eventos del teclado y del mouse
			- QTimer
			- QtDesigner
		- OpenGL (glOrtho, gluPerspective, glLookAt, rotación, traslación, texturas, ...)
		- Base de datos (SELECT e INSERT).
		- Resolver los problemas consultando documentación técnica de distintas fuentes


Inicio de clases en época del COVID-19
======================================

**Documento del Ministerio de Salud**

.. figure:: images/clase01/COVID-19_mini.png
	:target: images/clase01/presentacion_presidente_Ministros_COVID-19.pdf

**¿Qué alternativas tenemos para tener clases y prevenir?**



Biblioteca estándar de C++
==========================

.. figure:: images/clase01/comparacion_cpp_c.png

Espacio de nombres (namespace)
==============================

- Permite agrupar declaraciones (de clases, funciones, etc.).
- Permite declarar identificadores (nombre de variables) sin que se solapen. Es decir, identificadores iguales en distinto namespace.
- Se pueden incluir las definiciones en un namespace pero mejor no.
- Un espacio de nombre es un ámbito.

Ejemplos con namespace
======================

**Ejemplo 1**

.. code-block:: c

	#include <iostream>
	using namespace std;

	namespace enteros  {
	    int var1 = 5;
	    int var2 = 7;
	}

	namespace con_decimales  {
	    float var1 = 5.14;
	    float var2 = 7.13;
	}

	int main()  {
	    cout << enteros::var1 << endl;
	    cout << con_decimales::var1 << endl;
	    return 0;
	}

- ¿Cuál es la salida por consola?

.. ..

 <!---  
 Publica:    5    5.14		(para ocultar requiere una primer linea con .. ..    Los que queremos ocultar debe tener el menos un espacio)
 --->

**Ejemplo 2**

.. code-block:: c

	#include <iostream>
	using namespace std;
	
	namespace enteros  {
	    int var1 = 5;
	    int var2 = 7;
	}
	
	namespace con_decimales  {
	    float var1 = 5.14;
	    float var2 = 7.13;
	}
	
	int main()  {
	    using enteros::var1;
	    using con_decimal::var2;

	    cout << var1 << endl;
	    cout << var2 << endl;
	    cout << enteros::var2 << endl;
	    cout << con_decimales::var1 << endl;

	    return 0;
	}

.. ..

 <!---  
 Publica:    5		7.13		7		5.14
 --->

**Ejemplo 3**

.. code-block:: c

	#include <iostream>
	using namespace std;

	namespace enteros  {
	    int var1 = 5;
	    int var2 = 7;
	}
	
	namespace con_decimales  {
	    float var1 = 5.14;
	    float var2 = 7.13;
	}

	int main()  {
	    using namespace enteros;

	    cout << var1 << endl;
	    cout << var2 << endl;
	    cout << con_decimales::var1 << endl;
	    cout << con_decimales::var2 << endl;

	    return 0;
	}

.. ..

 <!---  
 Publica:    5		7		5.14		7.13
 --->

**Ejemplo 4**

.. code-block:: c

	#include <iostream>
	using namespace std;

	namespace enteros  {
	    int var1 = 5;
	    int var2 = 7;
	}
	
	namespace con_decimales  {
	    float var1 = 5.14;
	    float var2 = 7.13;
	}
	
	int main()  {
	    {
	    using namespace enteros;
	    cout << var1 << endl;
	    }

	    {
	    using namespace con_decimales;
	    cout << var1 << endl;
	    }

	    return 0;
	}

.. ..

 <!---  
 Publica:    5		5.14
 --->

