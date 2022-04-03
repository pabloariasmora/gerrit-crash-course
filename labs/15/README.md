# Gerrit usando Patch-Sets

## Limpiando laboratorios anteriores

1- Verificamos que en nuestra terminal no exista previamente el directorio `hello-world`.

```
ls 
```

1- Elimine la carpeta presente en cuyo nombre es `hello-world`

```
rm -rf hello-wold
```

## Clona un proyecto con commit-msg

1- Utilizando la interfaz web y nuestro usuario Administrador OpenSSO.

2- Damos click sobre Browse-> Repositories

3- Seleccionamos el repositorio `hello-world`

4- Copiar el comando desplegado bajo `SSH` - `Clone with commit-msg hook`

5- Utilizando el usuario `ubuntu`, pegar el comando `git clone` de la siguiente manera:

```
ubuntu@ip-172-31-20-48:~$ git clone "ssh://pabloariasmora@34.230.70.247:29418/hello-world" && scp -p -P 29418 pabloariasmora@34.230.70.247:hooks/commit-msg "hello-world/.git/hooks/"
Cloning into 'hello-world'...
The authenticity of host '[34.230.70.247]:29418 ([34.230.70.247]:29418)' can't be established.
ECDSA key fingerprint is SHA256:0AfHCUU+WPZMa5BgLGvg18HoVHJtaNV7foPHrZoaph0.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[34.230.70.247]:29418' (ECDSA) to the list of known hosts.
remote: Counting objects: 5, done
remote: Finding sources: 100% (5/5)
Receiving objects: 100% (5/5), 519 bytes | 519.00 KiB/s, done.
remote: Total 5 (delta 0), reused 3 (delta 0)
commit-msg             
```

6- **Importante:** Aunque especificamos una contraseña para el usuario administrador, en este caso el repositorio de `hello-world` no solicitó ninguna autenticación de usuario/contraseña para la operación de clonación, ya que la lista de control de acceso predeterminada para el proyecto permitía lecturas anónimas. (Arreglaremos esto más tarde)

## Configurando el Repositorio Local (ya que previamente lo borramos)

Para que el repositorio local esté listo para ser utilizado por Gerrit, debemos realizar los siguientes pasos:

7- Configure `Git` `user.email` y `user.name` alineados con el nombre completo y el correo electrónico ingresados previamente en la pantalla de `Sign-In` de usuarios de Gerrit.

```
cd hello-world
git config user.name "Pablo Arias Mora"
git config user.email "pabloariasmora@hotmail.com"
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

3- Sincronizamos el `Branch Local` con el repositorio remoto

```
git push origin HEAD:development

ubuntu@ip-172-31-20-48:~/hello-world$ git push origin HEAD:development
[...]
Total 0 (delta 0), reused 0 (delta 0)
remote: Processing changes: refs: 1, done    
To ssh://34.230.70.247:29418/hello-world
 * [new branch]      HEAD -> development
```

4- Ahora vamos a generar un cambio en el repositorio, esto creando un nuevo archivo llamado `new-file.txt`

```
ubuntu@ip-172-31-16-198:~/hello-world$ touch demo.txt
ubuntu@ip-172-31-16-198:~/hello-world$ vi demo.txt 
```

4- Dentro del archivo escribimos la frase

```
Gerrit Demo File
```

5- Sumamos el nuevo archivo a la lista de cambios.

```
git add demo.txt
```

6- Confirmamos los cambios

```
git commit
```

7- Escribimos en el archivo de confirmación.

```
Added a new file called: demo.txt
```

8- Confirmamos que el `Change-Id` se relacionara con el  `commit`

```
git log
```

9- En el mensaje de salida buscamos por la palabra `Change-Id`.

10- Podemos realizar un `Push` para `Code Review` utilizando las referencias.

```
git push origin HEAD:refs/for/development
```

El commando anterior debio mostrar una salida similar a:

```
ubuntu@ip-172-31-20-48:~/hello-world$ git push origin HEAD:refs/for/development
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Writing objects: 100% (3/3), 322 bytes | 322.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: Processing changes: refs: 1, new: 1, done    
remote: 
remote: SUCCESS
remote: 
remote:   https://34.230.70.247:8443/c/hello-world/+/6 Added a new file called: demo.txt [NEW]
remote: 
To ssh://34.230.70.247:29418/hello-world
 * [new branch]      HEAD -> refs/for/development
```

11- De la salida de success, copiamos el URL del cambio. (ej: `https://34.230.70.247:8443/c/hello-world/+/6`)

13- Visitamos el enlace, para visualizar nuestro cambio. Buscando por la palabra `patch set`.

## Creando un nuevo path-set

Para fines prácticos podemos suponer debemos realizar un cambio en nuestro archivo, ya que la revisión de código anterior recibio un comentario de realizar el cambio.

1- Agregamos información en nuestro archivo `demo.txt`

```
echo "Change in File" >> demo.txt 
```

2- Agregamos el archivo a nuestro próximo `commit`

```
git add demo.txt
```

3- Realizamos un `commit` utilizando parte de nuestro mensaje anterior, esto para mantener un poco de limpieza dentro de nuestro historial.

```
git commit --amend
```

En ese mensaje de commit agregamos en una siguiente linea

```
Added a new file called: demo.txt
Applied Changes
```

4- Confirmamos que el `Change-Id` no con este nuevo `commit`. Gerrit asocia una nueva confirmación al mismo `change` utilizando el `Change-ID`. La forma más típica de enviar un nuevo `patch-set` es usando la opción `--amend` para volver a escribir un `commit` existente; de lo contrario, se generará un nuevo ID mediante `commit-hook`, que luego creará un nuevo `Change` en lugar de agregar un `patch-set` al existente

```
git log
```

5- Podemos realizar un `Push` para `Code Review` utilizando las referencias.

```
git push origin HEAD:refs/for/development
```

El commando anterior debio mostrar una salida similar a:

```
ubuntu@ip-172-31-20-48:~/hello-world$ git push origin HEAD:refs/for/development
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Writing objects: 100% (3/3), 322 bytes | 322.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: Processing changes: refs: 1, new: 1, done    
remote: 
remote: SUCCESS
remote: 
remote:   https://34.230.70.247:8443/c/hello-world/+/6 Added a new file called: demo.txt Final private published Change
remote: 
To ssh://34.230.70.247:29418/hello-world
 * [new branch]      HEAD -> refs/for/development
```

11- De la salida de success, copiamos el URL del cambio. (ej: `https://34.230.70.247:8443/c/hello-world/+/6`)

12- Visitamos el enlace, para visualizar nuestro cambio. Buscando por la palabra `patch set`. Debemos confirmar que el mismo ha sido incrementado en `+1`.

13- Ambos cambios son mapeados dentro del historial del nuevo `patch-set`

