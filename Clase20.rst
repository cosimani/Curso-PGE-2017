.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 20 - PGE 2016
===================

Propuesta para Mini Examen 3
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: c++	

	#ifndef ADMINDB_H
	#define ADMINDB_H

	#include <QObject>
	#include <QSqlDatabase>

	class AdminDB : public QObject
	{
	    Q_OBJECT

	private:
	    static AdminDB *instancia;
	    explicit AdminDB(QObject *parent = 0);

	public:
	    static AdminDB *getInstancia();
	    ~AdminDB();

	    bool conectar();
	    void desconectar();
	    QStringList getNombres();
	
	private:
	    QSqlDatabase db;
	};

	#endif // ADMINDB_H

.. code-block:: c++	

	#include "admindb.h"
	#include <QDebug>
	#include <QSqlQuery>
	#include <QSqlRecord>
	#include <QSqlError>

	AdminDB *AdminDB::instancia = NULL;

	AdminDB::AdminDB(QObject *parent) : QObject(parent)  {
	    db = QSqlDatabase::addDatabase("QSQLITE");
	}

	AdminDB *AdminDB::getInstancia()  {
	    if( !instancia )  {
	        instancia = new AdminDB();
	    }
	    return instancia;
	}

	AdminDB::~AdminDB()  {
	    if (db.isOpen())
	        db.close();
	}

	bool AdminDB::conectar()  {
	    db.setDatabaseName("../db/db.sqlite");

	    return db.open();
	}

	void AdminDB::desconectar()  {
	    db.close();
	}

	QStringList AdminDB::getNombres()  {
	    QStringList nombres;

	    if ( this->conectar() )  {
	        QSqlQuery query( db );

	        query.exec("SELECT nombre FROM nombres");

	        while ( query.next() )  {
	            QSqlRecord registro = query.record();
	            nombres << registro.value(0).toString();
	        }

	        //  qDebug() << query.lastError() << query.lastQuery();
	        this->desconectar();
	    }
	    else
	        qDebug() << "No se encuentra conectado a la base";

	    return nombres;
	}

.. code-block:: c++	

	#ifndef FILTROS_H
	#define FILTROS_H

	#include <QStringList>
	#include "admindb.h"

	QStringList filtroNombres()  {

	    QStringList palabrasExcluidas = AdminDB::getInstancia()->getNombres();
	    return palabrasExcluidas;
	}

	QStringList filtroEmpresas()  {

	    QStringList palabrasExcluidas = AdminDB::getInstancia()->getEmresas();
	    return palabrasExcluidas;
	}

	#endif // FILTROS_H

.. code-block:: c++	

    // En LineaDeTexto

	private:
	    QStringList (*puntero)();
		
	public:
	    void conectarPuntero( QStringList (*puntero)() )  {
	        this->puntero = puntero;
	    }
		
.. code-block:: c++	

	// En el constructor de Ventana
	ui->leNombre->conectarPuntero( &filtroNombres );
	ui->leEmpresa->conectarPuntero( &filtroEmpresas );



