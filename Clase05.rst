.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 05 - PGE 2017
===================

**Resololución del Ejercicio LineaDeTexto:**

.. code-block::

	#include <QApplication>
	#include <QLineEdit>
	#include <QString>

	class LineaDeTexto : public QLineEdit  {
	    // Q_OBJECT 
	    // Si usamos Q_OBJECT sin separar la definicion de esta clase en su .h y .cpp puede no compilar
	    // Recordar que sin el Q_OBJECT no podremos definir signals ni slots en esta clase

	public:
	    LineaDeTexto(QString texto = "") : QLineEdit(texto)  {  }

	    // El constructor copia debe invocar explicitamente al constructor de 
	    // la clase base para que el compilador no tire un warning
	    LineaDeTexto(const LineaDeTexto & linea) : QLineEdit()  {
	        this->setText(linea.text());
	    }

	    LineaDeTexto& operator=(const LineaDeTexto & linea)  {
	        this->setText(linea.text());
	        return *this;
	    }

	    LineaDeTexto operator+(const LineaDeTexto & linea)  {
	        return LineaDeTexto(this->text() + linea.text());
	    }
	};

	int main(int argc, char *argv[])  {
	    QApplication a(argc, argv);
	    LineaDeTexto linea1("Hola ");
	    LineaDeTexto linea2("che");
	    LineaDeTexto total;

	    total = linea1 + linea2;
	    total.show();

	    return a.exec();
	}


**Ejercicio:** 

- Incorporar LineaDeTexto a un proyecto de Qt para promocionarlo en QtDesigner
- Crear un Formulario con QtDesigner que tenga 4 LineaDeTexto promocionadas
- El formulario será para alta de personas
- Un campo para Nombre, otro para Apellido, para DNI y uno para Nombre completo.
- Esta última LineaDeTexto concatenará en tiempo real el nombre y apellido usando el operator+ de LineaDeTexto



