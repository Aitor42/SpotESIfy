# E7: Healthcheck operativo

Se comprobar porque el HealthCheck devolvia que estaba unhealthy se uso `docker inspect --format='{{json .State.Health}}' spotesify-api`, lo que reveló el error: `/bin/sh: curl: not found`. La imagen alpine no incluye curl por defecto, por lo que se agregó la línea `RUN apk add --no-cache curl` al Dockerfile para instalar la herramienta necesaria.

Se reconstruyó la imagen con `docker compose build api` y se ejecutó nuevamente `docker inspect --format='{{json .State.Health}}' spotesify-api` para verificar el estado. Esta vez, la salida mostraba `"Status":"healthy"`, confirmando que el healthcheck se ejecutaba exitosamente.
