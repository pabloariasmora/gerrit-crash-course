Guia rápida para instalar Gerrit en Linux (Canonical, Ubuntu, 20.04 LTS, Focal Fossa) 
=

Introducción
======
Este contenido explica cómo instalar una instancia básica de Gerrit en una máquina Linux.

Requerimientos
======

* Un servidor con el sistema operativo Ubuntu, 20.04 LTS, Focal Fossa previamente instalado.
* Asegurar conectividad de los puertos 22, 80, 8080 y 29418.

Pasos por seguir
======

1- Iniciar sesión dentro del servidor. Para esto abra una ventana de terminal.
2- Actualizar los repositorios de Paquetes dentro de Ubuntu.

```
sudo apt-get update && sudo apt-get upgrade -y
```

3- Instalar Java SE Runtime Environment (versión 11 o superior). Actualmente versión 11.0.14 2022-01-18.

```
sudo apt install default-jre -y
```

4- Configuramos una variable de ambiente que contiene la versión actual a instalar. Actualmente versión 3.1.3

```
export GERRIT_VERSION=3.1.3
```

5- Configuramos una variable de ambiente que contiene el directorio de instalación.

```
export GERRIT_SITE=~/gerrit_testsite
```

6- Descargamos el archivo .war de Gerrit. Este es un archivo utilizado para distribuir la colección de archivos JAR necesarios para su ejecución.

```
wget https://gerrit-releases.storage.googleapis.com/gerrit-$GERRIT_VERSION.war
```

7 - Instalamos e inicializamos Gerrit
```
java -jar gerrit-$GERRIT_VERSION.war init --batch --dev -d $GERRIT_SITE --install-plugin download-commands
```

Este comando toma tres parámetros:

`--batch` asigna valores predeterminados a varias opciones de configuración de Gerrit. En este punto es para poder configurar inicialmente nuestro ambiente.

`--dev` configura el servidor Gerrit para usar la opción de autenticación, DEVELOPMENT_BECOME_ANY_ACCOUNT, que le permite cambiar entre diferentes usuarios para explorar cómo funciona Gerrit. 

`--install-plugin` instala el complementos en este caso el  `download-commands`, que muestra de manera útil los comandos de descarga de git. No está activado de forma predeterminada en las versiones recientes de Gerrit.




