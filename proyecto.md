## Prompt en Claude
Actúa como un administrador de base de datos: proyecto empresa de Venta de Télefonos, que entidades se necesitan para su gestión
muestrame graficamente las tablas con todo y sus relaciones
Las entidades con sus atributos y tipos de forma de tabla
crear un script en SQL

## Respuesta
Perfecto. Voy a crear el diagrama ERD con mermaid.js y las tablas de atributos, más el script SQL completo.Ahora las tablas de atributos con tipos de datos:Ahora el script SQL completo:Aquí tienes el sistema completo. Un resumen de las **12 entidades** diseñadas y su lógica:
<img width="1029" height="556" alt="image" src="https://github.com/user-attachments/assets/e45172f3-3469-489c-a8c8-e41ab27e67bf" />
<img width="750" height="669" alt="image" src="https://github.com/user-attachments/assets/41a004f2-11ab-46e3-8f21-07970038e0b9" />
<img width="747" height="735" alt="image" src="https://github.com/user-attachments/assets/09a47b51-9251-438e-a305-7d170ade8069" />
<img width="760" height="297" alt="image" src="https://github.com/user-attachments/assets/8fa8bd83-d9a3-415e-a798-f947c0ef5f32" />


**Catálogo del producto**
- `categoria` y `marca` clasifican cada teléfono. `telefono` centraliza el inventario con precio y stock.
- `garantia` tiene relación 1-a-1 con teléfono (cada modelo tiene sus propios términos).

**Ciclo de venta**
- `cliente` genera un `pedido`, atendido por un `empleado`.
- `detalle_pedido` es la tabla puente que permite vender varios modelos en un mismo pedido.
- `devolucion` registra devoluciones contra un pedido específico.

**Abastecimiento**
- `proveedor` alimenta el inventario mediante `compra_proveedor`, que un empleado gestiona.
- `detalle_compra` registra qué modelos y a qué costo llegaron.

El script incluye además **índices** para consultas frecuentes (estado de pedidos, fecha, modelo) y **3 vistas** listas para usar: ventas por empleado, inventario actual y detalle completo de pedidos.
