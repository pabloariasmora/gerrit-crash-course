# Gerrit branch namespace para el code review 
Ahora podemos experimentar con un ciclo de revisión de la vida real poniendo en práctica el flujo de trabajo ya descrito. 

La única acción que no exploraremos en esta etapa es la integración de CI Build (de momento).

La primera acción que desencadena una revisión de código es el commit inicial enviada a Gerrit.

Gerrit presenta un nuevo namespace de referencia de Git completamente dedicado a la revisión de código con el prefijo refs/for antes del path de la branch (por ejemplo, para impulsar un cambio para su revisión en branch master, necesitamos usar refs/for/master).

## Clona un proyecto con buenas prácticas

1- Utilizando la interfaz web y nuestro usuario Administrador OpenSSO.

2- Damos click sobre Browse-> Repositories

3- Seleccionamos el repositorio `hello-world`

4- Podemos cambiar el tipo de URL (HTTP anónimo, SSH o HTTP autenticado) haciendo click en una de las columnas de la tabla en la parte superior del proyecto y luego copiar.

5- Utilizando el usuario `ubuntu`, pegar el comando `git clone` de la siguiente manera:

```
ubuntu@ip-172-31-16-198:~$ git clone "ssh://54.175.5.53:8080/hello-world"
Cloning into 'hello-world'...
remote: Counting objects: 2, done
remote: Finding sources: 100% (2/2)
remote: Total 2 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (2/2), 195 bytes | 195.00 KiB/s, done.
```

6- **Importante:** Aunque especificamos una contraseña para el usuario administrador, en este caso el repositorio de `hello-world` no solicitó ninguna autenticación de usuario/contraseña para la operación de clonación, ya que la lista de control de acceso predeterminada para el proyecto permitía lecturas anónimas. (Arreglaremos esto más tarde)

## Configurando el Repositorio Local

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
curl --insecure -Lo `git rev-parse --git-dir`/hooks/commit-msg https://54.175.5.53:8443/tools/hooks/commit-msg; chmod +x `git rev-parse --git-dir`/hooks/commit-msg
```

## Cambio a un branch de desarrollo

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

## Realizar un cambio a un branch de desarrollo con revisión de código.

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
remote:   http://34.229.91.40:8080/c/hello-world/+/1 Change on filewqx [NEW]
remote: 
To http://3.89.66.201:8080/hello-world
 * [new branch]      HEAD -> refs/for/dev
 
 ```
 
14- Visitamos el enlace, para visualizar nuestro cambio.

## Implementando Topics

La organización de `Changes` con `Topics` permite interactuar de forma selectiva con la audiencia de revisores más adecuada. 
Un `Topic` es un conjunto de changes relacionados que están destinados a cumplir un objetivo global o son parte de una función global.

Podrian representar `User Story`-`Feature`-`Componentes`-`Target version number`-`Targer build architecture`

Solo un `Topic` se puede asignar a un `Change`

1- Realizamos un cambio all archivo `new-file.txt`

```
ubuntu@ip-172-31-16-198:~/hello-world$ vi new-file.txt 
```

4- Dentro del archivo adjuntamos la frase

```
Gerrit New File For Code Review + TOPIC
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
Minor Change to new-file.txt to Test Topics
```

9- Confirmamos que el `Change-Id` se relacionara con el  `commit`

```
git log
```

12- Podemos crear un nuevo cambio y asignar un Topic llamado `First-Topic`, al aprovechar la parte derecha de la referencia Gerrit después del separador `%`, podemos agregar el parámetro del nombre del `Topic` para indicar el destino. Nuevamente realizar un `Push` para `Code Review` utilizando las referencias, pero con el nombre correcto del `Remote Branch` -> `dev`


```
git push origin HEAD:refs/for/dev%topic=First-Topic
```

13- De la salida de success, copiamos el URL del cambio. (ej: `http://34.229.91.40:8080/c/hello-world/+/2`)

```
ubuntu@ip-172-31-16-198:~/hello-world$ git push origin HEAD:refs/for/dev%topic=First-Topic
[...]
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 322 bytes | 322.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: Processing changes: refs: 1, new: 1, done    
remote: 
remote: SUCCESS
remote: 
remote:   http://34.229.91.40:8080/c/hello-world/+/2 Minor Change to new-file.txt to Test Topics [NEW]
remote: 
To http://3.89.66.201:8080/hello-world
* [new branch]      HEAD -> refs/for/dev%topic=First-Topic
 
```
 
