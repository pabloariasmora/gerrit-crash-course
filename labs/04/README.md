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

6.2.3- Ingresar la contraseña.

6.2.4- Por último hacer click sobre el botón de `Log In`.

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
