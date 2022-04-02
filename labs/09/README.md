# Habilitar HTTPS con Certificado `Self-Signed`

El acceso a la interfaz de usuario web de Gerrit a través de Internet también debe ser seguro, ya que no queremos que los datos, las cookies y las credenciales de los usuarios que se conectan a Gerrit sean espiados, capturados o modificados por nadie en tránsito.

Si no hay requisitos para usar un servidor HTTP/S externo, la opción más fácil es reutilizar el interno proporcionado por Gerrit. 

1- Para habilitar la seguridad SSL en HTTP, debemos detener a Gerrit y reiniciar el proceso de inicio:

```
service gerrit stop
```

2- Ejecutar nuevamente la configuración inicial de Gerrit, utilizando el usuario gerrit

```
cd /opt/gerrit
export GERRIT_VERSION=3.1.3
java -jar gerrit-$GERRIT_VERSION.war init
```

Entre los parámetros de HTTP 

```
*** HTTP Daemon
*** 

Behind reverse proxy           [y/N]? N
Use SSL (https://)             [y/N]? Y
Listen on address              [*]: 
Listen on port                 [8080]: 8443
Canonical URL                  [https://host.domain.com:8443/]: https://3.88.13.175:8443/ 
Create new self-signed SSL certificate [Y/n]? Y
Certificate server name        [host.domain.com:8443]: 
Certificate expires in (days)  [365]:
```


3- Iniciamos el servicio de Gerrit como root

```
service gerrit start
```

4- Abrimos nuestro Canonical URL (Ej: `https://3.88.13.175:8443/ `) en nuestro navegador.

Cuando abrimos Gerrit usando la URL HTTPS (https://host.domain.com:8443) recibiremos una advertencia del navegador sobre la identidad del Certificado X.509; Chrome muestra el siguiente mensaje de advertencia: `No se ha verificado la identidad de este sitio web: el certificado del servidor no es de confianza`. 
Esto se espera para un certificado X.509 autofirmado.

Esto puede dependiendo de la configuración del navegador ignorarse (para terminos educativos), dentro del mensaje de `Warning` desplegado del navegador presionamos el botón de `Advanced`. El cual nos muestra el mensaje del error en el certificado, y a este nivel podemos aceptar el riesgo (en esta ocasión) presionando el botón `Accept the Risk and Continue`.

5- A este nivel deberiamos tener la posibilidad de ejecutar todas las acciones anteriores dentro de Gerrit, esta vez bajo `HTTPS`.

# Clone usando HTTPS

El mismo problema puede ocurrir cuando usamos `Git/HTTPS` para clonar los repositorios alojados en Gerrit.

1- Utilizando la interfaz web y nuestro usuario Administrador OpenSSO.

2- Damos click sobre `Browse`-> `Repositories`

3- Volver a la página principal del repositorio `first-project`.

4- Sobre la opción `ANONYMOUS HTTP`. Copiamos el comando que esta bajo la opcíon `Clone`.

Similar a

```
git clone "https://3.88.13.175:8443/first-repo"
```

5- Utilizando el usuario ubuntu, u otra máquina virtual con conexión a nuestra instancia de Gerrit. Ejecutamos el comando que copiamos anteriormente.

```
ubuntu@ip-172-31-20-48:~$ git clone "https://3.88.13.175:8443/first-repo"
Cloning into 'first-repo'...
fatal: unable to access 'https://3.88.13.175:8443/first-repo/': server certificate verification failed. CAfile: none CRLfile: none
```

Como era esperado obtenemos un error por la verificación del certificado `Self-Sign`.

6- Una solución temporal a esto es relajar la validación del certificado X.509 estableciendo http.sslVerify en falso en el archivo de configuración de Git. Para esto ejecutamos el siguiente commando:

```
git config --global http.sslVerify false
```

7- Nuevamente ejecutamos el comando de clonar el repositorio.

```
git clone "https://3.88.13.175:8443/first-repo"
Cloning into 'first-repo'...
remote: Counting objects: 2, done
remote: Finding sources: 100% (2/2)
Unpacking objects: 100% (2/2), 195 bytes | 195.00 KiB/s, done.
remote: Total 2 (delta 0), reused 0 (delta 0)
```

**Nunca utilice esta solución alternativa para una instalación de producción. Eliminar la validación del certificado X.509 presenta el riesgo de ataques de "man-in-the-middle".**




