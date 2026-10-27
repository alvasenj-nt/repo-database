# Repositorio de Base de Datos para Alumnos

Este repositorio proporciona una configuración sencilla para levantar un entorno de base de datos MySQL utilizando Docker. A continuación se detallan los pasos para instalar las herramientas necesarias y cómo conectarse a la base de datos.

## 1. Guía de Instalación de Prerrequisitos

Antes de empezar, necesitarás instalar Git y Docker en tu sistema.

### a. Windows

#### Git
Git es un sistema de control de versiones que te permitirá descargar (clonar) este repositorio en tu máquina.
- **Descarga**: Ve a la página oficial de [Git for Windows](https://git-scm.com/download/win) y descarga el instalador.
- **Instalación**: Ejecuta el instalador y sigue los pasos. La configuración por defecto es suficiente para la mayoría de los usuarios.

#### Docker Desktop
Docker nos permitirá ejecutar la base de datos en un contenedor aislado, sin necesidad de instalar MySQL directamente en tu sistema.
- **Descarga**: Ve a la página oficial de [Docker Desktop](https://www.docker.com/products/docker-desktop/) y descarga el instalador para Windows.
- **Instalación**: Ejecuta el instalador. Docker Desktop requiere que la característica de WSL 2 (Windows Subsystem for Linux) esté activada en Windows. El propio instalador de Docker te guiará en el proceso si no lo tienes activado. Es posible que necesites reiniciar tu ordenador.

### b. macOS

#### Git
Es muy probable que ya tengas Git instalado. Puedes verificarlo abriendo la **Terminal** y escribiendo `git --version`. Si no lo está, la forma más sencilla de instalarlo es a través de Homebrew.
- **Instalar Homebrew** (si no lo tienes): Abre la Terminal y ejecuta el siguiente comando:
  ```bash
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```
- **Instalar Git**: Una vez instalado Homebrew, ejecuta:
  ```bash
  brew install git
  ```

#### Docker Desktop
- **Descarga**: Ve a la página oficial de [Docker Desktop](https://www.docker.com/products/docker-desktop/). El sitio detectará que estás en macOS y te ofrecerá el archivo correcto.
- **Instalación**: Abre el archivo `.dmg` que has descargado y arrastra el icono de Docker a tu carpeta de Aplicaciones.

### c. Ubuntu / Derivados de Debian

#### Git
La mayoría de las distribuciones de Linux vienen con Git. Si no es tu caso, puedes instalarlo fácilmente.
- **Instalación**: Abre una terminal y ejecuta los siguientes comandos para actualizar la lista de paquetes e instalar Git:
  ```bash
  sudo apt update
  sudo apt install git
  ```

#### Docker Desktop
- **Descarga**: Descarga el paquete `.deb` para Ubuntu desde la página oficial de [Docker Desktop](https://www.docker.com/products/docker-desktop/).
- **Instalación**: Navega con la terminal a la carpeta donde descargaste el archivo y ejecuta el siguiente comando. Reemplaza `<version>` y `<arch>` con el nombre del archivo descargado.
  ```bash
  sudo apt update
  sudo apt install ./docker-desktop-<version>-<arch>.deb
  ```
- Al finalizar, es posible que necesites añadir tu usuario al grupo de Docker para poder ejecutar comandos sin `sudo`:
  ```bash
  sudo usermod -aG docker $USER
  ```
  Deberás cerrar sesión y volver a iniciarla para que este cambio surta efecto.

---

## 2. Configuración del Proyecto

Una vez instaladas las herramientas, sigue estos pasos:

### a. Clonar el Repositorio
Abre una terminal (Git Bash en Windows, Terminal en macOS/Linux) y ejecuta el siguiente comando para descargar el código:
```bash
git clone https://github.com/alvasenj-nt/repo-database.git
```
*Si no tienes Git, el botón **Code > Download ZIP** de esta página descarga lo mismo; descomprímelo y abre una terminal dentro de la carpeta.*

Luego, navega a la carpeta que se acaba de crear:
```bash
cd repo-database
```

### b. Iniciar el Contenedor de la Base de Datos
Asegúrate de que Docker Desktop esté en ejecución. Luego, en la misma terminal, dentro de la carpeta del proyecto, ejecuta el siguiente comando:
```bash
docker compose up -d
```
Este comando leerá el archivo `docker-compose.yml`, descargará la imagen de MySQL (si no la tienes) y creará e iniciará el contenedor `mysql_alumnos` en segundo plano (`-d`).

La primera vez tarda uno o dos minutos: el script `init_db.sql` carga las bases de datos de ejemplo (nba, jardineria, sakila, empresa, concesionario, caltech, instituto y NorthWind). Puedes seguir el proceso con:
```bash
docker compose logs -f db
```
(`Ctrl+C` para salir). Para comprobar que el contenedor está en marcha:
```bash
docker ps
```

### c. Gestión del contenedor
Ejecuta estas órdenes dentro de la carpeta del proyecto:

| Orden | Efecto | ¿Conserva tus datos? |
|---|---|---|
| `docker compose stop` / `docker compose start` | Para y vuelve a arrancar el servidor | Sí |
| `docker compose down` | Elimina el contenedor; el volumen con los datos sigue. `docker compose up -d` lo recrea | Sí |
| `docker compose down -v` | Elimina también el volumen. El siguiente `docker compose up -d` parte de cero y vuelve a ejecutar `init_db.sql` | **No** |
| `git pull` | Descarga los cambios del repositorio. Un `init_db.sql` nuevo solo se aplica tras `docker compose down -v` | Sí |

- `init_db.sql` solo se ejecuta cuando el volumen está vacío, es decir, la primera vez o después de `docker compose down -v`.
- `docker compose down -v` es el botón de reinicio: útil si has estropeado las bases de datos de ejemplo, pero borra todo lo que hayas creado. Guarda siempre tus scripts `.sql` en tu equipo, fuera del contenedor.
- El contenedor tiene `restart: always`: arranca solo cada vez que se abre Docker Desktop.
- Si `docker compose up -d` falla con *port is already allocated*, otro programa está usando el puerto `3307`.

---

## 3. Conexión con MySQL Workbench

Ahora que la base de datos está funcionando en Docker, puedes conectarte a ella usando cualquier cliente de MySQL. Las siguientes instrucciones son para MySQL Workbench.

1.  Abre MySQL Workbench.
2.  En la pantalla de bienvenida, haz clic en el icono `+` al lado de "MySQL Connections" para crear una nueva conexión.
3.  Se abrirá una ventana para configurar los parámetros. Rellénalos con la siguiente información:
    *   **Connection Name**: Dale un nombre descriptivo (ej. `MySQL Docker Alumnos`).
    *   **Connection Method**: Deja el valor por defecto `Standard (TCP/IP)`.
    *   **Hostname**: `127.0.0.1`
    *   **Port**: `3307` (¡Importante! Usamos el puerto `3307` como se especifica en `docker-compose.yml`).
    *   **Username**: `root`
    *   **Password**: Haz clic en "Store in Vault..." (Windows) o "Store in Keychain..." (macOS) e introduce la contraseña `password123`.

4.  Puedes hacer clic en **"Test Connection"** para verificar que los datos son correctos. Deberías ver un mensaje de éxito.
5.  Haz clic en **"OK"** para guardar la conexión.

¡Listo! Ahora verás tu nueva conexión en la pantalla de bienvenida. Haz doble clic en ella para abrir el editor de SQL y empezar a trabajar con la base de datos `nba`.

### Usuario Alternativo

El script de inicialización también crea un usuario con menos privilegios. Puedes conectarte con él usando los siguientes datos:
*   **Username**: `alumno`
*   **Password**: `alumno_pass`
*   El resto de los datos (Hostname, Port) son los mismos.

El usuario `alumno` puede crear bases de datos, tablas, vistas, usuarios y roles, pero no tiene `GRANT OPTION`: para conceder privilegios con `GRANT` hay que conectarse como `root`.

### Sin Workbench: cliente de terminal

```bash
docker exec -it mysql_alumnos mysql -u root -p
```

Para ejecutar un script desde Git Bash (en Windows) o desde la terminal (en macOS y Linux):
```bash
docker exec -i mysql_alumnos mysql -uroot -ppassword123 < mi_script.sql
```

Si Workbench muestra *Can't connect to MySQL server on '127.0.0.1:3307'*, el contenedor está parado o Docker Desktop no está abierto: compruébalo con `docker ps`.