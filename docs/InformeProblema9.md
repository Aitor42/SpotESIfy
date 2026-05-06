# E9: Arranque resiliente configurado

Se identificó que cuando los servicios se iniciaban simultáneamente, algunos fallaban porque sus dependencias aún no estaban completamente operativas. Se decidió implementar un mecanismo de arranque ordenado utilizando healthchecks y condiciones en la directiva `depends_on`.

Se configuró un healthcheck en el servicio Redis ejecutando `REDIS_CLI ping` para verificar que estaba respondiendo, y se estableció que el servicio API dependiera de Redis con la condición `condition: service_healthy`. Similarmente, se configuró que Nginx dependiera de la API con `condition: service_healthy`.

Se realizaron pruebas de arranque completo ejecutando primero `docker compose down` para detener todo, seguido de `docker compose up -d` para iniciar desde cero. Se monitoreó con `docker compose logs -f` para verificar que cada servicio esperaba a sus dependencias antes de intentar conectarse. Al revisar los logs, se confirmaba el patrón: Redis iniciaba primero (healthy), luego la API (healthy), y finalmente Nginx (started). Esto eliminó todos los errores transitorios de conexión durante el arranque.
