.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 20 - PGE 2017
===================
(Fecha: 1 de noviembre)


**Ejercicio:** Seguimiento de objetos

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


**Ejercicio 1:**

- Crear un QCameraViewfinder promovido a QWidget
- Un botón para capturar la imagen de la cámara
- Con el mouse se puede dibujar encima de la imagen como un lápiz
- Un botón para almacenar la imagen resultante.

**Ayuda para este ejercicio**

- A continuación un ejemplo de un widget que permite dibujar con el mouse

.. code-block:: c++	

	class Papel : public QWidget
	{
		Q_OBJECT
	public:
		explicit Papel(QWidget *parent = 0 );

	private:
		bool botonPresionado;
		QPoint posicionMouse;

	protected:
		bool eventFilter(QObject *obj, QEvent *e);
		void paintEvent(QPaintEvent *);
	};

.. code-block:: c++	
	
	Papel::Papel(QWidget *parent) : QWidget(parent), botonPresionado(false)  {
	    this->installEventFilter(this);  // Para trabajar con eventFilter 
	}
	
	bool Papel::eventFilter(QObject *obj, QEvent *e)  {
	    if (obj == this)  {
	        if (e->type() == QEvent::MouseButtonPress) 
	            botonPresionado = true;
	        if (e->type() == QEvent::MouseButtonRelease) 
	            botonPresionado = false;
	        if (e->type() == QEvent::MouseMove)  {
	            posicionMouse = static_cast<QMouseEvent*>(e)->pos();
				
	            // Solo redibujamos el pixedl que pintamos. Esto para que no se borre lo dibujado anteriormente.				
	            this->repaint(QRect(posicionMouse.x(), posicionMouse.y(), 1, 1));
	        }
	    }
	    return QWidget::eventFilter(obj, e);
	}

	void Papel::paintEvent(QPaintEvent *)  {
	    QPainter painter(this);

	    painter.fillRect(0, 0, this->width(), this->height(), QBrush(QColor(255,255,255)));

	    painter.drawPoint(posicionMouse);
	}
	
- Para capturar la pantalla se podría agregar un slot en Papel

.. code-block:: c++	

	// Esto captura la pantalla pero solo almacena el ultimo pixel dibujado, hay que resolverlo.
	void Papel::slot_capturarImagen()  {
	    QPixmap pixMap = this->grab();
	    QImage image = pixMap.toImage();

	    image.save("../imagen.png");
	}
	
**Ejercicio 2:**

- Con Archivador almacenar cada vez que se dibuja con el lápiz
- Almacenar con el siguiente formato:
	- Fecha y hora: 21.10.2014-20:53:42 - Píxel inicio: (153, 230) - Fin: (51, 76)
	
**Ejercicio 3:**

- Definir métodos para realizar procesamiento de las imágenes para:
	- Convertir a grises
	- Llevar a negativo
	- Eliminar algún color
- El prototipo puede ser:
	- QImage getGrayImage(QImage imagenOriginal);

**Ejercicio 4:**

- Imágenes de Google Street View en OpenGL