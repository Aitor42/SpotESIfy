# E6: Imagen optimizada (50MB, user: appuser)

Se realizó la decisión de cambiar la imagen base de `node:20` a `node:20-alpine` porque alpine es una distribución Linux minimal que reduce significativamente el tamaño de la imagen. Se editó el Dockerfile para crear un usuario no-root ejecutando `RUN addgroup -S appgroup && adduser -S appuser -G appgroup`, y cambiar a ese usuario con `USER appuser`.

Se reconstruyó la imagen con `docker compose build --no-cache api` y se ejecutó `docker inspect spotesify-api` para verificar que el contenedor se ejecutaba bajo el usuario `appuser` en lugar de root, cumpliendo así los requisitos de seguridad y optimización.
