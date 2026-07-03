# Skill: Reglas de ventas

## Cuándo usar esta skill

Usar cuando la tarea afecte ventas, carrito, clientes, pagos, descuentos, devoluciones, cancelaciones o reducción de stock.

## Concepto de venta

Una venta representa una salida comercial de productos hacia un cliente. Una venta confirmada debe reducir stock y generar movimientos de inventario.

## Estados recomendados

- draft
- pending
- confirmed
- cancelled
- refunded
- partially_refunded

El MVP puede empezar con menos estados, pero no debe depender de borrar ventas.

## Reglas obligatorias

1. Una venta debe tener al menos un item.
2. Cada item debe referenciar un SKU.
3. La cantidad vendida debe ser mayor a cero.
4. El sistema debe validar stock antes de confirmar.
5. Una venta confirmada reduce stock.
6. Una venta confirmada genera movimientos de inventario.
7. Una venta confirmada no debe borrarse físicamente.
8. Una cancelación debe quedar registrada.
9. Los precios usados en la venta deben guardarse como snapshot para no cambiar ventas antiguas si cambia el precio del SKU.
10. Los descuentos deben quedar explícitos.
11. El vendedor no debe poder cambiar precios críticos sin permiso.

## Reglas de pagos iniciales

El MVP puede registrar método de pago básico:

- efectivo
- tarjeta
- transferencia
- otro

Los pagos avanzados quedan para fases posteriores.

## Riesgos a bloquear

- Venta sin items.
- Venta con SKU inexistente.
- Venta con stock insuficiente.
- Venta confirmada sin movimiento.
- Borrado físico de ventas confirmadas.
- Modificación silenciosa de precios históricos.
