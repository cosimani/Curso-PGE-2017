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


**Otro ejemplo: Función callback**

.. code-block:: c++

	#ifndef BOTONES_H
	#define BOTONES_H

	class Boton{
	public:
	    virtual void click()  {  }
	};

	template <class T> class BotonCallBack : public Boton  {
	private:
	    T *destinatario;
	    void (T::*callback)(void);
	public:
	    BotonCallBack(T *otro, void (T::*puntero_funcion)(void))
	        : destinatario(otro), callback(puntero_funcion)  {  }
	
	    void click()  {
	        (destinatario->*callback)();
	    }
	};

	#endif // BOTONES_H

.. code-block:: c++

	#ifndef REPRODUCTOR_H
	#define REPRODUCTOR_H

	#include <QDebug>

	class MP3Player{
	public:
	    void play()  {
	        qDebug() << "Escuchando...";
	    }
	};

	#endif // REPRODUCTOR_H

.. code-block:: c++

	#include <QApplication>
	#include "botones.h"
	#include "reproductor.h"

	int main(int argc, char** argv)  {
	    QApplication a(argc, argv);

	    MP3Player mp3;
	    BotonCallBack<MP3Player> *boton;

	    //Conecta un MP3Player a un botón
	    boton = new BotonCallBack<MP3Player>(&mp3, &MP3Player::play);

	    boton->click();

	    return 0;
	}


