.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 22 - PGE 2016
===================

Ejercitación para segundo parcial
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
	 
Ejercicios con Procesamiento de Imágenes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Recomendaciones**

- No dejar de ver el video de Youtube sobre Excepciones
- Tener en cuenta todos los ejercicios sobre punteros a funciones
- Rápidamente debería poder hacer un hilo con QThread
- Dominar LineaDeTexto

**Ejercicio 1:** Seguimiento de objetos

- Se desea crear una aplicación que permita realizar el seguimiento de objetos según su color
- La GUI mostrará la cámara y permitirá hacer clic sobre un píxel y tomará los colores RGB
- A partir de esto, se realizará un seguimiento de este objeto. Dibujar un círculo o una marca cualquiera sobre este objeto.
- **Mejora 1:** Corregir el parpadeo que tiene la imagen
- **Mejora 2:** Corregir la orientación de la imagen
- **Mejora 3:** Trabajar con matices para identificar color. Se puede usar el método:

.. code-block:: c++	

	int QColor::hue() const

**Ayuda para Ejercicio 1:** 

- `Descargar proyecto Seguimiento desde aquí <https://github.com/cosimani/Curso-PGE-2015/blob/master/sources/clase20/seguimiento.rar?raw=true>`_

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
	
Ejemplo de un proyecto base para el parcial
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Permite desplazarse con teclado y mouse dentro de la escena OpenGL
- `Descargar proyecto base desde aquí <https://github.com/cosimani/Curso-PGE-2015/blob/master/sources/clase21/DesplazamientoEnEscena.rar?raw=true>`_
- Notar lo siguiente:
	- Método para dibujar plano horizontal y vertical
	- Control del mouse para la rotación
	- Teclas para el desplazamiento hacia adelante y atrás
	- Forma de organizar las carpetas
- Tener en cuenta:
	- Se puede pedir mirar para arriba y abajo
	- Saltar
	- Desplazarse hacia laterales

Ejercicio 2
^^^^^^^^^^^

- Dentro de la escena mostrar la cámara.
- Que permita pausar la cámara con la letra P.
	