14- Visitamos el enlace, para visualizar nuestro cambio. Y dentro del navegador podemos buscar por la palabra `First-Topic`, y damos click sobre ella.
 
15- Podemos visualizar un listado de Changes relacionados bajo el mismo `Topic`
 
16- Navegamos de vuelta a la ventana del `Change`.
 
17- Podemos eliminar el `Topic` en caso de ser necesario. Presionando sobre la `X` contiguo al nombre del `Topic`.
 
## Agregando Reviewers

1- Realizamos un cambio all archivo `new-file.txt`

```
ubuntu@ip-172-31-16-198:~/hello-world$ vi new-file.txt 
```

2- Dentro del archivo adjuntamos la frase

```
Gerrit New File For Code Review + REVIEWER
```

3- Guardamos el archivo.

4- Sumamos el nuevo archivo a la lista de cambios.

```
git add new-file.txt
```

5- Confirmamos los cambios

```
git commit
```

6- Escribimos en el archivo de confirmación.

```
Minor Change to new-file.txt to Add Reviewer
```

7- Confirmamos que el `Change-Id` se relacionara con el  `commit`

```
git log
```

8- Podemos crear un nuevo cambio y asignar un Topic llamado `First-Topic`, al aprovechar la parte derecha de la referencia Gerrit después del separador `%`, podemos agregar el parámetro del nombre del `Topic` para indicar el destino. Nuevamente realizar un `Push` para `Code Review` utilizando las referencias, pero con el nombre correcto del `Remote Branch` -> `dev`


```
git push origin HEAD:refs/for/dev%r=admin@example.com
```

9- De la salida de success, copiamos el URL del cambio. (ej: `http://34.229.91.40:8080/c/hello-world/+/2`)

```
ubuntu@ip-172-31-16-198:~/hello-world$ git push origin HEAD:refs/for/dev%r=admin@example.com
[...]
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 322 bytes | 322.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: Processing changes: refs: 1, new: 1, done    
remote: 
remote: SUCCESS
remote: 
remote:   http://34.229.91.40:8080/c/hello-world/+/3 Minor Change to new-file.txt to Add Reviewer [NEW]
remote: 
To http://3.89.66.201:8080/hello-world
 * [new branch]      HEAD -> refs/for/dev%%r=admin@example.com
 
 ```
 
 10- Visitamos el enlace, para visualizar nuestro cambio. Y dentro del navegador podemos buscar por la palabra `Reviewers`
 
 11- Podemos visualizar la lista de `Reviewers`. De la misma manera remover o agregar.
 

## Agregando Reviewers No existentes

1- Realizamos un cambio all archivo `new-file.txt`

```
ubuntu@ip-172-31-16-198:~/hello-world$ vi new-file.txt 
```

2- Dentro del archivo adjuntamos la frase

```
Gerrit New File For Code Review + REVIEWER NONE
```

3- Guardamos el archivo.

4- Sumamos el nuevo archivo a la lista de cambios.

```
git add new-file.txt
```

5- Confirmamos los cambios

```
git commit
```

6- Escribimos en el archivo de confirmación.

```
Minor Change to new-file.txt to Add Missing Reviewer
```

7- Confirmamos que el `Change-Id` se relacionara con el  `commit`

```
git log
```

8- Podemos crear un nuevo cambio y asignar un Topic llamado `First-Topic`, al aprovechar la parte derecha de la referencia Gerrit después del separador `%`, podemos agregar el parámetro del nombre del `Topic` para indicar el destino. Nuevamente realizar un `Push` para `Code Review` utilizando las referencias, pero con el nombre correcto del `Remote Branch` -> `dev`


```
git push origin HEAD:refs/for/dev%r=me@pabloariasmora.com
```

9- En la salida visualizamos el mensaje de error, dado que el usuario no existe. Los `Reviewers` deben ser usuarios registrados de Gerrit, es decir, usuarios que hayan iniciado sesión y que ya hayan proporcionado su dirección de correo electrónico.

```
git push origin HEAD:refs/for/dev%r=pabloariasmora@me.com
[...] 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 331 bytes | 331.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: Processing changes: refs: 1, done    
To https://34.230.70.247:8443/hello-world
 ! [remote rejected] HEAD -> refs/for/dev%r=pabloariasmora@me.com (Account 'pabloariasmora@me.com' not found
pabloariasmora@me.com does not identify a registered user or group)
error: failed to push some refs to 'https://34.230.70.247:8443/hello-world'
 
 ```
 
