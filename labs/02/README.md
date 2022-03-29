Guia para generar las SSH-Keys
Introducción
Este contenido explica cómo generar las llaves necesarias (SSH-Keys) para utilizar en Gerrit.

Requerimientos
======

* Un maquina con el sistema operativo Ubuntu, 20.04 LTS, Focal Fossa previamente instalado.
* Coneccion de red al servidor donde se encuentre instalado Gerrit.

Pasos por seguir
======

1- Iniciar sesión dentro del servidor. Para esto abra una ventana de terminal.
2- Actualizar los repositorios de Paquetes dentro de Ubuntu.

```
sudo apt-get update && sudo apt-get upgrade -y
```

3- Utilizando el usuario por defecto en la maquina en este (ej: ubuntu)

```
ubuntu@ip-172-31-18-186:~$ ssh-keygen -t rsa -b 4096
```

Ejemplo de salida del comando

```
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ubuntu/.ssh/id_rsa
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:eY9pRUstOd21CF7GyyuuBP0FTx4I1b2QoV64UAWbHSI ubuntu@ip-172-31-18-186
The key's randomart image is:
+---[RSA 4096]----+
|         E.*=*+ .|
|          = %Oooo|
|         . OB*=.o|
|        ..ooB*.. |
|       .S..oo+.  |
|        ...*..   |
|         .=.o    |
|        .. .     |
|         ..      |
+----[SHA256]-----+
```


4- Obtenemos la llave publica

```
cat ~/.ssh/id_rsa.pub
```

5- Copiamos la llave en un archivo de texto en nuestro computador.
