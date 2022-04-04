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

## Abandonar un `Change`

Algunas veces los `Changes` son inadecuados o obsoletos, y el equipo de trabajo decide ya sea iniciar nuevamente desde otra perspectiva, o incluso solamente son descartados.

Estos `Changes` pueden ser cerrados al marcar el mismo como `Abandon`.

1- Para esto darle click al botón en la ventana de `Changes`.

2- Agregamos la razón para nuestra decisión.

`Abandon Message`: `Understanding Abandon Operation`

3- Presionamos el botón `Abandon`

4- Vemos como el estado de nuestro `Change` ahora esta marcado como `Abandoned`.

Todo `Change` marcado como `Abandoned` puede ser restaurado luego si es requerido.

## Restaurar un `Change`

Los `Changes` marcados como `Abandoned` se mantienen con sus detalles, se pueden buscar y restaurar, si es necesario, 

1- Para restaurar un `Changes Abandoned`, navegue hasta el `Change` y haga clic en el botón `Restore`.

2- El estado del `Change` ahora debe ser `Active`, y su historial debe tener una entrada con nuestra decisión de restarurarlo.



