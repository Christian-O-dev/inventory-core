# Skill: Reglas de escalabilidad

## Cuándo usar esta skill

Usar cuando una tarea afecte decisiones futuras, crecimiento del sistema, múltiples almacenes, múltiples tiendas, reportes, rendimiento, integración con ecommerce o diseño de módulos.

## Principios

1. Empezar simple, pero no bloquear crecimiento futuro.
2. Preparar el diseño para múltiples almacenes, aunque el MVP use uno.
3. Preparar el diseño para múltiples tiendas, aunque el MVP use una.
4. No implementar microservicios al inicio.
5. No crear un marketplace en el MVP.
6. Priorizar integridad del inventario sobre velocidad superficial.
7. Diseñar IDs, timestamps y estados con visión de largo plazo.
8. Pensar en auditoría desde el inicio.
9. Evitar tablas sin historial para procesos críticos.
10. Evitar dependencias rígidas entre módulos.

## Decisiones recomendadas

- Usar PostgreSQL.
- Usar transacciones en operaciones críticas.
- Usar índices en campos de búsqueda frecuentes.
- Mantener snapshots de precios en ventas y compras.
- Mantener movimientos de inventario inmutables.
- Usar estados para compras, ventas y usuarios.

## Señales de sobreingeniería

- Crear microservicios sin necesidad.
- Crear colas/eventos antes de tener flujos básicos.
- Implementar multiempresa antes del MVP.
- Implementar predicción IA antes de reportes básicos.
- Hacer arquitectura tipo Amazon sin ventas simples funcionando.
