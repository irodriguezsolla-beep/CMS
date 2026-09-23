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
![Conexión SSH](fotos/Captura%20desde%202026-09-23%2013-05-18.png)

#### 2.1.2. Estado del servidor activo
![Servidor activo](fotos/Captura%20desde%202026-09-23%2013-13-43.png)

---

### 2.2. Actualización del sistema

Una vez establecida la conexión, ejecutaré un `sudo apt update` para actualizar la lista de paquetes del sistema.
![](fotos/Captura%20desde%202026-09-23%2013-15-53.png)








