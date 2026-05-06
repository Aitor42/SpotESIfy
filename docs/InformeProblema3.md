# E3: Catálogo de canciones accesible (6 canciones)

Se comenzó verificando qué rutas de almacenamiento estaban disponibles en el contenedor API en la direcion que se indicaba en el docker-compose ejecutando `docker exec -it spotesify-api ls -la /music`, lo que reveló que ese directorio estaba vacío. Se decidió entonces explorar otra ruta posible que salia en el docker-compose en el aprtado de db, encontrando que las canciones estaban en `/data/music`.

La razón de esta discrepancia era que el docker-compose.yml original no estaba mapeando correctamente los volúmenes. Se editó el archivo para asegurar que el volumen de almacenamiento de canciones apuntara a la ruta correcta `/data/music`.

Tras actualizar la configuración, se ejecutó `docker compose up -d` para aplicar los cambios, pero se notó que algunos nombres de canciones en la base de datos no coincidían con los archivos reales. Por esto, se realizó un re-seed completo de la base de datos ejecutando `docker compose restart api`, que reinicializó todos los datos y sincronizó correctamente las 6 canciones.
