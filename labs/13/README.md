# Visualizar los cambios realizados en el repositorio.

1- Utilizando la interfaz web y nuestro usuario Administrador OpenSSO.

2- Damos click sobre `Browse`-> `Repositories`

3- Luego en damos click sobre la opción a la derecha `Create New`

4- En la ventana que se nos despliega utilizamos la siguiente información:

```
Repository name: hello-world
Righs inherit from
Owner
Create initial empty commit: True
Only serve as parent for other repositories: False
```

5- Click sobre la opción `Create`

6- Una vez creado vamos a la opción izquierda `Branches`

7- Como podemos observar la columna de `Repository Browser` esta vacia. Como configuración por defecto Gerrit no tiene un visualizador para los repositorios.

8- Detener el servicio de Gerrit como root

```
service gerrit stop
```

9- Ejecutar nuevamente la configuración inicial de Gerrit, utilizando el usuario gerrit

```
cd /opt/gerrit
export GERRIT_VERSION=3.1.3
java -jar gerrit-$GERRIT_VERSION.war init
```

10- Mantener los valores por defecto excepto el valor en la parte de plugins, especificamente `gitiles`.

```
Install plugin gitiles version v3.1.3 [y/N]? Y
Installed gitiles v3.1.3
```

11- Iniciar el servicio de Gerrit como root

```
service gerrit start
```

12- Utilizando la interfaz web y nuestro usuario Administrador OpenSSO. Damos click sobre `Browse`-> `Repositories`

13- Seleccionamos la opción del repositorio `hello-world` que dice `Repository Browser`->`Browser`. Entonces podremos ver una UI destinada a mostrarnos los archivos que esten dentro del repositorio.

14- Para visualizar cada `Branch` podemos selecionar el nombre del repositorio `hello-world`, una vez dentro de el seleccionamos la opción izquierda `Branches`.

15- Como podemos observar la columna de `Repository Browser` esta activa. 
