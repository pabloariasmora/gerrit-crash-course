# Restringir accesos de usuarios Anónimos.

Si desea prohibir a los usuarios anónimos navegar/leer/buscar todos los cambios de un determinado proyecto, solo tiene que eliminar el permiso de lectura (`Read permission`) para usuarios anónimos (`anonymous users`) del proyecto.

Para poder modificar los permisos, debe ser administrador o propietario de ese proyecto.

### Para deshabilitar la navegación anónima, siga estos pasos:

1- Vaya a `Repositories` > Lista en el menú

2- Haga clic en el nombre del repositorio (o `All-Projects`, si desea modificar el valor predeterminado para todos los proyectos (requiere ser `Administrador`))

3- Elija `Access` en el submenú y presione el botón `Edit` allí.

4- De `Reference: refs/*` elimine en la sección `Read`-> `Anonymous Users` usando `REMOVE` en el lado derecho.

5- Presiona `SAVE` para guardar cambios 
