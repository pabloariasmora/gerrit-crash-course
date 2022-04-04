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
git commit -am 'initial commit branch-a'
```




git config --global user.name "Pablo Arias Mora"
git config --global user.email "pabloariasmora@hotmail.com"
