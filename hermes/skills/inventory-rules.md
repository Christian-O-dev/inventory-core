# Skill: Reglas de inventario

## Cuándo usar esta skill

Usar en tareas relacionadas con productos, SKUs, stock, almacenes, compras, ventas, devoluciones, ajustes, transferencias o reportes de inventario.

## Principio central

Todo cambio de stock debe generar un movimiento de inventario.

No aceptar diseños donde el stock se modifica directamente sin historial.

## Conceptos

### Producto

Artículo general, como “Camiseta básica”.

### SKU

Unidad vendible específica, como “Camiseta básica negra talla M”.

### Stock actual

Cantidad disponible de un SKU en una ubicación.

### Stock reservado

Cantidad separada para pedidos o ventas pendientes. Puede quedar para una fase futura, pero el diseño no debe impedirlo.

### Almacén

Ubicación física o lógica donde existe stock. En el MVP puede haber solo un almacén principal.

### Movimiento de inventario

Registro histórico de una entrada, salida, ajuste, devolución o transferencia.

## Tipos de movimientos iniciales

- purchase_in
- sale_out
- manual_adjustment
- return_in
- return_out
- damaged_out
- lost_out
- transfer_in
- transfer_out
- cancellation_reversal

No todos se implementan en MVP, pero el diseño debe permitirlos.

## Reglas obligatorias

1. No vender si no hay stock suficiente, salvo configuración futura de preventa.
2. No permitir stock negativo por error.
3. Toda compra confirmada aumenta stock y genera movimiento.
4. Toda venta confirmada reduce stock y genera movimiento.
5. Todo ajuste manual requiere motivo.
6. Todo movimiento debe registrar usuario, fecha, SKU, cantidad, tipo y motivo/referencia.
7. El SKU debe ser único.
8. El stock se calcula o se mantiene en una tabla controlada, pero nunca sin historial de movimientos.
9. No borrar movimientos de inventario.
10. Las correcciones deben hacerse con movimientos compensatorios.

## Riesgos a bloquear

- Campo `stock` editable manualmente sin auditoría.
- Ventas confirmadas sin reducir stock.
- Compras confirmadas sin aumentar stock.
- Movimiento sin usuario.
- Movimiento sin causa.
- SKU duplicado.
- Stock negativo accidental.
