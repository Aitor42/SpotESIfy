# E2: API conectada a la base de datos

El primer paso fue revisar los logs del contenedor API mediante `docker logs spotesify-api` para identificar el problema de conectividad. Se encontró el error `NOAUTH Authentication required`, indicando que Redis requería credenciales de autenticación que no estaban siendo proporcionadas.

Para encontrar la contraseña correcta, se buscó en los archivos del proyecto y se encontró en el script `seed_redis.sh` el valor `spotesify123`. Se editaron entonces tanto `server.js` como `seed.js` para modifica la variable `REDIS_PASS` añadiendo como valor por defecto `spotesify123` dejando la línea de la siguiente manera: `const REDIS_PASS = process.env.REDIS_PASS || 'spotesify123';`

Además, se verificó que ambos servicios estuvieran en la misma red Docker ejecutando `docker inspect spotesify-api` y comprobando el apartado de redes. Con la configuración actualizada, se ejecutó `docker compose up -d --build` para reconstruir los contenedores, y los logs confirmaron la conexión exitosa: `[Redis] Connected`.
