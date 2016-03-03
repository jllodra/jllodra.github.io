---
layout: post
title: Determinar si una base de datos postgresql ha cambiado
published: true
---

Hay veces en las que tenemos que plantearnos cómo hacer copias de seguridad de forma inteligente de nuestras bases de datos.

<!--more-->

Existen varias soluciones cuando se trata de Postgresql:

* Usar **pg_dump** (con formato plain, o custom)
* Usar **pg_dumpall**
* Usar PITR y wal archiving con **pg_basebackup**
* Parar el servidor y copiar el cluster

Si lo que queremos es realizar copias de seguridad para casos de catástrofes, lo más cómodo es pg\_basebackup, ya que es más rápido, menos costoso, y restaurar es muy fácil. El problema es que debemos activar el Write Ahead Log, y además no podemos restaurar selectivamente una base de datos del cluster.

Otra solución es usar pg\_dumpall, que realmente lo que hace es usar pg\_dump por debajo. Es una buena solución pero no es tan flexible como usar pg_dump directamente.

Si lo que queremos es realizar copias de seguridad completas y a la vez poder restaurar bases de datos por separado (imaginemos que un cliente borró algunos datos por equivocación y necesitamos restaurar solo la base de dados de ese cliente), la solución más idónea es utilizar **pg_dump**, y con el formato **custom**, que nos permite el restaurado con el comando pg\_restore, y también será posible convertir el *dump* a *plain sql*.

Además, si nuestro escenario consiste en muchas (cientos) de bases de datos que debemos copiar, pero realmente pocas de esas bases de datos cambian diariamente (muchos de nuestros usuarios solo realizan consultas durante el día), llegamos a la conclusión que no es necesario copiar todas las bases de datos, si no solo las bases de datos que han cambiado desde la última copia realizada.

Necesitamos saber qué bases de datos han cambiado.

> Los valores de las columnas **tup_inserted**, **tup_updated**, **tup_deleted** nos indican el número de inserciones, actualizaciones y eliminaciones totales que ha sufrido una base de datos.

	select tup_inserted,tup_updated,tup_deleted from pg_stat_database where datname = 'base_de_datos';
	 tup_inserted | tup_updated | tup_deleted
	--------------+-------------+-------------
	            0 |           0 |           0
	(1 row)

Esta base de datos no ha sufrido ningún cambio todavía.
Vamos a hacer un cambio de definición (DDL).

	CREATE TABLE DEPARTMENT(
	    ID INT PRIMARY KEY      NOT NULL,
	    DEPT           CHAR(50) NOT NULL,
	    EMP_ID         INT      NOT NULL
	 );

	select tup_inserted,tup_updated,tup_deleted from pg_stat_database where datname = 'base_de_datos';
	 tup_inserted | tup_updated | tup_deleted
	--------------+-------------+-------------
	           21 |           0 |           0
	(1 row)

El hecho de crear una tabla ha tenido como consecuencia 21 operaciones de inserción. Los cambios en datos (DML) también se reflejarían como operaciones, por supuesto.

Así pues, tenemos una forma consistente de comprobar si una base de datos ha sufrido escrituras (cambios tanto DDL como DML) desde la última vez que se consultó.

> Con esta estrategia podemos realizar copias de seguridad selectivas, copiando solo aquellas bases de datos que han cambiado, y así generando el mínimo *overhead* en nuestro sistema.

Cabe destacar que la rutina de **vacuum** (analyze) no nos afecta y no debemos preocuparnos por ello.

Nos vemos en el siguiente post...

---

Si necesitas consultoría informática profesional, no dudes en contactar con [Atlantis of Code](http://atlantisofcode.com), o escríbeme a mi correo personal.
