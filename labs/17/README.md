# Conflictos

1- Por facilidad y congruencia en el laboratorio. Es mejor borrar nuestra copia local del repositorio.

```
rm -rf hello-world
```


## Clona un proyecto con buenas prácticas

1- Utilizando la interfaz web y nuestro usuario Administrador OpenSSO.

2- Damos click sobre Browse-> Repositories

3- Seleccionamos el repositorio `hello-world`

4- Podemos cambiar el tipo de URL `SSH` haciendo click en una de las columnas de la tabla en la parte superior del proyecto y luego copiar el commando `Clone with commit-msg hook`.

5- Utilizando el usuario `ubuntu`, pegar el comando `git clone` de la siguiente manera:

```
ubuntu@ip-172-31-16-198:~$ git clone "ssh://pabloariasmora@34.224.27.61:29418/hello-world" && scp -p -P 29418 pabloariasmora@34.224.27.61:hooks/commit-msg "hello-world/.git/hooks/"
Cloning into 'hello-world'...
remote: Counting objects: 2, done
remote: Finding sources: 100% (2/2)
remote: Total 2 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (2/2), 195 bytes | 195.00 KiB/s, done.
```

6- Entramos en la carpeta 

```
cd hello-world
```

7- Revisamos que nuestra `branch` sea `master`

```
git branch
* master
```
8- Por facilidad a nivel global, seteamos nuevamente las variables de email y username de git

```
git config --global user.name "Pablo Arias Mora"
git config --global user.email "pabloariasmora@hotmail.com"
```

9- Creamos dos `branch` locales

```
git checkout -b branch-a
git checkout master
git checkout -b branch-b
```

10- Dentro del `branch` `branch-a`, ejecutamos los siguientes commandos

```
git checkout branch-a
echo "echo Hello" > my_code.sh
git add my_code.sh
git commit 
```

11- Agregamos el siguiente mensaje en nuestro commit

```
Initial commit branch-a
```

12- Verificamos nuestro `Change-ID`

```
git log
```

13- Creamos un `patch-set` con nuestro cambio

```
git push origin HEAD:refs/for/master
```

10- Dentro del `branch` `branch-b`, ejecutamos los siguientes commandos

```
git checkout branch-b
```
Revisamos que nuestro archivo `my_code.sh` no exista

```
ls -la
```

Continuamos con los comandos

```
echo "mkdir -p test/dir/a" > my_code.sh
git add my_code.sh
git commit 
```

11- Agregamos el siguiente mensaje en nuestro commit

```
Initial commit branch-b
```

12- Creamos un `patch-set` con nuestro cambio

```
git push origin HEAD:refs/for/master
```

13- Revisamos la lista de `Changes` en la GUI de Gerrit
(ej: https://34.224.27.61:8443/q/status:open). A este punto deberá ser posible visualizar ambos `Changes` hacia las dos diferentes `branches`

14- Seleccionamos el primer `Change` - `branch-a`.

15- Asumamos el cambio se valido y no hay comentarios, entonces marcamos la casilla de `Code-Review+2`.

16- Revisamos que el estado del `Change` sea `Ready to submit`.

17- Damos click sobre `Submit` para realizar nuestro `merge`.

18- Confirmamos el `Submit`.

19- El estado final del `Change` debe ser `Merged`.

20- Nuevamente revisamos la lista de `Changes` en la GUI de Gerrit
(ej: https://34.224.27.61:8443/q/status:open). A este punto deberá ser posible visualizar solamente el `Change` - `branch-b`

21- Seleccionamos el primer `Change` sobre el `branch-b`.

22- Debido a nuestro ultimo `submit` el estado del `Change` es `Merge Conflict` y no `Active`

23- Asumamos el cambio se valido y no hay comentarios, entonces marcamos la casilla de `Code-Review+2`.

24- Revisamos que el estado del `Change` continue como `Merge Conflict`.

25- En esta ocasión el click sobre `Submit` esta disable hasta corregir el problema del `Merge Conflict`. Esto se debe a que el `branch` destino `master` a cambiado con respecto al origen que ha tomado este `branch`.

25- La forma de arreglar este problema es utilizar el comando de `git fetch` para este `patch-set`, para esto podemos utilizar el botón de `Download`. Y copiamos el comando presente en `checkout`.

Similar a:
```
git fetch "ssh://pabloariasmora@34.224.27.61:29418/hello-world" refs/changes/29/29/1 && git checkout FETCH_HEAD
```

26- Nuevamente en la terminal, revisamos que tenemos la ultima versión del repositorio.

```
git pull
```

27- Pegamos el comando de `git fetch` del paso 25

Salida similar a:

```
git fetch "ssh://pabloariasmora@34.224.27.61:29418/hello-world" refs/changes/29/29/1 && git checkout FETCH_HEAD
[...]
 git fetch "ssh://pabloariasmora@34.224
.27.61:29418/hello-world" refs/changes/29/29/1 && git checkout FETCH_HEAD
From ssh://34.224.27.61:29418/hello-world
 * branch            refs/changes/29/29/1 -> FETCH_HEAD
Note: switching to 'FETCH_HEAD'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 7a85f3e Initial commit branch-b
[...]
```

28- A este punto nos encontramos en un estado de `DETTACH HEAD` donde podemos ver los cambio dentro del Code Review. Ahora podemos ejecutar el `rebase` hacia el `branch` destino (`master`).

```
git rebase origin/master
```

Como era esperado el `auto-merge` fallo por los conflictos. Entonces debemos solucionar los conflictos y completar el `rebase` antes de enviar nuestros cambios a `Change Review`.

29- Resolvamos los conflictos. Pero para esto debemos saber sobre cuales files debemos de trabajar, el siguiente comando nos presenta una lista de archivos con conflictos

```
git diff --name-only --diff-filter=U
```

30- Si queremos revisar a detalle cuales son estos conflictos podemos remover las banderas del comando anterior para filtrado.

```
git diff
```


