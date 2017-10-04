.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 06 - PGE 2017
===================
(Fecha: 30 de agosto)

Captura de eventos con eventFilter
==================================

- En un QWidget dibujar una imagen de 50x50.
- Con un QTimer hacer rotar la imagen cada 20 mseg.
- Permitir desplazar la imagen con las teclas y usando eventFilter.
- Utilizar los métodos rotate y translate de QPainter.

.. code-block::

	#include <QWidget>

	class QTimer;
	class QImage;

	namespace Ui  {
		class Principal;
	}

	class Principal : public QWidget  {
		Q_OBJECT

	public:
		explicit Principal(QWidget *parent = 0);
		~Principal();

	private:
		Ui::Principal *ui;
		QTimer* timer;
		int rotacion, transX, transY;
		QImage * image;

	protected:
		void paintEvent(QPaintEvent * e);
		bool eventFilter(QObject *, QEvent *);

	private slots:
		void slot_ciclos();
	};


	#include "principal.h"
	#include "ui_principal.h"
	#include <QTimer>
	#include <QPainter>
	#include <QKeyEvent>
	#include <QImage>



	Principal::Principal(QWidget *parent) : QWidget(parent), ui(new Ui::Principal),
							 rotacion(0), transX(0), transY(0)
	{
		ui->setupUi(this);
		this->installEventFilter(this);
		image = new QImage(":/User.png");
		timer = new QTimer;
		timer->setInterval(20);
		connect(timer, SIGNAL(timeout()), SLOT(slot_ciclos()));
		timer->start();
	}

	Principal::~Principal()  {
		delete ui;
	}

	void Principal::paintEvent(QPaintEvent * e)  {
		QPainter* p = new QPainter(this);
		p->translate(this->width()/2 + transX, this->height()/2 + transY);
		p->rotate(10*++rotacion);
		p->drawImage(0, 0, *image);
		if (rotacion>=360)
			rotacion = 0;
	}

	void Principal::slot_ciclos()  {
		this->repaint();
	}

	bool Principal::eventFilter(QObject *obj, QEvent *e)  {
		if (obj == this)  {
			if (e->type() == QEvent::KeyPress)  {
				int key = static_cast<QKeyEvent*>(e)->key();
				switch(key)  {
				case Qt::Key_Up:
					transY -= 10;
					break;
				case Qt::Key_Down:
					transY += 10;
					break;
				case Qt::Key_Left:
					transX -= 10;
					break;
				case Qt::Key_Right:
					transX += 10;
					break;
				case Qt::Key_Escape:
					this->close();
					break;
				}

				// Devolvemos true para indicar que este evento ya lo controlamos aquí
				// y no es necesario que se propague para que alguien más lo controle.
				return true;  
			}
		}

		// Esto es para que la clase base decida si necesita controlar los eventos
		// que no estamos capturando nosotros.
		return QWidget::eventFilter(obj, e);
	}


**Ejercicio 8:** Modificar este ejemplo para que la imagen rote sobre su centro.

**Ejercicio 9:** Cada vez que el usuario desplace la ventana Principal, que al soltarlo se coloque centrado en pantalla.



Clase QCompleter
================

.. figure:: images/clase03/qcompleter.png

**Ejercicio 10:** Hacer lo mismo pero la base de datos que sea un archivo SQLite.

**Ejercicio 11:** Definir una clase genérica Vector que herede de QVector.

- Modificar el comportamiento de los siguientes métodos:
	- at() - En vez de obtener el primer elemento con 0 que lo haga con 1, el 1 con 2,...
	- size() - Bloquearlo, que no se pueda usar.
	- float getPromedio() - Si los elementos son int o float, devolver el promedio sino devolver -1




