# Vaultwarden -- Contenedor Docker

Este repositorio contiene la configuración necesaria para desplegar
**Vaultwarden**, una implementación ligera y autoalojada del servidor de
Bitwarden, utilizando **Docker Compose**.

Vaultwarden es ideal para entornos personales o empresariales que desean
gestionar contraseñas de forma segura sin depender de servicios
externos.

------------------------------------------------------------------------

## 🚀 Características

-   Basado en la imagen oficial: `vaultwarden/server:latest`
-   Ligero y eficiente, ideal para servidores modestos
-   Compatible con clientes Bitwarden oficiales
-   WebSockets habilitados para sincronización en tiempo real
-   Control de registro de usuarios
-   Panel de administración protegido mediante token Argon2id
-   Datos persistentes en volumen local

------------------------------------------------------------------------

## 📁 Estructura de archivos

    .
    ├── compose.yml
    └── vaultwarden/

-   `vaultwarden/` → Contiene base de datos, configuraciones y adjuntos.

> **Importante:** asegúrate de tener permisos sobre este directorio.

------------------------------------------------------------------------

## 🐳 docker-compose.yml

``` yaml
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    volumes:
      - ./vaultwarden:/data
    ports:
      - "8200:80"
    restart: unless-stopped
    environment:
      ADMIN_TOKEN: "$$argon2id$$v=19$$m=65540,t=3,p=4$$b0e6fMiTokenSegurotcMWZ/Kn8sMiTokenSeguroUH5xWQ$MiTokenSeguro$E6ixULavY398lD/uc5Xw7pC+EMSMiTokenSegurof4rfU"
      SIGNUPS_ALLOWED: "false"
      WEBSOCKET_ENABLED: "true"
      DOMAIN: "http://localhost:8200"
```

------------------------------------------------------------------------

## 🔧 Variables de entorno

  -----------------------------------------------------------------------
  Variable                      Descripción
  ----------------------------- -----------------------------------------
  `ADMIN_TOKEN`                 Token Argon2id hash utilizado para
                                acceder al panel admin.

  `SIGNUPS_ALLOWED`             Habilita o bloquea Registro de usuarios.

  `WEBSOCKET_ENABLED`           Activa sincronización en tiempo real.

  `DOMAIN`                      URL pública donde se servirá Vaultwarden.
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 🔐 Generar un token Argon2id

Para generar un hash seguro para tu panel de administración:

``` bash
docker run --rm -it vaultwarden/server /vaultwarden hash
```

Ejemplo de salida:

    Token: MiTokenSeguro123
    Hash: $argon2id$v=19$m=65540,t=3,p=4$xxxxx...

> Guarda el **token original** en un gestor de contraseñas.\
> El **hash** es lo que debes poner en `ADMIN_TOKEN`.

------------------------------------------------------------------------

## 🌐 Acceso a Vaultwarden

Interfaz principal:

    http://TU-IP:8200

Panel de administración:

    http://TU-IP:8200/admin

------------------------------------------------------------------------

## ▶️ Puesta en marcha

``` bash
docker compose up -d
```

Ver logs:

``` bash
docker logs -f vaultwarden
```

------------------------------------------------------------------------

## 🛑 Detener el contenedor

``` bash
docker compose down
```

------------------------------------------------------------------------

## 🔄 Actualizar Vaultwarden

``` bash
docker compose pull
docker compose up -d
```

------------------------------------------------------------------------

## 📦 Copias de seguridad recomendadas

Realiza backup del directorio `vaultwarden/`, especialmente:

-   `db.sqlite3` → Base de datos
-   `rsa_key.pem` y `rsa_key.pub` → Claves de cifrado
-   `attachments/` → Adjuntos cifrados

------------------------------------------------------------------------

