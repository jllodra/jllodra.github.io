---
layout: post
title: Postgresql y muchas bases de datos
published: true
---

PostgreSQL se compone de varios procesos, entre ellos el recolector de estadísticas, y el autovacuum.

**Información general**

El [recolector de estadísticas](http://www.postgresql.org/docs/devel/static/monitoring-stats.html) se encarga de recopilar datos acerca de la actividad del servidor. El [autovacuum daemon](http://www.postgresql.org/docs/devel/static/routine-vacuuming.html) es un proceso que según los datos recogidos por el recolector, optimizará y limpiará nuestras bases de datos. El autovacuum es una funcionalidad muy importante y necesaria para el mantenimiento de nuestras bases de datos.

<!--more-->

**¿Cuántas bases de datos puede soportar PostgreSQL?**

Técnicamente muchas, dependerá de factores como nuestro espacio en disco, el sistema de ficheros (el número de inodos que soporte, o archivos por directorio, etc), y nuestro hardware.

Pero tener muchas bases de datos tiene un gran efecto sobre el recolector de estadísticas, y es que va a trabajar más si tenemos muchas bases de datos. Además, nuestro autovacuumer deberá analizar y limpiar más bases de datos.

Si tenemos bases de datos algo complejas (~50 relaciones o más), el trabajo del recolector no es trivial, llegando a realizar un thoughput de decenas de Megabytes por segundo hacia nuestro disco duro (se puede comprobar con el comando iotop o atop, por ejemplo). Esto ocasionará utilizaciones muy altas en nuestro sistema y su bloqueo por operaciones de E/S.

**Solución a la elevada carga de E/S**

Desactivar el recolector de estadísticas, y en consecuencia el autovacuum: Así dejaremos automáticamente de producir estadísticas y por lo tanto no habrá ni lectura ni escritura a disco por parte del recolector de estadísticas. Problema: Tendremos que hacer autovacuum manual, algo no deseable.

Y otra solución, crear un ramdisk (algo así como un disco duro en memoria), y dejar que el recolector de estadísticas trabaje sobre ese archivo que hemos creado y añadido a nuestro /etc/fstab. Así liberaremos nuestro HDD de cualquier actividad relacionada con la recolección de estadísticas, además, como la memoria ram es volátil, postgreSQL siempre vuelca las estadísticas a disco cuando el servidor se detiene. Problema resuelto.

---

Si necesitáis ayuda en algunos de estos temas, no dudéis en contactar con [Atlantis of Code](http://atlantisofcode.com), o escribidme a mi correo personal.
