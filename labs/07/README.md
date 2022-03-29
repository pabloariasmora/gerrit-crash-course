# Dar permisos de Administrador a usuarios de Launchpad (Open_SSO)


1- Detener el servicio de Gerrit como root

```
service gerrit stop
```

2- Ejecutar nuevamente la configuración inicial de Gerrit, utilizando el usuario gerrit

```
cd /opt/gerrit
export GERRIT_VERSION=3.1.3
java -jar gerrit-$GERRIT_VERSION.war init
```

Mantener los valores por defecto excepto el Authentication method.

```
[...]
*** User Authentication
***
Authentication method [OPENSSO/?]: development_become_any_account  RETURN
[...]
Initialized /opt/gerrit
```

3- Iniciamos el servicio de Gerrit como root

```
service gerrit start
```

4- En el GUI, hacemos Sign In como el Administrador.

5- Buscamos la opcion de `Browse`, Groups


