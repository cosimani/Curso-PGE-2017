.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 14 - PGE 2016
===================

Uso de atributos estáticos
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: c++

	// Archivo archivador.h
	#ifndef ARCHIVADOR_H
	#define ARCHIVADOR_H

	#include <QFile>
	#include <QTextStream>

	class Archivador  {
	private:
		static QFile *file;

	public:
		static bool abrir(QString ruta);
		static bool almacenar(QString texto);
	};
	
	#endif // ARCHIVADOR_H

	
.. code-block:: c++

	// Archivo archivador.cpp
	#include "archivador.h"

	QFile * Archivador::file = new QFile("./defecto.txt");

	bool Archivador::abrir(QString ruta)  {
		file->setFileName(ruta);

		if (!file->exists())  {
			return false;
		}

		return file->open(QIODevice::Append | QIODevice::Text);
	}

	bool Archivador::almacenar(QString texto)  {
		if (!file->isOpen())
			return false;

		QTextStream salida(file);
		salida << texto;

		return true;
	}

**Ejercicio:**

- En el ejercicio donde se utilizó el QComboBox para elegir la cámara, incorporar Archivador.
- Que se almacene en un archivo la fecha y hora de cada vez que se inicia la cámara.

**Ejercicio:**

- Continuar con el ejercicio de la pantalla con las imágenes de la cámara
- Incorporar otro LCD mostrando como textura una imagen de Street View

**MiniExamen:**

- Miércoles 5 de octubre.
- OpenGL, texturas y cámaras.
- Traer hechos los ejercicios.



