Guia para instalar Gerrit Sandbox en Linux (Canonical, Ubuntu, 20.04 LTS, Focal Fossa) 
=

Introducción
======
Montaremos una instalación reducida de Gerrit sandbox con las siguientes características:
- Algoritmos de seguridad predeterminados (abordaremos seguridad más adelante)
- HTTP estándar sin SSL
- Registro de usuarios locales sin validación de contraseña (solo para sandboxes)

Requerimientos
======

* Un servidor con el sistema operativo Ubuntu, 20.04 LTS, Focal Fossa previamente instalado.
* Asegurar conectividad de los puertos 22, 80, 8080 y 29418.

Descarga 
======

1- Iniciar sesión dentro del servidor. Para esto abra una ventana de terminal.

2- Elevamos permisos para convertirnos en root.

```
sudo su
```

2- Actualizar los repositorios de Paquetes dentro de Ubuntu.

```
apt-get update && sudo apt-get upgrade -y
```

3- Instalar Java SE Runtime Environment (versión 11 o superior). Actualmente versión 11.0.14 2022-01-18. Gerrit aún no es compatible con Java 13 o posterior en este momento.

```
apt install default-jre -y
```

4- Configuramos una variable de ambiente que contiene la versión actual a instalar. Actualmente versión 3.1.3

```
export GERRIT_VERSION=3.1.3
```

5 - Creamos una carpeta para contener los archivos de configuración de Gerrit. Y a su vez descargamos el archivo .war de Gerrit. Este es un archivo utilizado para distribuir la colección de archivos JAR necesarios para su ejecución.

```
mkdir -p /opt/gerrit
cd /opt/gerrit
wget https://gerrit-releases.storage.googleapis.com/gerrit-$GERRIT_VERSION.war
```
Ejecutando la configuración inicial de Gerrit
=====

1- Antes de comenzar la configuración de Gerrit, debemos crear un usuario dedicado para él (por ejemplo, Gerrit), para permitir que el servicio funcione en su propia cuenta y no en la cuenta del administrador del sistema (root). Para mayor comodidad, asignamos /opt/gerrit como directorio de inicio:

```
useradd -s /bin/bash -d /opt/gerrit -m -G sudo gerrit
chown -R gerrit:gerrit /opt/gerrit
su - gerrit
```

2- Una vez tomamos la identidad del usuario Gerrit. Iniciaremos con la configuración del sistema.

```
cd /opt/gerrit
export GERRIT_VERSION=3.1.3
java -jar gerrit-$GERRIT_VERSION.war init
```

# TODO - Agregar salidas

Start/Stop Daemon, Inicio en reinicio
====

1- Para controlar el daemon Gerrit Code Review que se ejecuta en segundo plano, use el script de inicio de estilo rc.d creado por 'init':

```
   /opt/gerrit/bin/gerrit.sh start
   /opt/gerrit/bin/gerrit.sh stop
   /opt/gerrit/bin/gerrit.sh restart
```

2- Configure el daemon para que se inicie y detenga automáticamente con el sistema operativo. 

```
vi /opt/gerrit//bin/gerrit.sh
```

3- Descomente las siguientes 3 líneas en el script `/opt/gerrit//bin/gerrit.sh`:

```
chkconfig: 3 99 99
description: Gerrit Code Review
processname: gerrit
```

4- Usando root, vincule el script gerrit.sh a rc3.d:

```
  sudo ln -snf /opt/gerrit/bin/gerrit.sh /etc/init.d/gerrit
  sudo ln -snf /etc/init.d/gerrit /etc/rc3.d/S90gerrit
```

('Opcional') Configure el daemon para que se inicie y detenga automáticamente con el sistema operativo.
