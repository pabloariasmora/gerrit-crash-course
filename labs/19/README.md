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

Revisamos que nuestro archivo `another_code.sh` exista

```
ls -la
```

Continuamos con los comandos

```
echo "echo Done" >> my_code.sh
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
(ej: https://34.224.27.61:8443/q/status:open). A este punto deberá ser posible visualizar ambos `Changes`.

14- Seleccionamos el segundo `Change` - `branch-d`.

15- Asumamos el cambio se valido y no hay comentarios, entonces marcamos la casilla de `Code-Review+2`.

16- Revisamos que el estado del `Change` continue como `Active`. Y que la marca sobre `Submit` sea `Submit including parents`

Esto debido a la dependencia con el `branch` local `branch-c`

`Cherrypick` es una utilidad que le permite seleccionar una referencia de permiso específica de un `Change` de otra `branch` y aplicarla al `branch` de destino.

`Cherry picking` ayuda a enviar un `Change` que dependa de otro `Change` seleccionando esa referencia específica.

`Cherry picking` ayudará a hacer `merge` una referencia de `change` específica, incluso si existe una dependencia de un `change` anterior. (Nuestro caso) Este `Change` no se puede enviar y hacer `merge` debido a una dependencia con una revisión de `change` anterior, que aún no se ha aprobado.


17- Para realizar el `cherry-pick`, necesitamos que el comando disponible en el botón `Download`.

18- Copiamos el comando `Cherry Pick`

Similar a:

```
git fetch "ssh://pabloariasmora@34.224.27.61:29418/hello-world" refs/changes/36/36/1 && git cherry-pick FETCH_HEAD
```

19- **IMPORTANTE** en el repositorio local. Asegurese de estar en el `branch` destino
