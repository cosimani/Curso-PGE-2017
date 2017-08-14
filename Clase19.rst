.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 19 - PGE 2015
===================

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

**Ejercicio:** Este trabajo es entregable y es parte de una nota más de MiniExámenes. 

- **Entrega:** 1ero de noviembre hasta las 23.59 horas. 
- Agregar la funcionalidad de sugerencias a la clase LineaDeTexto y que dichas sugerencias las busque desde Google.
- Ejemplo en: http://doc.qt.io/qt-5/qtnetwork-googlesuggest-example.html
- `Descargar LineaDeTexto con sugerencias de Google aquí <https://github.com/cosimani/Curso-PGE-2015/blob/master/sources/clase18/lineadetexto.rar?raw=true>`_
- Implementar LineaDeTexto en un Formulario de alta de directivos de empresas (pide Nombre completo, Empresa y Cargo)
- Suponga que un Call Center llama a las empresas para dar de alta a los directivos. 
- Con apoyo de las sugerencias de Google se ingresan los datos sin errores en el nombre ni en la empresa.
- Guardar en una base Sqlite.
- Agregar en LineaDeTexto un método que recibe un puntero a una función global que aplique un filtro a las sugerencias.
- El filtro será una QStringList que contiene todas las cadenas que deberán descartarse de las sugerencias.
- El filtro se actualizará cada vez que se de de alta un nuevo directivo, la idea es que los nombre ya agregados no se sugieran más.


