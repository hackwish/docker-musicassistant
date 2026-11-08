# Music Assistant Server

Configuración de Docker Compose para el servidor Music Assistant, basada en la [instalación oficial con Docker](https://www.music-assistant.io/installation/#docker-image) y en la imagen usada por `k8s-music-assistant-server`.

## Uso

Desde esta carpeta, inicia el servidor con `docker compose up -d`. La configuración persiste los datos en `./data` y usa el canal estable `latest`.

La interfaz web queda disponible en `http://IP_DEL_HOST:8095`. Music Assistant también usa el puerto TCP 8097 por defecto para audio; en modo host no se declaran `ports`, y el servidor puede abrir puertos adicionales según los reproductores y protocolos usados.

Para ver los registros: `docker compose logs -f music-assistant-server`.

## Red y biblioteca musical

`network_mode: host` permite el descubrimiento multicast (mDNS/uPnP) y la comunicación directa con los reproductores. Debe ejecutarse en un host Linux conectado a la misma red local plana que Home Assistant y los reproductores. El modo host en Docker Desktop para macOS no proporciona el acceso de capa 2 requerido por Music Assistant.

Para agregar música local, monta el directorio en `docker-compose.yml` en `/media` y, de preferencia, en solo lectura. Si la música está en NAS, móntala primero en el host y expón esa ruta al contenedor. No se agregaron `SYS_ADMIN`, `DAC_READ_SEARCH` ni `apparmor:unconfined`: la guía oficial los reserva para montar recursos SMB/NFS desde dentro del contenedor, que es menos seguro.

`LOG_LEVEL` admite `critical`, `error`, `warning`, `info` y `debug`; el valor predeterminado es `info`. `TZ` usa `America/Santiago` de forma predeterminada.
