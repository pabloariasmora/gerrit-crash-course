# Usando la GUI de Gerrit para Revertir/Abandonar/Restaurar `Changes`

## Revertir un `Change`

1- Utilizando la interfaz web y nuestro usuario Administrador OpenSSO.

2- Damos click sobre `Changes`-> `Merged`

3- Seleccionamos el último `change` que trabajamos en el laboratorio anterior.
`Initial commit branch-b`

Una vez que el `Change` a sido `Submit` en su `branch` destino es posible revertir el cambio.

Los `Changes` son revertidos creando un nuevo `Change`.

4- Para revertir este `Change`, damos click sobre el botón `Revert`.

El boton `Revert` solamente aparece sobre `Changes` que han sido `Merged`

5- Editamos el mensaje de `commit`, en el cual nos solicita una razón para realizar el `Revert`.

6- Reemplazamos `<INSERT REASONING HERE>` por `Test Revert Feature`.

Similar a :
```
Revert "Initial commit branch-b"
This reverts commit c38ead289086f9aac8673e8d770ddd2b88c38a4d.
Reason for revert: Test Revert Feature
```

7- Click sobre `REVERT`, esto abrirá a un nuevo cambio. En caso de tener reviewers en el `Change` previo, estos son también agregados en este nuevo `Change`.

8- Si vamos a la ventana anterior del `Change`-`Merged`, vamos a poder visualizar en el historial la solicitud del `Revert`.
Ambos `Changes` van a estar asociados por historial.

```
Created a revert of this change as If3d6081b688066f3172c466f35f8d7837f3bf48c
```

9- Podemos ver en la lista de cambios como se hace una `revert` sobre el archivo `my_code.sh`. Esto si damos click sobre `EXPAND ALL`.

10- Este nuevo `Change` deberá seguir los pasos completos de aprobación. En este momento no vamos a realizarlos para poder seguir con el laboratorio.







