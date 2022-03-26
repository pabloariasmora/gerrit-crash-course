Autenticación a través de Internet a través de OpenID
====

1- Detener el daemon de Gerrit

```
/opt/gerrit/bin/gerrit.sh stop
```

2- Ejecutar nuevamente la configuración inicial de Gerrit 

```
cd /opt/gerrit
java -jar gerrit.war init
```

Mantener los valores por defecto excepto el Authentication method.

```
[...]
*** User Authentication
***
Authentication method [DEVELOPMENT_BECOME_ANY_ACCOUNT/?]: openid RETURN
[...]
Initialized /opt/gerrit
```

3- Iniciar nuevamente el daemon.

```
/opt/gerrit/bin/gerrit.sh start
Starting Gerrit Code Review: OK
```

4- Abrir en el navegador el URL de Gerrit, usado en los laboratorios anteriores.

5- Observar como ha cambiado, no permite el uso de admin, y nos muestra el logo de OpenID.

6- Hacer click sobre `Sign in with a Launchpad ID`, esto nos redirigue a la página de `https://login.launchpad.net/+login`

6.1- `Poseen usuario registrado`: 

      6.1.1-  Deben ingresar el usuario (ó correo electrónico).

      6.1.2- Seleccionar la opcíon `I have an Ubuntu One account and my password is:` 

      6.1.3- Ingresar la contraseña.

      6.1.4- Por último hacer click sobre el botón de `Log In`.

6.2- `No poseen usuario registrado`: 

      6.2.1- Deben ingresar correo electrónico del nuevo usuario.

      6.2.2- Seleccionar la opcíon `I don’t have an Ubuntu One account:`

      6.2.3- Ingresar nombre completo `Full Name`

      6.2.4- El usuario `Username`

      6.2.5- Ingresar la contraseña.

      6.2.6- Confirmar la contraseña.

      6.2.7- Aceptar los términos y condiciones.

      6.2.8- Por último hacer click sobre el botón de `Create account`.

      6.2.9- Verificar el correo electrónico.

6.3- `Para ambos casos`:

      6.3.1- Confirmar la solicitud de compartir la informacíon personal. Tanto el correo electrónico como el nombre completo.

      6.3.2- Por último hacer click sobre el botón de `Yes, log me in`.

Esto nos va a redirigir nuevamente a la pagina principal de Gerrit.

6.4- Entramos a las opciones de configuración, dar click sobre su nombre a la izquierda y luego dar click a `Settings`.

6.5- Podemos observar que en este punto no es posible cambiar el correo electrónico del usuario.

6.6- Debido a la configuración por defecto de LaunchPad, el nombre de usuario no se ha compartido, por ende debemos actualizarlo en el cambio de `Username` de manera manual.

6.7- Click en `Save Changes`

Ocultar las demás opciones de OpenID. Y realizar una redireccíon a OpenID
=======

Podríamos nuevamente utilizar el `*.war` y su parámetro `init`. Pero este no nos permite detallar en las opciónes de configuración de OpenID. Esta vez utilizaremos otro método.

1- Detener el daemon de Gerrit

```
/opt/gerrit/bin/gerrit.sh stop
```

2- Cambiaremos el tipo de autenticación de OpenID a OpenID_SSO. Esto admite OpenID de un solo proveedor. No hay registro y el enlace "Iniciar sesión" envía al usuario directamente Entry-Point de SSO de la cuenta del proveedor de OpenID.

```
git config --file etc/gerrit.config auth.type OpenID_SSO
```

3- Debemos especificar cual será el ENtry-Point a utilizar, en este caso `https://login.launchpad.net/+openid`

```
git config --file etc/gerrit.config auth.openIdSsoUrl https://login.launchpad.net/+openid
```

4- Iniciar nuevamente el daemon.

```
/opt/gerrit/bin/gerrit.sh start
Starting Gerrit Code Review: OK
```

5- Podemos interntar volver a inicar sesión. Esta vez el botón de `Sign In`, nos reenviará directamente a `launchpad.net`.

Otras formas de modificar las configuraciones.
======

1- Podemos modificar el archivo localizado en `/opt/gerrit/etc/gerrit.config` para configurar nuestro sistema.

```
vi /opt/gerrit/etc/gerrit.config
```

2- Toda modificación realizada debe estar acompañada de un reinicio, para que la misma tenga efecto.


