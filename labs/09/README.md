# Clona tu primer proyecto 

1- Utilizando la interfaz web y nuestro usuario Administrador OpenSSO.

2- Damos click sobre Browse-> Repositories

3- Seleccionamos el repositorio `hello-world`

4- Podemos cambiar el tipo de URL (HTTP anónimo, SSH o HTTP autenticado) haciendo click en una de las columnas de la tabla en la parte superior del proyecto y luego copiar.

5- Utilizando el usuario `ubuntu`, pegar el comando `git clone` de la siguiente manera:

```
ubuntu@ip-172-31-16-198:~$ git clone "http://54.175.5.53:8080/hello-world"
Cloning into 'hello-world'...
remote: Counting objects: 2, done
remote: Finding sources: 100% (2/2)
remote: Total 2 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (2/2), 195 bytes | 195.00 KiB/s, done.
```

6- **Importante:** Aunque especificamos una contraseña para el usuario administrador, en este caso el repositorio de `hello-world` no solicitó ninguna autenticación de usuario/contraseña para la operación de clonación, ya que la lista de control de acceso predeterminada para el proyecto permitía lecturas anónimas. (Arreglaremos esto más tarde)

# Configurando el Repositorio Local

Para que el repositorio local esté listo para ser utilizado por Gerrit, debemos realizar los siguientes pasos:

1- Configure `Git` `user.email` y `user.name` alineados con el nombre completo y el correo electrónico ingresados previamente en la pantalla de `Sign-In` de usuarios de Gerrit.

```
cd hello-world
git config user.name "Pablo Arias Mora"
git config user.email "pabloariasmora@hotmail.com"
```

2- Instale el `hook` `commit-msg` de Gerrit en el repositorio de Git clonado localmente. Se utiliza para generar el campo `Change-Id`, una identificación global única para cada nuevo cambio creado con un `git commit`
