.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 21 - PGE 2017
===================
(Fecha: 7 de noviembre)

Ejercitación para segundo parcial
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Recomendaciones**

- No dejar de ver el video de Youtube sobre Excepciones
- Tener en cuenta todos los ejercicios sobre punteros a funciones
- Rápidamente debería poder hacer un hilo con QThread


Temas para segundo parcial
^^^^^^^^^^^^^^^^^^^^^^^^^^

- Imágenes de cámaras con QCameraViewfinder y QAbstractVideoSurface (traer cámara USB si van a usar PC de laboratorio, o pedirme que tengo).
- Análisis pixel a pixel, escala de grises, negativo, eliminar componente
- Tratamiento de excepciones
- Hilos con QThread
- Callback, punteros a funciones, punteros a métodos de clases
- OpenGL, texturas, imagen de la cámara como textura


Ejemplo de un proyecto base para el parcial
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Permite desplazarse con teclado y mouse dentro de la escena OpenGL
- `Descargar proyecto base desde aquí <https://github.com/cosimani/Curso-PGE-2017/blob/master/sources/clase21/DesplazamientoEnEscena.rar?raw=true>`_
- Notar lo siguiente:
	- Método para dibujar plano horizontal y vertical
	- Control del mouse para la rotación
	- Teclas para el desplazamiento hacia adelante y atrás
	- Forma de organizar las carpetas
- Tener en cuenta:
	- Se puede pedir mirar para arriba y abajo
	- Saltar
	- Desplazarse hacia laterales


Ejercicio 41:
============

- Dentro de la escena mostrar la cámara.
- Que permita pausar la cámara con la letra P.

Ejercicio 42:
============

- Pensar en la siguiente topología

.. figure:: images/clase21/topologia.png

- `Descargar el siguiente proyecto Qt <https://github.com/cosimani/Curso-PGE-2016/blob/master/resources/clase21/redes.rar?raw=true>`_
- En la clase Router definir un método que reciba como parámetro un puntero a función, por ejemplo:

.. code-block:: c++	

	void setTabla( QList<QStringList> (*puntero) () );
	
- El puntero apunta a una función global en el archivo de cabecera rutas.h donde existen varias funciones que generan distintas tablas de ruteo.
- Notar que cada router tiene su propia tabla de ruteo

