# E4: Streaming de audio funcional

El proceso comenzó probando manualmente la reproducción desde la interfaz web para identificar en qué punto fallaba el streaming. Se accedió a través del navegador a `http://192.168.56.10/` y se intentó reproducir una canción, registrando el error exacto que se producía.

Se revisaron los logs de la API con `docker logs -f spotesify-api` mientras se intentaba reproducir una canción, observando el flujo de solicitudes y respuestas. Se identificó que la ruta de streaming `/api/stream/:songId` estaba siendo consultada correctamente, pero los archivos de audio no se encontraban en la ruta esperada, lo que causaba que el servidor devolviera un error 404.

Se corrigieron dos cosas: primero, se aseguró que el volumen de música estuviera correctamente mapeado en el docker-compose (como en E3), y segundo, se verificó que los nombres de los archivos en el sistema de archivos coincidieran exactamente con los registros en Redis. Se ejecutó `docker compose restart api` para aplicar los cambios, y luego se probó nuevamente el streaming desde la web. Esta vez, al reproducir una canción, el audio se transmitía correctamente sin cortes ni errores, confirmando que el streaming era funcional.
