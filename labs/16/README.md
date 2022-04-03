# Dependencias de `Change` en la vida real con Gerrit

1- Verificamos nos encontremos en el `branch`-> `development`

```
git branch
* development
master
```

2- Para este ejemplo vamos a utilizar el `master` `branch`

```
git checkout master
[...]
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

3- Verificamos nos encontremos en el `branch`-> `master`

```
git branch
* master
development
```
4- Primero creamos un `Change A1` relacionado al `Topic A`

```
echo "Change A1" > change-a1.txt
git add change-a1.txt
git commit -a -m "Change A1"
[...]
git push origin HEAD:refs/for/master%topic=topic-A 
```

5- Ahora creamos el `CHange A2` sobre el `Change A1`, que es nuestro punto principal:

```
echo "Change A2" >> change-a1.txt 
git add change-a1.txt 
git commit -a -m "Change A2" 
git push origin HEAD:refs/for/master%topic=topic-A
```
6- A continuación, debemos restablecer el HEAD actual al `Change A1` y crear un nuevo `Change B`.

```
git checkout HEAD^
```

Salida similar a:
```
[...]
Note: switching to 'HEAD^'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 954f332 Change A1
[...]
```

7- Realizamos el `Change B`:

```
echo "Change B" > change-b.txt
git add change-b.txt
git commit -m "Change B"
[…]
git push origin HEAD:refs/for/master
```

8- Finalmente, creamos el `Change C` encima del `Change B`, que es el `HEAD` actual:

```
echo "Change C" > change-c.txt
git add change-c.txt
git commit -m "Change C"
[...]
git push origin HEAD:refs/for/master
[...]
```

## Navegando a través del gráfico de dependencias de cambios de Gerrit

Los cuatro cambios ahora se han enviado a Gerrit y podemos ver cómo se han conectado en su gráfico de dependencia.

1- Al abrir la GUI de Gerrit en `Changes` opción izquierda.
(EJ: https://34.224.27.61:8443/q/status:open)

2- Seleccionamos el `Change A1`.

3-Podemos ver la sección de dependencias sobre `Relation chain`, que indica los vínculos con sus hijos dependientes.

El `Change A1` está vinculado al `Change A2` y al `Change B` ^ `Change C` como nodos dependientes. Todas las entradas que se muestran en la lista de dependencias se pueden utilizar como hipervínculos para navegar por el gráfico muy fácilmente.

4- Hacer clic en `Change B` para navegar hacia abajo a través del gráfico de dependencia. Enfasis en que a nivel de `Change B` desconoce `Change A2`
