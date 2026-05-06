# E8: Worker estable (0 reinicios)

Al revisar los logs del contenedor worker, se observó que también se reiniciaba continuamente, presentando síntomas similares a los del contenedor API. Se ejecutó `docker logs spotesify-worker` para examinar los errores, y se identificó que en algún momento el contenedor agotaba la memoria disponible.

Se investigó el código de `worker.js` para entender qué estaba consumiendo tanta memoria. Se encontró que existía un array `metricsHistory` que acumulaba registros de métricas indefinidamente sin nunca limpiarlos. Se tomó la decisión de implementar un mecanismo de circular buffer limitando el tamaño máximo del array a 50 registros con `const MAX_HISTORY_SIZE = 50`. Se agregó entonces la lógica `if (metricsHistory.length > MAX_HISTORY_SIZE) { metricsHistory.shift(); }` para eliminar automáticamente los registros más antiguos cuando se alcanzaba el límite. Ademas de eliminar el limite de memoria del archivo docker-compose para evitar reinicios.

Se reconstruyó el contenedor con `docker compose up -d --build`. El consumo de memoria se estabilizó en un nivel constante, y al ejecutar `docker ps` se confirmó que el contenedor ya no se reiniciaba espontáneamente.
