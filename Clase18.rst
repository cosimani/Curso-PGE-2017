.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 18 - PGE 2016
===================

**Array de punteros a función**

- Los punteros a funciones se pueden agruparse en arreglos

.. code-block:: c++	

	int (* afptr[10])(int);    // array de 10 punteros a función

- Los 10 punteros apuntan a funciones con el mismo prototipo
- Permiten muchas variantes para invocar funciones

.. code-block:: c++	

	int a = afptr[n](x);
	
**Resolución Ejercicio 1: Ordenador** 

.. figure:: images/clase17/ordenador.png

.. code-block:: c++

	#ifndef ORDENADOR
	#define ORDENADOR

	class Ordenador  {
	public:
	    void burbuja(int * v, int n)  {
	        int i, j, aux;
	        for(i=0 ; i<=n ; i++)  {
	            for(j=0 ; j<n-1 ; j++)  {
	                if(v[j] > v[j+1])  {
	                    aux = v[j];
	                    v[j] = v[j+1];
	                    v[j+1] = aux;
	                }
	            }
	        }
	    }

	    void insercion(int * v, int n)  {
	        int i, j, aux;
	        for (i=1 ; i<n; i++)  {
	            aux = v[i];
	            j = i - 1;
	            while ( (v[j] > aux) && (j >= 0) )  {
	                v[j+1] = v[j];
	                j--;
	            }
	            v[j+1] = aux;
	        }
	    }
	};

	#endif // ORDENADOR
	
.. code-block:: c++

	#ifndef LISTADOENTEROS_H
	#define LISTADOENTEROS_H

	#include <QVector>
	#include "ordenador.h"

	class ListadoEnteros : public QVector<int>  {
	public:

	    void ordenar(void (Ordenador::*pFuncionOrdenamiento)(int *, int))  {
	        (ordenador.*pFuncionOrdenamiento)(this->data(), this->size());
	    }

	private:
	    Ordenador ordenador;
	};

	#endif // LISTADOENTEROS_H
	
.. code-block:: c++

	#ifndef PRINCIPAL_H
	#define PRINCIPAL_H

	#include <QWidget>
	#include "listadoEnteros.h"

	namespace Ui {
	    class Principal;
	}

	class Principal : public QWidget  {
	    Q_OBJECT

	public:
	    explicit Principal(QWidget *parent = 0);
	    ~Principal();

	private:
	    Ui::Principal *ui;
	    ListadoEnteros listado;

	private slots:
	    void slot_ordenar();
	    void slot_valorNuevo();
	};

	#endif // PRINCIPAL_H

.. code-block:: c++

	#include "principal.h"
	#include "ui_principal.h"

	Principal::Principal(QWidget *parent) : QWidget(parent), ui(new Ui::Principal)  {
	    ui->setupUi(this);

	    connect(ui->pbOrdenar, SIGNAL(clicked()), this, SLOT(slot_ordenar()));
	    connect(ui->leValorNuevo, SIGNAL(returnPressed()), this, SLOT(slot_valorNuevo()));
	}

	Principal::~Principal()  {  delete ui;  }

	void Principal::slot_ordenar()  {

	    if (ui->cbMetodo->currentText() == "Burbuja")  {
	        void (Ordenador::*burbuja)(int *, int) = &Ordenador::burbuja;
	        listado.ordenar(burbuja);
	    }
	    else  {
	        void (Ordenador::*insersion)(int *, int) = &Ordenador::insercion;
	        listado.ordenar(insersion);
	    }

	    for (int i=0 ; i<listado.size() ; i++)  {
	        ui->teOrdenados->append(QString::number(listado.at(i)));
	    }
	}

	void Principal::slot_valorNuevo()  {
	    listado.push_back(ui->leValorNuevo->text().toInt());

	    ui->teValores->append(ui->leValorNuevo->text());

	    ui->leValorNuevo->clear();
	}
	
**Ejercicio:**

- Con la misma idea del ejercicio anterior. Crear la clase genérica ListadoGenerico que herede de QVector<T>
- La clase ListaGenerico tendrá el siguiente método:
	
.. code-block:: c++	
		
	void ordenar(Ordenador::*puntero_funcion)(T * v, int n))
	// Este método ordenará los elementos
	
- Notar que ordenar podrá ordenar elementos de cualquier tipo, siempre y cuando los objetos a ordenar sean de una clase que tenga sobrecargado el operador >
	

**Ejercicio:**

- Modificar el ejercicio de la clase ListadoEnteros para usar funciones globales de ordenamiento, es decir, que no se encuentren dentro de Ordenador ni de ninguna clase.

.. code-block:: c++	

	class ListadoEnteros : public QVector<int>  {
	public:
	    void ordenar(void (*pFuncionOrdenamiento)(int *, int))  {
	        (*pFuncionOrdenamiento)(this->data(), this->size());
	    }
	};

.. code-block:: c++		
	///// Desde main se puede utilizar así:

    void (*ordenador)(int *, int) = &burbuja;

    listado.ordenar(ordenador);

**Ejercicio:**

- Modificar el ejercicio anterior usando también funciones globales de ordenamiento pero con la clase ListadoGenerico que sea un template:

.. code-block:: c++	

	template<class T> class ListadoGenerico : public QVector<T>  {
	public:
	    void ordenar(void (*pFuncionOrdenamiento)(int *, int))  {
	        (*pFuncionOrdenamiento)(this->data(), this->size());
	    }
	};

**Ejercicio:**

- Necesitamos conocer el rendimiento de cada algoritmo de ordenamiento midiendo su tiempo.
- Utilizar un array de punteros a funciones que apunte a cada función global de ordenamiento.
- Utilizar Archivador para almacenar los tiempos en un archivo.
- Utilizar un ListadoEnteros de 50.000 números generados con qrand()

.. code-block:: c++		

	///// Desde main se puede utilizar así:

    void (*ordenador[2])(int *, int);
    ordenador[0] = &burbuja;
    ordenador[1] = &insercion;

    listado.ordenar(ordenador[1]);




