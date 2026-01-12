# Repositorio de Base de Datos para Alumnos

Este repositorio proporciona una configuración sencilla para levantar un entorno de base de datos MySQL utilizando Docker. A continuación se detallan los pasos para instalar las herramientas necesarias en Windows y cómo conectarse a la base de datos.

## 1. Prerrequisitos de Software (Instalación en Windows)

Antes de empezar, necesitarás instalar las siguientes herramientas en tu sistema.

### a. Git
Git es un sistema de control de versiones que te permitirá descargar (clonar) este repositorio en tu máquina.
- **Descarga**: Ve a la página oficial de [Git for Windows](https://git-scm.com/download/win) y descarga el instalador.
- **Instalación**: Ejecuta el instalador y sigue los pasos. La configuración por defecto es suficiente para la mayoría de los usuarios.

### b. Docker Desktop
Docker nos permitirá ejecutar la base de datos en un contenedor aislado, sin necesidad de instalar MySQL directamente en tu sistema.
- **Descarga**: Ve a la página oficial de [Docker Desktop](https://www.docker.com/products/docker-desktop/) y descarga el instalador para Windows.
- **Instalación**: Ejecuta el instalador. Docker Desktop requiere que la característica de WSL 2 (Windows Subsystem for Linux) esté activada en Windows. El propio instalador de Docker te guiará en el proceso si no lo tienes activado. Es posible que necesites reiniciar tu ordenador.

### c. MySQL Workbench
MySQL Workbench es una herramienta visual que te permitirá conectarte a la base de datos, explorar su estructura y ejecutar consultas SQL.
- **Descarga**: Ve a la página de descargas de [MySQL Workbench Community](httpss://dev.mysql.com/downloads/workbench/) y selecciona la versión para Microsoft Windows.
- **Instalación**: Ejecuta el instalador y sigue los pasos de la instalación.

## 2. Configuración del Proyecto

Una vez instaladas las herramientas, sigue estos pasos:

### a. Clonar el Repositorio
Abre una terminal (puedes buscar "Git Bash" que se instaló con Git, o usar "PowerShell" o "Símbolo del sistema") y ejecuta el siguiente comando para descargar el código del repositorio:
```bash
git clone <URL_DEL_REPOSITORIO>
```
*Nota: Reemplaza `<URL_DEL_REPOSITORIO>` con la URL real de este repositorio.*

Luego, navega a la carpeta que se acaba de crear:
```bash
cd repo-database
```

### b. Iniciar el Contenedor de la Base de Datos
Asegúrate de que Docker Desktop esté en ejecución. Luego, en la misma terminal, dentro de la carpeta del proyecto, ejecuta el siguiente comando:
```bash
docker-compose up -d
```
Este comando leerá el archivo `docker-compose.yml`, descargará la imagen de MySQL (si no la tienes) y creará e iniciará el contenedor en segundo plano (`-d`).

## 3. Conexión con MySQL Workbench

Ahora que la base de datos está funcionando en Docker, puedes conectarte a ella usando MySQL Workbench.

1.  Abre MySQL Workbench.
2.  En la pantalla de bienvenida, haz clic en el icono `+` al lado de "MySQL Connections" para crear una nueva conexión.
3.  Se abrirá una ventana para configurar los parámetros. Rellénalos con la siguiente información:
    *   **Connection Name**: Dale un nombre descriptivo (ej. `MySQL Docker Alumnos`).
    *   **Connection Method**: Deja el valor por defecto `Standard (TCP/IP)`.
    *   **Hostname**: `127.0.0.1`
    *   **Port**: `3307` (¡Importante! Usamos el puerto `3307` como se especifica en el `docker-compose.yml` para no entrar en conflicto con posibles instalaciones locales de MySQL).
    *   **Username**: `root`
    *   **Password**: Haz clic en "Store in Keychain..." e introduce la contraseña `password123`.

    ![Configuración de MySQL Workbench](httpss://i.imgur.com/your-image-url.png)

4.  Puedes hacer clic en **"Test Connection"** para verificar que los datos son correctos. Deberías ver un mensaje de éxito.
5.  Haz clic en **"OK"** para guardar la conexión.

¡Listo! Ahora verás tu nueva conexión en la pantalla de bienvenida. Haz doble clic en ella para abrir el editor de SQL y empezar a trabajar con la base de datos `nba` que ha sido creada por el script de inicialización.

### Usuario Alternativo

El script de inicialización también crea un usuario con menos privilegios llamado `alumno`. Si deseas conectarte con este usuario, puedes crear otra conexión en MySQL Workbench con los siguientes datos:
*   **Username**: `alumno`
*   **Password**: `alumno_pass`
*   El resto de los datos (Hostname, Port) son los mismos.
