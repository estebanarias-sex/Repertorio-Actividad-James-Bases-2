# Repertorio-Actividad-James-Bases-2

# AutoFactory --- Procedimientos almacenados y Views

## Descripción general

AutoFactory es un sistema de gestión para una industria automotriz. La
base de datos administra modelos de vehículos, líneas de producción,
estaciones de trabajo, proveedores, componentes, inventario, vehículos,
empleados, asignaciones de personal, controles de calidad y órdenes de
compra.

Esta entrega corresponde a la actividad de **Conocimiento ASP-03-05
Profundización y aplicaciones en bases de datos**, específicamente al
**Sistema 7: AutoFactory**.

El objetivo es implementar procedimientos almacenados y vistas que
permitan gestionar y consultar la información de producción, inventario,
calidad, personal y proveedores.

## Alcance

La actividad indica que la base de datos ya fue creada en el módulo
anterior, por lo que este proyecto no reemplaza ni reconstruye las
tablas originales.

La estructura original no contenía entidades para almacenar órdenes de
producción ni la relación entre una orden y los componentes utilizados.
Por esa razón se agregan únicamente dos tablas de apoyo:

-   `orden_produccion`
-   `orden_componente`

Estas tablas permiten implementar correctamente los dos primeros
procedimientos solicitados.

## Procedimientos almacenados

### 1. `CrearOrdenProduccion`

Crea una orden de producción para un modelo específico.

Valida:

-   Que el modelo exista.
-   Que la línea exista.
-   Que el tipo de vehículo del modelo corresponda con la línea.
-   Que la línea se encuentre operativa.
-   Que la cantidad sea mayor que cero.

### 2. `AsignarComponentesProduccion`

Asigna a una orden los componentes necesarios de acuerdo con la tabla
`modelo_componente`.

Valida:

-   Que la orden exista.
-   Que los componentes no hayan sido asignados previamente.
-   Que el modelo tenga componentes configurados.
-   Que exista inventario suficiente.

Al realizar la asignación, descuenta del inventario las cantidades
utilizadas y cambia la orden a estado `En proceso`.

### 3. `RegistrarControlCalidad`

Registra una inspección de calidad para un vehículo.

Valida la existencia del vehículo, estación e inspector, además de
comprobar que el empleado tenga un cargo de inspector y que el resultado
sea válido.

### 4. `GestionarCompraComponentes`

Busca componentes cuyo inventario se encuentre por debajo del stock
mínimo y genera automáticamente una orden de compra por la cantidad
faltante.

### 5. `AsignarPersonalLinea`

Asigna un empleado a una estación de trabajo.

Verifica que el empleado sea operario y que sus habilidades sean
compatibles con el tipo de trabajo de la estación.

## Vistas

### `V_EstadoLineasProduccion`

Muestra el estado de las líneas de producción, estaciones registradas,
operarios activos y vehículos asociados.

### `V_InventarioComponentes`

Presenta los componentes junto con proveedor, cantidad disponible, stock
mínimo, estado del inventario, ubicación, calidad, costo y tiempo de
entrega.

### `V_OrdenesProduccionActivas`

Lista las órdenes de producción que se encuentran pendientes o en
proceso.

### `V_EstadisticasCalidad`

Presenta estadísticas de inspecciones y defectos agrupadas por modelo,
estación y resultado de calidad.

### `V_RendimientoProveedores`

Permite analizar proveedores teniendo en cuenta componentes
suministrados, tiempo promedio de entrega, precio promedio, calidad del
inventario y órdenes de compra.

## Instrucciones de uso

1.  Abrir MySQL Workbench.
2.  Ejecutar primero el script original de la base de datos
    `auto_factory`, desarrollado en el módulo anterior.
3.  Seleccionar la base de datos:

``` sql
USE auto_factory;
```

4.  Ejecutar el archivo:

`AutoFactory_procedimientos_vistas.sql`

5.  Verificar las vistas:

``` sql
SHOW FULL TABLES WHERE TABLE_TYPE = 'VIEW';
```

6.  Consultar las vistas:

``` sql
SELECT * FROM V_EstadoLineasProduccion;
SELECT * FROM V_InventarioComponentes;
SELECT * FROM V_OrdenesProduccionActivas;
SELECT * FROM V_EstadisticasCalidad;
SELECT * FROM V_RendimientoProveedores;
```

7.  Para probar los procedimientos, utilizar los `CALL` que aparecen
    comentados al final del script. Se recomienda ejecutarlos de manera
    controlada porque algunos procedimientos modifican inventario o
    agregan registros.

## Estructura del proyecto

``` text
AutoFactory/
├── AutoFactory_procedimientos_vistas.sql
└── README.md
```

## Tecnologías

-   MySQL
-   MySQL Workbench
-   SQL
-   Stored Procedures
-   Views
-   Foreign Keys
-   Transactions

## Resultado

La solución implementa los cinco procedimientos almacenados y las cinco
vistas solicitadas para el Sistema 7: AutoFactory, manteniendo como base
el modelo de datos desarrollado previamente y agregando solamente las
estructuras de apoyo indispensables para representar las órdenes de
producción.
