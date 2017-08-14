.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 12 - PGE 2016
===================

**CLI >> GUI >> NUI**

.. figure:: images/clase11/cli_gui_nui.png
	:target: https://prezi.com/_iqvhrobpe0p/desarrollo/

**Análisis píxel a píxel**

- **Ejemplo:** Dejar sólo el componente rojo

.. code-block::

    QPixmap pixMap = ui->visor->grab();  // Para llevar el QWidget a QImage
    QImage image = pixMap.toImage();

    for (int x=0 ; x<image.width() ; x++)  {
        for (int y=0 ; y<image.height() ; y++)  {
            QRgb rgb = image.pixel(x, y);  // typedef unsigned int QRgb;
            QRgb nuevoValorRgb = qRgb(qRed(rgb), 0, 0);
            image.setPixel(x, y, nuevoValorRgb);
        }
    }

**Ejercicio:**

- Agregar un QPushButton "Capturar imagen" para procesarla
- Dibujar con paintEvent esa imagen procesada 
- Procesar la imagen con lo siguiente:
	- Invertir los colores rgb a bgr
	- Transformar a escala de grises
		Y = 0.3 R + 0.3 G + 0.3 B // Y en cada componente
	- Convertir al negativo: Cada componente, si lo leemos en binario, debemos invertir cada bit.
		- Ejemplo: R=144=10010000 -> R=01101111


Levantar frame por frame: Clase QAbstractVideoSurface
=====================================================

- QAbstractVideoSurface es una clase abstracta
- Proporciona streaming de video a través de la función virtual pura present()

.. code-block:: c++

	bool QAbstractVideoSurface::present ( const QVideoFrame & frame ) [pure virtual]


**Clase Capturador para obtener los frames de la cámara**

.. code-block:: c++

	class Capturador : public QAbstractVideoSurface  {
		Q_OBJECT

	public:
		Capturador(QObject *parent = 0);

		QList<QVideoFrame::PixelFormat> supportedPixelFormats(
				QAbstractVideoBuffer::HandleType handleType = QAbstractVideoBuffer::NoHandle) const;

		bool present(const QVideoFrame &frame);

		QVideoFrame getFrameActual()  {  return frameActual;  }

	private:
		QVideoFrame frameActual;
	};


- QVideoFrame encapsula los datos de video (bits, ancho, alto, etc.)
- Para acceder a los bits es necesario mapearlo con el método map()
- El mapeo deja en memoria los datos para se accedidos.

.. code-block:: c++

	bool Capturador::present(const QVideoFrame &frame)  {
		frameActual = frame;
		
		frameActual.map(QAbstractVideoBuffer::ReadOnly);
		
		return true;  // Con la idea de devolver true si este frame fue usado
	}


- La función virtual pura supportedPixelFormats() devuelve un listado de formatos soportados.

.. code-block:: c++

	QList<QVideoFrame::PixelFormat> 
	Capturador::supportedPixelFormats(QAbstractVideoBuffer::HandleType handleType) const  {
	
		if (handleType == QAbstractVideoBuffer::NoHandle) {
			return QList<QVideoFrame::PixelFormat>()
					<< QVideoFrame::Format_RGB32
					<< QVideoFrame::Format_ARGB32;
		} else {
			return QList<QVideoFrame::PixelFormat>();
		}
	}

**El constructor**

.. code-block:: c++

	Capturador::Capturador(QObject *parent) : QAbstractVideoSurface(parent)  {

	}



