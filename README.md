# Tarea 01: Instalación de un CMS en un VPS

## 1. ESPECIFICACIONES Y REQUISITOS DEL SERVIDOR

| Elemento | Lo que dice la documentación | Lo que necesita la VM | Otros (Comentarios relevantes) | Fuente de información |
| :--- | :--- | :--- | :--- | :--- |
| **S. O.** | Ubuntu Server, Debian o Windows Server. | Ubuntu Server 26.04.1 | En la [documentación oficial de WordPress](https://wordpress.org/about/requirements/) no existe un sistema operativo recomendado de forma explícita. Al estar escrito en PHP y utilizar bases de datos MySQL/MariaDB, se puede ejecutar sobre cualquier S. O. que admita estas tecnologías. | Consultas en la documentación de WordPress |
| **Servidor web** | Apache o Nginx | Apache2 | Se recomiendan Apache o Nginx por ser los más robustos y con más funciones para ejecutar WordPress, pero cualquier servidor que admita PHP y MySQL funcionará. | Documentación de WordPress |
| **Versión de PHP** | Versión 8.3 o superior | Versión 8.3 | — | Documentación de WordPress |
| **Gestor de BBDD** | MariaDB 10.11+ o MySQL 8.0+ | MySQL 8.0 | — | Documentación de WordPress |
| **Memoria y Disco** | 1 GB de disco y mínimo 512 MB a 1 GB de memoria RAM | 20 GB de disco y 8 GB de RAM | WordPress no especifica una cantidad rígida de disco o memoria RAM a nivel de servidor, ya que depende de los plugins, temas y archivos subidos. No obstante, sí detalla los límites mínimos de consumo e instalación. | Consultas en la documentación oficial de WordPress |

---

## 2. PROCEDIMIENTO DE INSTALACIÓN DE WORDPRESS

### 2.1. Conexión por SSH

Para realizar la instalación remota de WordPress, me conecto al servidor mediante **SSH** ejecutando el comando correspondiente en la terminal.

#### 2.1.1. Conexión SSH desde el servidor

![Conexión SSH al servidor](fotos/Captura%20desde%202026-09-23%2013-05-18.png)

#### 2.1.2. Estado del servidor activo

![Estado del servidor activo](fotos/Captura%20desde%202026-09-23%2013-13-43.png)

---

### 2.2. Actualización del sistema

Una vez establecida la conexión, ejecutaré un `sudo apt update` para actualizar la lista de paquetes del sistema.

![Actualización de la lista de paquetes](fotos/Captura%20desde%202026-09-23%2013-15-53.png)

### 2.3. Instalar Apache2

Una vez actualizado el sistema instalaremos la dependencia de apache2.

### 2.4. Instalar WordPress

Voy a usar la versión de WordPress.org en lugar del paquete APT de los archivos de Ubuntu, ya que este es el método recomendado por la propia comunidad de WordPress. De esta forma evitaré problemas inesperados que los voluntarios de soporte no podrían prever ni solucionar.

Para ello, primero crearé el directorio del servidor con `sudo mkdir -p /srv/www`, asignaré su propiedad al usuario del servidor web mediante `sudo chown www-data: /srv/www` y, finalmente, descargaré y descomprimiré la última versión oficial directamente en la carpeta usando `curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www`.

![Descarga e instalación de WordPress](fotos/Captura%20desde%202026-09-23%2013-18-09.png)

### 2.5. Configurar el sitio en Apache2

Una vez descargado WordPress, voy a crear el VirtualHost para que Apache sepa dónde buscar los archivos y cómo gestionar las peticiones del sitio.

#### 2.5.1. Crear el archivo de configuración del VirtualHost

Crearé el archivo de configuración en `/etc/apache2/sites-available/wordpress.conf` añadiendo la siguiente estructura:

![Configuración del VirtualHost de WordPress](fotos/Captura%20desde%202026-09-23%2013-36-32.png)

Con esto defino la ruta raíz en `/srv/www/wordpress`, indico que `index.php` será el archivo principal y doy permisos de lectura y acceso al directorio del contenido (`wp-content`). Además, la directiva `AllowOverride` permitirá que WordPress gestione sus propias reglas mediante archivos `.htaccess`.

#### 2.5.2. Habilitar el sitio y módulos necesarios

Para poner en marcha la configuración, habilitaré el VirtualHost de WordPress y el módulo de reescritura de URL, que es fundamental para que funcionen los enlaces permanentes en WordPress. Habilito el nuevo sitio de WordPress con `sudo a2ensite wordpress`, activo el módulo de reescritura de URLs con `sudo a2enmod rewrite`, desactivo el sitio predeterminado de Apache con `sudo a2dissite 000-default` y, por último, recargaré el servicio de Apache2 para aplicar toda la nueva configuración sin interrumpir el servidor con `sudo service apache2 reload`.

![Habilitación del sitio WordPress y módulo rewrite](fotos/Captura%20desde%202026-09-23%2013-37-47.png)

![Desactivación del sitio predeterminado y recarga de Apache](fotos/Captura%20desde%202026-09-23%2013-38-48.png)

#### 2.5.3. Verificar la correcta aplicación

Para confirmar que todo quedó activo de forma correcta, ejecuto `sudo systemctl status apache2`.

![Estado del servicio Apache2](fotos/Captura%20desde%202026-09-23%2013-40-04.png)

### 2.6. Instalar MySQL

Instalé el servicio de base de datos ejecutando `sudo apt install mysql-server -y`.

![Instalación de MySQL](fotos/Captura%20desde%202026-09-23%2013-41-51.png)

Después me aseguro de que el servicio se inicie automáticamente con el sistema con el comando `sudo systemctl enable --now mysql` y compruebo que la base de datos está corriendo con `sudo systemctl status mysql`.

![Comprobación del servicio MySQL](fotos/Captura%20desde%202026-09-23%2013-42-56.png)

### 2.7. Crear la base de datos para WordPress

Con la parte del servidor web lista, el siguiente paso es preparar la base de datos donde WordPress guardará toda la información del sitio (entradas, páginas, usuarios y ajustes).

#### 2.7.1. Acceder a la consola de MySQL

Para empezar, me conecto a la consola de administración de MySQL ejecutando el siguiente comando en la terminal: `sudo mysql`.

![Acceso a la consola de MySQL](fotos/Captura%20desde%202026-09-23%2013-43-36.png)

#### 2.7.2. Configuración de la base de datos y usuario

Una vez dentro de la consola de MySQL, voy a ejecutar en orden las siguientes sentencias para crear la base de datos, definir el usuario y asignarle los permisos correspondientes:

- Crear la base de datos de WordPress:

  Defino la codificación `utf8mb4` para asegurar la compatibilidad total con cualquier carácter o emoji.

![Creación de la base de datos WordPress](fotos/Captura%20desde%202026-09-23%2013-44-15.png)

- Crear el usuario de la base de datos:

  Creo el `usuario wordpress` asignándole su contraseña correspondiente.

![Creación del usuario de WordPress](fotos/Captura%20desde%202026-09-23%2013-45-22.png)

- Asignar privilegios:

  Le otorgo permisos totales sobre la base de datos `wordpress` que acabo de generar.

![Asignación de privilegios al usuario de WordPress](fotos/Captura%20desde%202026-09-23%2013-47-02.png)

- Aplicar cambios y salir:

  Recargo las tablas de privilegios para que MySQL reconozca los nuevos permisos y cierro la sesión.

- ATENCIÓN ERROR:

  En las versiones más recientes de MySQL (como MySQL 8.0+), el plugin `mysql_native_password` puede venir desactivado o dar problemas de sintaxis según la configuración del servidor, ya que MySQL utiliza por defecto `caching_sha2_password`. Si salta este error, la forma correcta de crear el usuario sin forzar el plugin nativo antiguo es usar la sintaxis estándar:

![Error con mysql_native_password](fotos/Captura%20desde%202026-09-23%2013-46-24.png)

### 2.8. Configurar el archivo wp-config.php

Con la base de datos lista, me toca enlazar WordPress con MySQL y añadir las claves de seguridad para dejar la instalación a punto.

#### 2.8.1. Crear el archivo de configuración desde la plantilla

WordPress incluye una plantilla de ejemplo llamada `wp-config-sample.php`. Lo primero que hago es sacar una copia y renombrarla a `wp-config.php`, asegurándome de usar el usuario `www-data` para mantener los permisos correctos sobre los archivos:

![Creación del archivo wp-config.php](fotos/Captura%20desde%202026-09-23%2013-52-45.png)

#### 2.8.2. Definir las credenciales de la base de datos

Para no editar el archivo a mano y tardar menos, uso el comando `sed` para reemplazar directamente el nombre de la base de datos, el usuario y la contraseña dentro de `wp-config.php`.

![Configuración de las credenciales de la base de datos](fotos/Captura%20desde%202026-09-23%2013-53-11.png)

#### 2.8.3. Añadir las claves secretas (SALT keys) de seguridad

Para proteger las cookies del sitio y evitar ataques por "secretos conocidos", me toca cambiar las frases de seguridad por defecto por unas claves totalmente aleatorias.

- Abro el archivo de configuración con el editor nano y busco el bloque de líneas que contiene lo siguiente:

```bash
    define( 'AUTH_KEY',         'put your unique phrase here' );
    define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
    define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
    define( 'NONCE_KEY',        'put your unique phrase here' );
    define( 'AUTH_SALT',        'put your unique phrase here' );
    define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
    define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
    define( 'NONCE_SALT',       'put your unique phrase here' );
```
- Borro esas 8 líneas pulsando la combinación de teclas `Ctrl + K` encima de cada una, genero un conjunto nuevo de claves aleatorias entrando en la API oficial de WordPress: https://api.wordpress.org/secret-key/1.1/salt/ y copio el código devuelto por el generador, lo pego en el archivo sustituyendo las líneas borradas, guardo con `Ctrl + O` y cierro el editor con `Ctrl + X`.

![Configuración de las claves SALT de WordPress](fotos/Captura%20desde%202026-09-23%2013-58-27.png)

### Ahora abrimos desde el navegador

Yo cuando abro desde el navegador me salta el siguiente error.
![Captura desde 2026-09-23 14-07-13.png](fotos/Captura%20desde%202026-09-23%2014-07-13.png)
El error "Error establishing a database connection" significa que WordPress no puede conectarse a MySQL con la información cargada en el archivo `/srv/www/wordpress/wp-config.php`.

#### Cómo se arregla

Abro el archivo de configuración `sudo nano /srv/www/wordpress/wp-config.php` y me fijo si las siguientes líneas coinciden exactamente con lo que hice en MySQL.

```bash
    define( 'DB_NAME', 'wordpress' );
    define( 'DB_USER', 'wordpress' );
    define( 'DB_PASSWORD', 'tu_contraseña_real' );
    define( 'DB_HOST', 'localhost' );
```
En mi caso, el `define( 'DB_USER', 'wordpress' );` ponía `wordpress` en vez de `isaac`, por eso me daba un error.
![Error de conexión con la base de datos de WordPress](fotos/Captura%20desde%202026-09-23%2014-09-24.png)
![Captura desde 2026-09-23 14-11-20.png](fotos/Captura%20desde%202026-09-23%2014-11-20.png)