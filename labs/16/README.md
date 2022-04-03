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
