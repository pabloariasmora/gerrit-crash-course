# Descargar un repositorio de manera anónima por HTTP

Al usuario administrador de Gerrit se le otorga automáticamente el permiso para crear nuevos proyectos de Gerrit.

### Pasos

1- Utilizando la interfaz web y nuestro usuario Administrador OpenSSO.

2- Damos click sobre `Browse`-> `Repositories`

3- Luego en damos click sobre la opción a la derecha `Create New`

4- En la ventana que se nos despliega utilizamos la siguiente información:

```
Repository name: first-repo
Righs inherit from
Owner
Create initial empty commit: True
Only serve as parent for other repositories: False
```

5- Click sobre la opción `Create`

6- Recordemos que a nivel de los plugins se habia instalado `download-commands`. Esto nos permite visualizar los comandos para clonar a nivel del repositorio. Una vez creado nos muestra la pantalla de descarga para el repositorio.

7- Sobre la opción `ANONYMOUS HTTP`. Copiamos el comando que esta bajo la opcíon `Clone`.

Similar a

```
git clone "http://54.175.5.53:8080/first-repo"
```

8- Utilizando el usuario ubuntu, u otra máquina virtual con conexión a nuestra instancia de Gerrit. Ejecutamos el comando que copiamos anteriormente.

```
ubuntu@1.2.3.4 ubuntu % git clone http://54.90.80.219:8080/first-repo
Cloning into 'first-repo'...
remote: Counting objects: 2, done
remote: Finding sources: 100% (2/2)
remote: Total 2 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (2/2), 195 bytes | 97.00 KiB/s, done.
```

9- Listamos los archivos dentro de nuestro directorio actual

```
ls -la
```

10- Confirmamos tener una carpeta con el nombre de `first-repo`.

11- Abrimos la carpeta 

```
cd first-repo
```

12- Listamos los archivos dentro de esta nueva carpeta, incluyendo ocultos.

```
ls -la
```

13- Confirmamos la existencia de la carpeta oculta `.git`.

14- Borramos la carpeta

```
cd ..
rm -rf first-repo
```

# Descargar un repositorio con nuestro usuario administrador por HTTP

1- Definir la contraseña HTTP a utilizar con el usuario. Seleccionar el nombre del usuario a la derecha de la pantalla, y luego la opción de `Settings`.

2- El menú izquierdo seleccionar la opción `HTTP Credentials`.

3- Click sobre `Generate New Password`. Anotar el valor de la contraseña.
   Si despliega un error de que no tiene `username` asignado, solamente deben subir a las opciones de `Profile`, y definir uno, luego volver a ejecutar el paso 2-3.

4- Volver a la página principal del repositorio `first-project`.

5- Sobre la opción `HTTP`. Copiamos el comando que esta bajo la opcíon `Clone`.

Similar a

```
git clone "http://54.175.5.53:8080/a/first-repo"
```

6- Utilizando el usuario ubuntu, u otra máquina virtual con conexión a nuestra instancia de Gerrit. Ejecutamos el comando que copiamos anteriormente. Esta vez nos solicitara el nombre de usuario y la contraseña (en este valor colocamos la contraseña anotada).

```
ubuntu@1.2.3.4 test-gerrit % git clone "http://54.90.80.219:8080/a/first-repo"
Cloning into 'first-repo'...
Username for 'http://54.90.80.219:8080': pabloariasmora
Password for 'http://pabloariasmora@54.90.80.219:8080': 
remote: Counting objects: 2, done
remote: Finding sources: 100% (2/2)
remote: Total 2 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (2/2), 195 bytes | 48.00 KiB/s, done.
```

7- Listamos los archivos dentro de nuestro directorio actual

```
ls -la
```

8- Confirmamos tener una carpeta con el nombre de `first-repo`.

9- Abrimos la carpeta 

```
cd first-repo
```

10- Listamos los archivos dentro de esta nueva carpeta, incluyendo ocultos.

```
ls -la
```

11- Confirmamos la existencia de la carpeta oculta `.git`.

12- Borramos la carpeta

```
cd ..
rm -rf first-repo
```

# Descargar un repositorio por medio de SSH

1- Debemos previamente cumplir con los laboratorios 02 y 06.

2- Volver a la página principal del repositorio `first-project`.

3- Sobre la opción `SSH`. Copiamos el comando que esta bajo la opcíon `Clone`. De no estar presente y cumplir con el primer paso, solamente `SignOut - SignIn`

Similar a

```
git clone "ssh://pabloariasmora@54.90.80.219:29418/first-repo"
```

4- Utilizando el usuario ubuntu, u otra máquina virtual con conexión a nuestra instancia de Gerrit. Ejecutamos el comando que copiamos anteriormente. Esta vez no solicitara la instancia porque hace uso de las SSH Keys configuradas previamente, pero si nos solicitará confirmar la identidad de nuestro servidor de gerrit (yes).

Similar a

```
git clone "ssh://pabloariasmora@54.90.80.219:29418/first-repo"
Cloning into 'first-repo'...
The authenticity of host '[54.90.80.219]:29418 ([54.90.80.219]:29418)' can't be established.
ED25519 key fingerprint is SHA256:JhUUuIdk+8TSfdsECV6GrpybPd87S8ulf8SvBvjCwFEHzI.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[54.90.80.219]:29418' (ED25519) to the list of known hosts.
remote: Counting objects: 2, done
remote: Finding sources: 100% (2/2)
remote: Total 2 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (2/2), done.
```
5- Listamos los archivos dentro de nuestro directorio actual

```
ls -la
```

6- Confirmamos tener una carpeta con el nombre de `first-repo`.

7- Abrimos la carpeta 

```
cd first-repo
```

8- Listamos los archivos dentro de esta nueva carpeta, incluyendo ocultos.

```
ls -la
```

9- Confirmamos la existencia de la carpeta oculta `.git`.

10- Borramos la carpeta

```
cd ..
rm -rf first-repo
```


