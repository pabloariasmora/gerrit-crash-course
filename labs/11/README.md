# Crear una herencia de proyectos

1- Abrimos nuestro Canonical URL (Ej: `https://3.88.13.175:8443/ `) en nuestro navegador.

2- Iniciamos sesión con nuestro usuario con Role Administrator.

3- En la parte superior del navegador seleccionamos `BROWSE`

4- Seleccionamos la opción `REPOSITORIES`

5- Damos click sobre la opción `CREATE NEW` a la derecha.

6- En la ventana emergente utilizamos la siguiente información:

`Repository name:`: `OpenSource-Projects`

`Righs inherit from`: `All-Projects`

`Owner`: `Administrators`

`Create initial empty commit`: `False`

`Only serve as paren for other repositories`: `True`

7- Presionamos `CREATE`

8- El projecto anterior es hijo de `All-Projects`

9- Seleccionamos la opción `REPOSITORIES` a la derecha

10- Damos click sobre la opción `CREATE NEW` a la derecha.

11- En la ventana emergente utilizamos la siguiente información:

`Repository name:`: `Gerrit`

`Righs inherit from`: `OpenSource-Projects`

`Owner`: `Administrators`

`Create initial empty commit`: `True`

`Only serve as paren for other repositories`: `False`

12- Presionamos `CREATE`

13- El projecto anterior es hijo de `OpenSource-Projects`

14- Seleccionamos la opción `REPOSITORIES` a la derecha

15- Damos click sobre la opción `CREATE NEW` a la derecha.

16- En la ventana emergente utilizamos la siguiente información:

`Repository name:`: `JGit`

`Righs inherit from`: `OpenSource-Projects`

`Owner`: `Administrators`

`Create initial empty commit`: `True`

`Only serve as paren for other repositories`: `False`

17- Presionamos `CREATE`

18- El projecto anterior es hijo de `OpenSource-Projects`

19- Seleccionamos la opción `REPOSITORIES` a la derecha

20- Damos click sobre la opción `CREATE NEW` a la derecha.

21- En la ventana emergente utilizamos la siguiente información:

`Repository name:`: `Internal-Projects`

`Righs inherit from`: `All-Projects`

`Owner`: `Administrators`

`Create initial empty commit`: `False`

`Only serve as paren for other repositories`: `True`

22- Presionamos `CREATE`

23- El projecto anterior es hijo de `All-Projects`

24- Seleccionamos la opción `REPOSITORIES` a la derecha

25- Damos click sobre la opción `CREATE NEW` a la derecha.

26- En la ventana emergente utilizamos la siguiente información:

`Repository name:`: `OpenReview-Projects`

`Righs inherit from`: `Internal-Projects`

`Owner`: `Administrators`

`Create initial empty commit`: `False`

`Only serve as paren for other repositories`: `True`

27- Presionamos `CREATE`

28- El projecto anterior es hijo de `Internal-Projects`

29- Seleccionamos la opción `REPOSITORIES` a la derecha

30- Damos click sobre la opción `CREATE NEW` a la derecha.

31- En la ventana emergente utilizamos la siguiente información:

`Repository name:`: `TopSecret-Projects`

`Righs inherit from`: `Internal-Projects`

`Owner`: `Administrators`

`Create initial empty commit`: `False`

`Only serve as paren for other repositories`: `True`

32- Presionamos `CREATE`

33- El projecto anterior es hijo de `Internal-Projects`

34- Seleccionamos la opción `REPOSITORIES` a la derecha

35- Damos click sobre la opción `CREATE NEW` a la derecha.

36- En la ventana emergente utilizamos la siguiente información:

`Repository name:`: `Projects-X`

`Righs inherit from`: `OpenReview-Projects`

`Owner`: `Administrators`

`Create initial empty commit`: `True`

`Only serve as paren for other repositories`: `False`

37- Presionamos `CREATE`

38- El projecto anterior es hijo de `OpenReview-Projects`

39- Seleccionamos la opción `REPOSITORIES` a la derecha

40- Damos click sobre la opción `CREATE NEW` a la derecha.

41- En la ventana emergente utilizamos la siguiente información:

`Repository name:`: `Projects-Y`

`Righs inherit from`: `TopSecret-Projects`

`Owner`: `Administrators`

`Create initial empty commit`: `True`

`Only serve as paren for other repositories`: `False`

42- Presionamos `CREATE`

43- El projecto anterior es hijo de `TopSecret-Projects`
