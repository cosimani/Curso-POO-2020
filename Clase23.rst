.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 23 - POO 2019
===================
(Fecha: 14 de junio)


Rotación de la escena
^^^^^^^^^^^^^^^^^^^^^

- Gira un ángulo en sentido contrario a las agujas del reloj.
- Sobre el eje formado desde el origen hasta el punto (x, y, z).

.. code-block:: c

	// glRotatef( angulo, x, y, z ); 
	glRotatef( 5, 0, 0, 1 );  // gira 5° con respecto al eje z

Traslación de la escena
^^^^^^^^^^^^^^^^^^^^^^^

- Desplaza el punto (0, 0, 0) a la nueva posición (x, y, z).

.. code-block:: c

	// glTranslatef( x, y, z );
	glTranslatef( 2, 0, 0 );  // Desplaza 2 unidades en el eje x

Escalado de la escena
^^^^^^^^^^^^^^^^^^^^^

- Escala. Con valores mayores a 1, se amplía. Entre 0 y 1 se reduce.

.. code-block:: c

	// glScalef( x, y, z );
	glScalef( 1, 2, 1 );  // escala el doble en vertical
	
Objetos ocultos
^^^^^^^^^^^^^^^

- En 3D un objeto puede estar detrás de otro.
- Por defecto, OpenGL no tiene en cuenta esto. Pinta siguiendo el orden en el código fuente,.
- El siguiente código no se vería muy real:

.. code-block:: c

	glColor3f( 0, 1, 0 );
	glBegin( GL_TRIANGLES );
	    glVertex3f( -5, -5, 5 );
	    glVertex3f( 0, 0, 0 );
	    glVertex3f( 5, -5, 5 );
	glEnd();

	glColor3f( 0, 0, 1 );
	glPointSize( 5 );
	glBegin( GL_POINTS );
	    glVertex3f( 0, -1, 0 );
	    glVertex3f( 0, -2, 5 );
	glEnd();

- Para solucionar activamos el buffer de profundidad

.. code-block:: c

	glEnable( GL_DEPTH_TEST ); 

- Cada vez que se renderiza la escena, limpiamos la pantalla

.. code-block:: c

	glClear( GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT );

Seguimiento continuo del mouse
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Al usar ``mouseMoveEvent`` ¿por qué sólo se sigue al mouse al presionar un botón?

.. code-block:: c

	setMouseTracking(bool enable)

- Es un método de la clase QWidget
- Activa el seguimiento continuo del mouse sobre un QWidget.
- Por defecto se encuentra desactivado.
- Cuando está desactivado sólo se reciben los eventos del movimiento del mouse cuando al menos se presiona un botón del mismo.

**Ejercicio 28**

- Dibujar un cajón deforme sin tapa con un color distinto en cada lado.
- Utilizar el teclado para hacerlo rotar sobre los tres ejes.



Modelo de sombreado
^^^^^^^^^^^^^^^^^^^

- Lo especificamos con la función ``glShadeModel()``. ``(shade = tono - matiz)``
- Si el parámetro es ``GL_FLAT`` se rellena con el úlimo color activo. ``(flat = plano)``
- Con ``GL_SMOOTH`` se interpolan los colores de cada vértice. ``(smooth = suavizar)``

.. code-block:: c
     
	glShadeModel( GL_SMOOTH );	
	glBegin( GL_TRIANGLES );
	    glColor3f( 1, 0, 0 ); // activamos el color rojo
	    glVertex3f( -1.0f, -0.5f, 0.0f );
	    glColor3f( 0, 1, 0 ); // activamos el color verde
	    glVertex3f( 1.0f, 0.0f, 0.0f );
	    glColor3f( 0, 0, 1 ); // activamos el color azul
	    glVertex3f( 0.3f, 1.0f, 0.0f );
	glEnd();

**Transformación de viewport (o vista)**

- Análogamente con una cámara de fotos, es el tamaño de la fotografía.
- Generalmente se inicializa para que ocupe toda la ventana.
- Pensar en la relación ancho / alto.

.. code-block:: c

	void glViewport( GLint x, GLint y, GLsizei width, GLsizei height );
	
**Proyecciones**

- La proyección define el volumen del espacio que va a usarse para formar la imagen.
- Los vértices de la escena es afectada por la matriz de proyección.
- Es necesario activarla e inicializarla:

.. code-block:: c

	glMatrixMode( GL_PROJECTION );
	glLoadIdentity();

**Proyección ortogonal**

- Define un volumen de la vista como una "caja".
- La distancia de un objeto a la cámara no influye en su tamaño.

.. code-block:: c

	void glOrtho( GLdouble left, GLdouble right, 
	              GLdouble bottom, GLdouble top, 
	              GLdouble near, GLdouble far )

.. figure:: images/clase21/ortogonal.png

.. figure:: images/clase21/proyeccion_ortogonal.png

**Proyección perspectiva**

- Define un volumen de la vista como una pirámide truncada (o frustum).
- Los objetos aparecen más pequeños mientras más alejados están de la cámara.

.. code-block:: c

	void glFrustum( GLdouble left, GLdouble right, 
	                GLdouble bottom, GLdouble top, 
	                GLdouble near, GLdouble far )
	
.. figure:: images/clase21/frustum.png	

.. code-block:: c

	void gluPerspective(angulo, aspecto, znear, zfar);

.. figure:: images/clase21/perspective.png	

- Es muy común usar:

.. code-block:: c

	gluPerspective( 45.0f, ( GLfloat )( width / height ), 0.01f, 100.0f );
	// donde width y height es el ancho y alto de la escena

- Para utilizar ``gluPerspective`` es necesario linkear a la librería en el .pro:

.. code-block:: c
	
	// Para Linux
	unix:LIBS += "/usr/lib/x86_64-linux-gnu/libGLU.so"

	// Para Windows
	win32::LIBS += -lGLU	

	// Posiblemente también requiera incluir el archivo de cabecera:
	#include <GL/glu.h>

**Ejercicio 29**

- Dibujar un triángulo dentro del campo de visión de la escena.
- Active un temporizador (100 ms) para que gire 3° el triángulo sobre el eje z.	
		   

**Posicionando la cámara**

- La siguiente función realiza el efecto del posicionamiento de la cámara.

.. code-block:: c

	void gluLookAt( GLdouble ojoX, GLdouble ojoY, GLdouble ojoZ, 
	                GLdouble haciaX, GLdouble haciaY, GLdouble haciaZ, 
	                GLdouble upX, GLdouble upY, GLdouble upZ )
				   
.. figure:: images/clase22/lookat.png		

**Ejercicio 30**

- Marcar 4 puntos en la escena donde se haga clic con el mouse.
- Ni bien se marque el 4to, automáticamente se generará el polígono de 4 vértices.
- Con la tecla C se puede cambiar entre distintos colores de relleno.
- Con A y D se rota sobre el eje Y.
- Con W y S se rota sobre el eje X.

**Ejercicio 31**

- Dibujar un cuadrado cualquiera en el plano z=-2.
- Controlar la posición de la cámara con las teclas.
- La cámara siempre vertical y mirando al punto ( 0, 0, -100 ).

**Ejercicio 32**

- Dibujar una ruta con la línea blanca interrumpida.
- Con las teclas Up y Down acelerar y frenar




