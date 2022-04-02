# Descargar un repositorio de manera anónima por HTTP

Al usuario administrador de Gerrit se le otorga automáticamente el permiso para crear nuevos proyectos de Gerrit.

### Pasos

1- Utilizando la interfaz web y nuestro usuario Administrador OpenSSO.

2- Damos click sobre `Browse`-> `Repositories`

3- Luego en damos click sobre la opción a la derecha `Create New`

4- En la ventana que se nos despliega utilizamos la siguiente información:

```
Repository name: second-repo
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
git clone "http://54.175.5.53:8080/second-repo"
```

8- Utilizando el usuario ubuntu, u otra máquina virtual con conexión a nuestra instancia de Gerrit. Ejecutamos el comando que copiamos anteriormente.

```
ubuntu@1.2.3.4 ubuntu % git clone http://54.90.80.219:8080/second-repo
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

10- Confirmamos tener una carpeta con el nombre de `second-repo`.

11- Abrimos la carpeta 

```
cd second-repo
```

12- Listamos los archivos dentro de esta nueva carpeta, incluyendo ocultos.

```
ls -la
```

13- Confirmamos la existencia de la carpeta oculta `.git`.

14- Borramos la carpeta

```
cd ..
rm -rf second-repo
```

# Restringir accesos de usuarios Anónimos.

Si desea prohibir a los usuarios anónimos navegar/leer/buscar todos los cambios de un determinado proyecto, solo tiene que eliminar el permiso de lectura (`Read permission`) para usuarios anónimos (`anonymous users`) del proyecto.

Para poder modificar los permisos, debe ser administrador o propietario de ese proyecto.

### Para deshabilitar la navegación anónima, siga estos pasos:

1- Vaya a `Repositories` > Lista en el menú

2- Haga clic en el nombre del repositorio (o `All-Projects`, si desea modificar el valor predeterminado para todos los proyectos (requiere ser `Administrador`))

3- Elija `Access` en el submenú y presione el botón `Edit` allí.

4- De `Reference: refs/*` elimine en la sección `Read`-> `Anonymous Users` usando `REMOVE` en el lado derecho.

5- Presiona `SAVE` para guardar cambios 
