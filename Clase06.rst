.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 06 - PGE 2016
===================

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


**Ejercicio:** Modificar este ejemplo para que la imagen rote sobre su centro.

