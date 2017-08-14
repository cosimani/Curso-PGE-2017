.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 21 - PGE 2016
===================

Ejercicio con punteros a funciones y ruteo
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Pensar en la siguiente topología

.. figure:: images/clase21/topologia.png

- `Descargar el siguiente proyecto Qt <https://github.com/cosimani/Curso-PGE-2016/blob/master/resources/clase21/redes.rar?raw=true>`_
- En la clase Router definir un método que reciba como parámetro un puntero a función, por ejemplo:

.. code-block:: c++	

	void setTabla( QList<QStringList> (*puntero) () );
	
- El puntero apunta a una función global en el archivo de cabecera rutas.h donde existen varias funciones que generan distintas tablas de ruteo.
- Notar que cada router tiene su propia tabla de ruteo


Ejercicios para OpenGL y Procesamiento de Imágenes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

.. ..

 <!--- Esta es la forma para ocultar texto. Ver instrucciones más abajo para ampliar.

 Usabilidad
 ^^^^^^^^^^

 - Se refiere a la capacidad de ser comprendido, aprendido, usado y ser atractivo.

 - El concepto de usabilidad involucra:
	- Aprendizaje
	- Eficiencia (que se logre la tarea o meta)
	- Recordación
	- Manejo de errores
	- Satisfacción

 **Mensajes de error**

 - Los errores ocurren por falta de conocimiento, comprensión incorrecta o equivocaciones involuntarias.
 - Es probable que el usuario esté confundido.
 - Mensajes de error demasiado genéricas no ayudan.
 - Los sistemas se recuerdan más cuando las cosas van mal.
 - Mejorar los mensajes de error es una buena forma de mejorar la interfaz.
 - Los logs de errores permiten a los desarrolladores revisar procedimientos y mejorar la documentación.
 - Se recomienda crear mensajes de error con tono positivo, especificidad y formato apropiado.

 **Tono positivo**

 - No condenar al usuario.
 - Las palabras MAL, ILEGAL, ERROR deberían eliminarse.
 - Los mensajes hostiles alteran a los usuarios no técnicos.
 - Error 800405: Fallo del método string de objeto Sistema.

 **Especificidad**

 - ERROR DE SINTAXIS  ---->  Paréntesis izquierdo sin correspondencia
 - ENTRADA ILEGAL     ---->  Escriba la primer letra Enviar, Leer o Eliminar
 - DATOS INVÁLIDOS    ---->  Los días deben estar en el intervalo 1 - 31
 - NOMBRE INVÁLIDO    ---->  El archivo C:\Datos\datos.txt no existe

 **Formato apropiado**  

 - Los mensajes que comienzan con un código numérico y misterioso no sirven a los usuarios comunes.
 - Llamar la atención pero sin molestar al usuario.
 - Mostrar un cuadro de texto cerca del problema pero sin ocultarlo.
 --->

 <!--- Need blank line before this line (and the .. line above).
 HTML comment written with 3 dashes so that Pandoc suppresses it.
 Blank lines may appear anywhere in the comment.

 All non-blank lines must be indented at least one space.
 HTML comment close must be followed by a blank line and a line
 that is not indented at all (if necessary that can be a line
 with just two periods followed by another blank line).
 --->
 

Temas para segundo parcial
^^^^^^^^^^^^^^^^^^^^^^^^^^

- Imágenes de cámaras con QCameraViewfinder y QAbstractVideoSurface (traer cámara USB si van a usar PC de laboratorio, o pedirme que tengo).
- Análisis pixel a pixel, escala de grises, negativo, eliminar componente
- Tratamiento de excepciones
- Hilos con QThread
- Callback, punteros a funciones, punteros a métodos de clases
- LineaDeTexto con sugerencia de Google
- OpenGL, texturas, imagen de la cámara como textura
