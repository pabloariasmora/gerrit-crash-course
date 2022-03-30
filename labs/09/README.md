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

2- Instale el `hook` `commit-msg` de Gerrit en el repositorio de Git clonado localmente. Se utiliza para generar el campo `Change-Id`, una identificación global única para cada nuevo cambio creado con un `git commit`, reemplazar la `IP` en el comando por la IP Pública de su servidor Ej:`54.175.5.53`

```
mkdir -p .git/hooks
curl -Lo `git rev-parse --git-dir`/hooks/commit-msg http://54.175.5.53:8080/tools/hooks/commit-msg; chmod +x `git rev-parse --git-dir`/hooks/commit-msg)
```

# Cambio a un branch de desarrollo

1- Verificamos nos encontremos en el `Branch`-> `master`

```
git branch
* master
```

2- Creamos una nueva `Branch Local`-> `development`

```
git checkout -b development
Switched to a new branch `development`
```

3- Ahora vamos a generar un cambio en el repositorio, esto creando un nuevo archivo llamado `new-file.txt`

```
ubuntu@ip-172-31-16-198:~/hello-world$ touch new-file.txt
ubuntu@ip-172-31-16-198:~/hello-world$ vi new-file.txt 
```

4- Dentro del archivo escribimos la frase

```
Gerrit New File
```

5- Sumamos el nuevo archivo a la lista de cambios.

```
git add new-file.txt
```

6- Confirmamos los cambios

```
git commit
```

7- Escribimos en el archivo de confirmación.

```
Added a new file called: new-file.txt
```

8- Confirmamos que el `Change-Id` se relacionara con el  `commit`

```
git log
```

9- En el mensaje de salida buscamos por la palabra `Change-Id`.

10- Sincronizamos el `Branch Local` con el repositorio remoto

```
git push origin HEAD:dev

[...]
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Writing objects: 100% (3/3), 318 bytes | 318.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: Processing changes: refs: 1, done    
To http://54.175.5.53:8080/hello-world
 * [new branch]      HEAD -> dev
```

11- El comando debería hacer solicitud de la contraseña si se utilizó el git clone por http. Si se utilizó por medio de ssh utilizará las llaves de SSH.

12- Una vez completado, el nuevo `Branch` se puede visualizar en nuestra interfaz de Gerrit. 
`Browse`->`Repository`->`hello-world`->`Branches`->`dev`

13- *Importante*: El nombre del `Branch local`(`development`) puede ser totalmente diferente al remoto (`dev`).

14- Podemos visualizar el cambio si damos click en `Browse`

15- Podemos visualizar el contenido del archivo, si damos click sobre su nombre `new-file.txt`

16- Podemos observar como hemos realizado un `Bypass` a la revisión de código.

# Realizar un cambio a un branch de desarrollo con revisión de código.

1- Realizamos un cambio all archivo `new-file.txt`

```
ubuntu@ip-172-31-16-198:~/hello-world$ vi new-file.txt 
```

4- Dentro del archivo adjuntamos la frase

```
Gerrit New File For Code Review
```

5- Guardamos el archivo.

6- Sumamos el nuevo archivo a la lista de cambios.

```
git add new-file.txt
```

7- Confirmamos los cambios

```
git commit
```

8- Escribimos en el archivo de confirmación.

```
Minor Change to new-file.txt
```

9- Confirmamos que el `Change-Id` se relacionara con el  `commit`

```
git log
```

10- Podemos realizar un `Push` para `Code Review` utilizando las referencias.

```
git push origin HEAD:refs/for/development
```

11- El commando anterior debio mostrar una salida de error. Ya que no existe un `Remote Branch` llamado `development`

```
ubuntu@ip-172-31-16-198:~/hello-world$ git push origin HEAD:refs/for/development: 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 322 bytes | 322.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: Processing changes: refs: 1, done    
To http://3.89.66.201:8080/hello-world
 ! [remote rejected] HEAD -> refs/for/development (branch development not found)
error: failed to push some refs to 'http://3.89.66.201:8080/hello-world'
```

12- Nuevamente realizar un `Push` para `Code Review` utilizando las referencias, pero con el nombre correcto del `Remote Branch` -> `dev`

```
git push origin HEAD:refs/for/dev
```

13- De la salida de success, copiamos el URL del cambio. (ej: `http://34.229.91.40:8080/c/hello-world/+/2`)

```
ubuntu@ip-172-31-16-198:~/hello-world$ git push origin HEAD:refs/for/dev
[...]
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 322 bytes | 322.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: Processing changes: refs: 1, new: 1, done    
remote: 
remote: SUCCESS
remote: 
remote:   http://34.229.91.40:8080/c/hello-world/+/2 Change on filewqx [NEW]
remote: 
To http://3.89.66.201:8080/hello-world
 * [new branch]      HEAD -> refs/for/dev
 
 ```
 
 14- Visitamos el enlace, para visualizar nuestro cambio.
