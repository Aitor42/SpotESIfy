# E5: Contenedor API estable (0 reinicios)

El primer síntoma observado fue que el contenedor API se reiniciaba continuamente con el eror 137. Se ejecutó `docker ps` para confirmar que el contenedor estaba siendo recreado constantemente, y luego se consultaron los logs del sistema con `dmesg` para obtener información del kernel.

En los logs de dmesg aparecía el mensaje `Memory cgroup out of memory: Killed process 32489 (npm start)` junto con un mensaje de oom-kill y el id de un contenedor, para confirmar que era el contenedor de api use `docker inspect d10d8fa406d82d798e007a550418e7b6760188d0ae54406a0cd12559703d6b01` y vi dos cosas "Name": "/spotesify-api" y "OOMKilled": true, indicando que el kernel estaba terminando el proceso por falta de memoria.

La solución fue editar el docker-compose.yml y eliminar el límite de memoria del servicio API, permitiendo que el contenedor utilizara toda la memoria disponible del host. Se ejecutó `docker compose up -d` para aplicar estos cambios.
