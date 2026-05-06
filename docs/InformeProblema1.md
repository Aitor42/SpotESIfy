# E1: Web accesible a través del proxy

Para hacer la web accesible a través del proxy de Nginx, primero se revisó la configuración de los servicios en el docker-compose para entender la arquitectura de la aplicación. Se comprobó que el archivo de configuración de Nginx hacía referencia a un upstream llamado `backend:5000`, el cual no existia. Ejecutando `docker compose logs api` se podria comprobar que salia este mensaje: `[emerg] host not found in upstream "backend:5000" in /etc/nginx/conf.d/default.conf:10`

Se identificó que el servicio correcto se llamaba `api`, por lo que se editó `nginx.conf` cambiando la referencia de `backend` a `api`. Esto era fundamental porque Nginx necesita conocer exactamente el nombre del servicio Docker para poder resolver el hostname correctamente dentro de la red interna.

Después de realizar el cambio en la configuración, se ejecutó `docker compose down` para detener todos los servicios, seguido de `docker compose up -d` para iniciarlos nuevamente y aplicar la nueva configuración. Esta secuencia completa es necesaria para que Nginx recargue su configuración y establezca las conexiones correctamente. Se verificó accediendo a través de curl: `curl http://192.168.56.10`, confirmando que la web era ahora accesible a través del proxy.
