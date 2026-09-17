# Tarea 01
## Instalación de VPS de un CMS
### El sevidor

| **Elemento** | **Lo que dice la documentación** | **Lo que necesita la VM**| **Otros (Comentarios que consideres relevantes)**|**Fuente de info:**|
| ------------ | ------------ | ------------ | ------------ | ------------ |
| S.O    | Ubuntu Server, Debian o Windows Server.     | Ubuntu Server26.04.1      |En la documentación oficial de WordPress (wordpress.org/about/requirements), no existe un sistema operativo recomendado de forma explícita. WordPress está escrito en PHP y utiliza bases de datos MySQL/MariaDB, por lo que se puede ejecutar sobre cualquier sistema operativo que admita estas tecnologías|Prompt(Búscame en la documentación de wordpress el sistema operativo recomensado )|
| Servidor web   |  Apache o Nginx     | Apache2      |  Apache o Nginx se recomiendan como los servidores más robustos y con más funciones para ejecutar WordPress, pero cualquier servidor que admita PHP y MySQL servirá| Documentación de WordPress|
| Versión de PHP  | Versión 8.3 o superior.      | Versión 8.3     | |Documentación de WordPress|
|Gestor de BBDD  | MariaDB 10.11+ o MySQL 8.0+.     |   MySQL 8.0  | |Documentación de WordPress|
| Memoria y Disco  |  1 GB de disco y mínimo 512 MB a 1 GB de memoria RAM |  Usare 20GB para disco y 8GB de RAM   |En la documentación oficial de WordPress (wordpress.org/about/requirements y guías de administración), WordPress no especifica una cantidad rígida de disco o memoria RAM a nivel de servidor, ya que depende de los plugins, temas y archivos subidos. No obstante, sí detalla los límites mínimos de consumo e instalación. |Prompt(Buscame en la documentación oficial de wordpress la memoria y discos que necesita )|

### Instalación de WordPress
#### Conectarse por SSH
Haré una instalación remota de WordPress, para eso me conecto al servidor mediante SSH ejecutando el comando ssh.
