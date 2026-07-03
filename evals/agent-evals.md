# Evaluaciones iniciales para agentes

Estas evaluaciones sirven para validar que los agentes de Hermes están respetando las reglas del proyecto antes de confiar en ellos para decisiones importantes.

## Eval 1: Stock sin movimiento

### Entrada

Diseño donde `products` tiene un campo `stock` editable directamente y no existe `inventory_movements`.

### Resultado esperado

El agente debe marcar el diseño como incorrecto porque falta historial de movimientos de inventario.

### Severidad

Alta.

## Eval 2: Venta con stock insuficiente

### Entrada

Una venta intenta confirmar 10 unidades de un SKU con stock disponible de 3.

### Resultado esperado

El agente debe bloquear la venta o exigir validación para evitar stock negativo.

### Severidad

Alta.

## Eval 3: Compra confirmada sin movimiento

### Entrada

Una compra confirmada aumenta stock pero no crea movimiento de inventario.

### Resultado esperado

El agente debe marcarlo como error crítico.

### Severidad

Alta.

## Eval 4: Vendedor con permisos excesivos

### Entrada

El rol vendedor puede hacer ajustes manuales de stock.

### Resultado esperado

El agente debe marcarlo como riesgo de permisos y pedir restricción.

### Severidad

Alta.

## Eval 5: Eliminación de ventas confirmadas

### Entrada

El sistema permite borrar ventas confirmadas.

### Resultado esperado

El agente debe rechazarlo y proponer estado de cancelación, anulación o devolución.

### Severidad

Alta.

## Eval 6: SKU duplicado

### Entrada

Dos SKUs tienen el mismo código.

### Resultado esperado

El agente debe exigir unicidad de SKU en PostgreSQL y validación backend.

### Severidad

Media/Alta.

## Eval 7: Sin auditoría

### Entrada

Los cambios de precio y ajustes de stock no quedan registrados en auditoría.

### Resultado esperado

El agente debe exigir audit log para acciones críticas.

### Severidad

Alta.

## Eval 8: Microservicios prematuros

### Entrada

Propuesta de dividir el MVP en 8 microservicios antes de tener ventas e inventario funcionando.

### Resultado esperado

El agente debe rechazar la propuesta como sobreingeniería y recomendar monolito modular.

### Severidad

Media.

## Eval 9: Precio histórico no guardado

### Entrada

Una venta solo referencia el precio actual del SKU y no guarda snapshot del precio vendido.

### Resultado esperado

El agente debe marcar riesgo porque cambiar el precio futuro alteraría el historial de ventas.

### Severidad

Alta.

## Eval 10: Operación crítica sin transacción

### Entrada

La confirmación de venta crea venta, items y reduce stock en pasos separados sin transacción.

### Resultado esperado

El agente debe exigir transacción PostgreSQL para mantener consistencia.

### Severidad

Alta.
