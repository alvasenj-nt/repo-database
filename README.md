# Repositorio de Base de Datos para Alumnos

Este repositorio proporciona una configuración sencilla para levantar un entorno de base de datos MySQL utilizando Docker. A continuación se detallan los pasos para instalar las herramientas necesarias en Windows y cómo conectarse a la base de datos.

## Guía de Instalación para el Entorno Local

### Docker Desktop en Windows (con WSL2)

Docker nos permite empaquetar la aplicación y sus dependencias en "contenedores". Esto garantiza que el entorno de desarrollo sea idéntico para todos los miembros del equipo, eliminando el clásico problema de "en mi máquina sí funciona".

Esta guía asume que ya tienes WSL2 (Subsistema de Windows para Linux) instalado y configurado en tu máquina.

1.  **Desinstalar Versiones Anteriores:**
    *   Ve a "Agregar o quitar programas" en la configuración de Windows.
    *   Busca "Docker Desktop" en la lista de aplicaciones y selecciónalo.
    *   Haz clic en "Desinstalar". **Necesitarás proporcionar una contraseña de administrador** para confirmar la acción.

2.  **Instalar Docker Desktop:**
    *   Descarga el instalador oficial desde la página de Docker: [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
    *   Ejecuta el archivo descargado. La instalación también **requerirá permisos de administrador**.
    *   Sigue las instrucciones del asistente de instalación, asegurándote de que la opción para usar WSL2 esté seleccionada.

### Instalación de Git y Clonación del Proyecto

Git es un sistema de control de versiones que nos permite guardar un historial de los cambios en nuestro código. Es una herramienta esencial para colaborar en equipo, gestionar diferentes versiones del proyecto y poder volver a un estado anterior si algo sale mal.

1.  **Instalar Git:**
    *   Descarga Git para Windows desde el sitio web oficial: [https://git-scm.com/downloads](https://git-scm.com/downloads)
    *   Ejecuta el instalador. Generalmente, puedes aceptar las opciones por defecto durante la instalación.

## 1. Configuración del Proyecto

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

## 2. Conexión con MySQL Workbench

Ahora que la base de datos está funcionando en Docker, puedes conectarte a ella usando MySQL Workbench.

1.  Abre MySQL Workbench.
2.  En la pantalla de bienvenida, haz clic en el icono `+` al lado de "MySQL Connections" para crear una nueva conexión.
3.  Se abrirá una ventana para configurar los parámetros. Rellénalos con la siguiente información:
    *   **Connection Name**: Dale un nombre descriptivo (ej. `MySQL Docker Alumnos`).
    *   **Connection Method**: Deja el valor por defecto `Standard (TCP/IP)`.
    *   **Hostname**: `127.0.0.1`
    *   **Port**: `3307` (¡Importante! Usamos el puerto `3307` como se especifica en el `docker-compose.yml` para no entrar en conflicto con posibles instalaciones locales de MySQL).
    *   **Username**: `root`
    *   **Password**: Introduce la contraseña `password123`.

4.  Puedes hacer clic en **"Test Connection"** para verificar que los datos son correctos. Deberías ver un mensaje de éxito.
5.  Haz clic en **"OK"** para guardar la conexión.

¡Listo! Ahora verás tu nueva conexión en la pantalla de bienvenida. Haz doble clic en ella para abrir el editor de SQL y empezar a trabajar con la base de datos `nba` que ha sido creada por el script de inicialización.

### Usuario Alternativo

El script de inicialización también crea un usuario con menos privilegios llamado `alumno`. Si deseas conectarte con este usuario, puedes crear otra conexión en MySQL Workbench con los siguientes datos:
*   **Username**: `alumno`
*   **Password**: `alumno_pass`
*   El resto de los datos (Hostname, Port) son los mismos.
