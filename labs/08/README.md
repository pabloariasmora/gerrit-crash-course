# Descargar un repositorio por HTTP

Al usuario administrador de Gerrit se le otorga automáticamente el permiso para crear nuevos proyectos de Gerrit.

### Pasos

1- Utilizando la interfaz web y nuestro usuario Administrador OpenSSO.

2- Damos click sobre `Browse`-> `Repositories`

3- Luego en damos click sobre la opción a la derecha `Create New`

4- En la ventana que se nos despliega utilizamos la siguiente información:

```
Repository name: first-repo
Righs inherit from
Owner
Create initial empty commit: True
Only serve as parent for other repositories: False
```

5- Click sobre la opción `Create`

6- Recordemos que a nivel de los plugins se habia instalado `download-commands`. Esto nos permite visualizar los comandos para clonar a nivel del repositorio. Una vez creado nos muestra la pantalla de descarga para el repositorio.

7- Sobre la opción `ANONYMOUS HTTP`. Copiamos el comando que esta bajo la opcíon `Clone`.

Similar a

```
git clone "http://54.175.5.53:8080/first-repo"
```

8- Utilizando el usuario ubuntu, u otra máquina virtual con conexión a nuestra instancia de Gerrit. Ejecutamos el comando que copiamos anteriormente.

```
ubuntu@1.2.3.4 ubuntu % git clone http://54.90.80.219:8080/first-repo
Cloning into 'first-repo'...
remote: Counting objects: 2, done
remote: Finding sources: 100% (2/2)
remote: Total 2 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (2/2), 195 bytes | 97.00 KiB/s, done.
```

9- Listamos los archivos dentro de nuestro directorio actual

```
ls -la
```

10- Confirmamos tener una carpeta con el nombre de `first-repo`.

11- Abrimos la carpeta 

```
cd first-repo
```

12- Listamos los archivos dentro de esta nueva carpeta, incluyendo ocultos.

```
ls -la
```

13- Confirmamos la existencia de la carpeta oculta `.git`.



