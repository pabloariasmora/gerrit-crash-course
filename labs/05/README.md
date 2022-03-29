# Base de datos de Gerrit 3.X 

NoteDb es el único backend de almacenamiento para metadatos de revisión de código. 
Se basa en Git, por lo que las revisiones de código se almacenan junto con el código que se está revisando.
NoteDb reemplazó el servidor SQL tradicional para los metadatos de cambio, cuenta y grupo que se usaba en la serie 2.x.

Ventajas
Simplicidad: todos los datos se almacenan en una ubicación en el directorio del sitio, en lugar de dividirse entre el directorio del sitio y un servidor de base de datos posiblemente externo.

Coherencia: la replicación y las copias de seguridad pueden usar una instantánea de las referencias del repositorio de Git, que incluirá las referencias de la rama y del conjunto de parches, y los metadatos de cambio que apuntan a ellas.

Auditabilidad: en lugar de almacenar filas mutables en una base de datos, las modificaciones a los cambios se almacenan como una secuencia de confirmaciones de Git, preservando automáticamente el historial de los metadatos.
No hay garantías estrictas y las referencias meta pueden reescribirse, pero la suposición predeterminada es que se registran todas las operaciones.

Extensibilidad: los desarrolladores de complementos pueden agregar nuevos campos a los metadatos sin que el esquema de la base de datos central tenga que conocerlos.

Nuevas funciones: permite la federación simple entre servidores Gerrit, así como la revisión de código fuera de línea y la interoperabilidad con otras herramientas.
Instalación con Base de Datos Postgres

