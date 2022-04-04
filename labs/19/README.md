# Conflictos - Cherry Picking

1- Utilizando el repositorio descargado en el laboratorio 17. Entramos en la carpeta 

```
cd hello-world
```

2- Nos aseguramos de utilizar el `branch` `master`.

```
git checkout master
git branch
```

3- Tomamos la ultima versión del `branch`

```
git pull origin master
```

4- Esto posiblemente nos muestra nuestro archivo `my_code.sh`

```
ls my_code.sh
```

5- Creamos un nuevo `branch` local

```
git checkout -b branch-c
```

6- Dentro del `branch` `branch-c`, ejecutamos los siguientes commandos

```
git checkout branch-c
echo "echo Hello" > another_code.sh
git add another_code.sh
git commit 
```

7- Agregamos el siguiente mensaje en nuestro commit

```
Initial commit branch-c
```

8- Verificamos nuestro `Change-ID`

```
git log
```

9- Creamos un `patch-set` con nuestro cambio

```
git push origin HEAD:refs/for/master
```

10- Creamos un nuevo `branch` local

```
git checkout -b branch-d
```

Revisamos que nuestro archivo `another_code.sh` no exista

```
ls -la
```

Continuamos con los comandos

```
echo "echo Done" > my_code.sh
git add my_code.sh
git commit 
```

11- Agregamos el siguiente mensaje en nuestro commit

```
Initial commit branch-d
```

12- Creamos un `patch-set` con nuestro cambio

```
git push origin HEAD:refs/for/master
```


13- Revisamos la lista de `Changes` en la GUI de Gerrit
(ej: https://34.224.27.61:8443/q/status:open). A este punto deberá ser posible visualizar ambos `Changes` hacia las dos diferentes `branches`

14- Seleccionamos el segundo `Change` - `branch-d`.

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

31- Editamos el archivo `my_code.sh`

```
vi my_code.sh
```

32- Realizamos las ediciones deseadas sobre el archivo. En este caso removeremos las siguientes lineas, asumiendo que ese es el cambio deseado.

```
<<<<<<< HEAD
echo Hello
=======
>>>>>>> Initial commit branch-b
```

Resumen de significados

La línea (o líneas) entre las líneas que comienzan `<<<<<<<` y `======` es lo que tenemos localmente.

Ej:

```
<<<<<<< HEAD
echo Hello
=======
```

La línea (o líneas) entre las líneas que comienzan `=======` y `>>>>>>>` es lo que introdujo el otro (`pulled`) `commit` en este caso `>>>>>>> Initial commit branch-b`

Ej:

```
=======
mkdir -p test/dir/a
>>>>>>> Initial commit branch-b
```

Para nuestro ejemplo nuestro archivo solo debe contener la siguiente linea:

```
mkdir -p test/dir/a
```

33- Agregamos nuestro archivo dentro de la lista de archivos del `commit`

```
git add my_code.sh
```

34- Procedemos a continuar con nuestro `rebase`

```
git rebase --continue
```

En este punto hemos aplicado el `commit` del `change review`.
El `Change-ID` de este cambio no ha sido modificado para que Gerrit pueda tener el historial de cambios.

```
git log
```

35- Vamos a generar un `patch-set` para iniciar nuevamente el proceso de `code-review`.

```
git push origin HEAD:refs/for/master
```

Salida similar a:

```
git push origin HEAD:refs/for/master
[...]
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 319 bytes | 319.00 KiB/s, done.
Total 3 (delta 0), reused 2 (delta 0)
remote: Processing changes: refs: 1, updated: 1, done    
remote: warning: c38ead2: no files changed, was rebased
remote: 
remote: SUCCESS
remote: 
remote:   https://34.224.27.61:8443/c/hello-world/+/29 Initial commit branch -b
remote: 
To ssh://34.224.27.61:29418/hello-world
 * [new branch]      HEAD -> refs/for/master
[...] 
```

36- Nuevamente revisamos la lista de `Changes` en la GUI de Gerrit
(ej: https://34.224.27.61:8443/q/status:open). A este punto deberá ser posible visualizar solamente el `Change` - `branch-b`

37- Seleccionamos el primer `Change` sobre el `branch-b`.

38- Debido a `rebase` el estado del `Change` es `Active` nuevamente, pero como un segundo `patch-set`

39- Asumamos el cambio se valido y no hay comentarios, entonces marcamos la casilla de `Code-Review+2`.

40- Damos click sobre `Submit` para realizar nuestro `merge`.

41- Confirmamos el `Submit`.

42- El estado final del `Change` debe ser `Merged`.
