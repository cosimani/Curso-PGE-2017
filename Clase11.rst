.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 11 - PGE 2016
===================

Base de datos con SQLite
========================

.. figure:: images/clase09/sqlite1.png

.. figure:: images/clase09/sqlite2.png

.. figure:: images/clase09/sqlite3.png

**Ejercicio 4**

.. figure:: images/clase09/ejercicio4.png

.. figure:: images/clase09/ejercicio4a.png

.. figure:: images/clase09/ejercicio4b.png

**Para independizar del SO**

.. figure:: images/clase09/independizar.png

**Consulta a la base de datos**

.. figure:: images/clase09/consultar1.png

.. figure:: images/clase09/consultar2.png

Utilización de cámaras de video con Qt
======================================

- Clase QCamera: Controlador de las cámaras
- Clase QCameraViewfinder: Es un QWidget visualizador de imágenes de la cámara
- Clase QCameraInfo: Listado de las cámaras disponibles y la info de cada una
- Requiere en el .pro: QT += multimedia multimediawidgets #Qt5.3 mínimo

**Publicar la descripción de las cámaras disponibles**

.. code-block::

	QList<QCameraInfo> cameras = QCameraInfo::availableCameras();
	for (int i=0 ; i<cameras.size() ; i++)  
	    qDebug() << cameras.at(i).description();

**Instanciar QCamera y mostrar los frames sobre el QCameraViewfinder**

.. code-block::

    QCameraInfo cameraInfo = cameras.at(0);
    QCamera * camera = new QCamera(cameraInfo);

    QCameraViewfinder *visor = new QCameraViewfinder;

    camera->setViewfinder(visor);
    camera->start();

    visor->show();

**Creación de un visor promovido a QWidget para QtDesigner**

.. code-block::

	// Puede estar sólo en el .h (en visor.h)
	#ifndef VISOR_H
	#define VISOR_H

	#include <QCameraViewfinder>

	class Visor : public QCameraViewfinder  {
	    Q_OBJECT
	public:
	    explicit Visor(QWidget *parent = 0 ) : QCameraViewfinder(parent)  {   }
	};

	#endif // VISOR_H

**Ejercicio 1:**

- Crear una aplicación con un QCameraViewfinder promovido a QWidget en QtDesigner
- Un botón "Mostrar imagen" para que encienda la cámara y muestre la imagen

**Ejercicio 2:**

- Una aplicación que complete un QComboBox con las cámaras disponibles
- Un QPushButton para iniciar la cámara seleccionada

**Resolución**

:Código fuente: https://github.com/cosimani/Curso-PGE-2015/tree/master/sources/clase10/camera





