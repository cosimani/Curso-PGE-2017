.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 20 - PGE 2017
===================
(Fecha: 1 de noviembre)

MiniExamen de preguntas múltiples
=================================

:Tarea para Clase 21 (7 de noviembre):
	Ver `Tutorial Qt Designer <https://www.youtube.com/watch?v=na0dOHmLFYI>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_
	
	Ver `Tutorial Qt Creator - Icono de la aplicación <https://www.youtube.com/watch?v=eM9ItsibSjc>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_
	
	Ver `Tutorial Qt Creator - QMessageBox <https://www.youtube.com/watch?v=pEjzODGZCxk>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_
	
	Ver `Tutorial Qt Creator - QCompleter <https://www.youtube.com/watch?v=VmDVprlLupo>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_
	
	Ver `Tutorial Qt Creator - QSystemTrayIcon <https://www.youtube.com/watch?v=Fe1L6u064ao>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_
	
	Ver `Tutorial Qt Creator - QMouseEvent <https://www.youtube.com/watch?v=5dI0u84VGoY>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_
	
	Ver `Tutorial Qt Creator - QResizeEvent <https://www.youtube.com/watch?v=2mFuXsgJBoI>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_

	Ver `Tutorial Qt Creator - QKeyEvent <https://www.youtube.com/watch?v=44fCm1KlQGY>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_


**Ejercicio:** Seguimiento de objetos

- Se desea crear una aplicación que permita realizar el seguimiento de objetos según su color
- La GUI mostrará la cámara y permitirá hacer clic sobre un píxel y tomará los colores RGB
- A partir de esto, se realizará un seguimiento de este objeto. Dibujar un círculo o una marca cualquiera sobre este objeto.
- **Mejora 1:** Corregir el parpadeo que tiene la imagen
- **Mejora 2:** Corregir la orientación de la imagen
- **Mejora 3:** Trabajar con matices para identificar color. Usar el siguiente método:

.. code-block:: c++	

	int QColor::hue() const

**Ayuda para Ejercicio:** 

- `Descargar proyecto Seguimiento desde aquí <https://github.com/cosimani/Curso-PGE-2017/blob/master/sources/clase20/seguimiento.rar?raw=true>`_

- También puede usar el siguiente `Código fuente <https://github.com/cosimani/Curso-PGE-2017/blob/master/sources/clase19/camaraOgl.zip?raw=true>`_

- Dispone de la clase Capturador que tiene el siguiente método:

.. code-block:: c++	
	
	/**
	 * @brief Metodo que devuelve el cuadro actual.
	 * @return QVideoFrame que luego se puede convertir en QImage
	 */
	QVideoFrame getFrameActual()  {  return frameActual;  }

- Configurando un QTimer podríamos obtener el cuadro actual, procesarlo y publicarlo sobre algún QWidget que esté promocionado en QtDesigner
- Para encender la cámara necesitamos hacer:

.. code-block:: c++	

	Capturador * capturador = new Capturador;
	QCamera * camera;

	QList<QCameraInfo> listaCamaras = QCameraInfo::availableCameras();
	
	if ( ! listaCamaras.empty() )  {  // Si hay al menos una camara
	    camera = new QCamera(listaCamaras.at(0));  // Preparamos la primera camara disponible
	    camera->setViewfinder(capturador); 
	    camera->start();  // Encendemos la primera
	}

- Esto hace que se vayan levantando las imágenes de la cámara pero no se visualizarán en ningún lado. Esto trabaja distinto a QCameraViewfinder.
- Necesitamos entonces obtener este QVideoFrame, quizás convertirlo a QImage y dibujarlo sobre algún QWidget.
- Para convertir de QVideoFrame a QImage se puede hacer lo siguiente:

.. code-block:: c++	

	QVideoFrame frameActual = capturador->getFrameActual();

	QImage::Format imageFormat = QVideoFrame::imageFormatFromPixelFormat(frameActual.pixelFormat());

	QImage image( frameActual.bits(),
	              frameActual.width(),
	              frameActual.height(),
	              frameActual.bytesPerLine(),
	              imageFormat );


**Ejercicio:** 

- Usar la técnica de Croma (https://es.wikipedia.org/wiki/Croma) para eliminar el fondo de las imágenes de la cámara
- Utilizar el mouse para elegir un pixel, el cual será tomado como el color que se eliminará.
- Colocar un botón que permita abrir el disco y elegir la imagen que será colocada como fondo.