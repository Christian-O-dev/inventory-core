# Skill: Reglas de arquitectura

## Cuándo usar esta skill

Usar cuando una tarea afecte arquitectura, estructura de carpetas, división de módulos, backend, frontend, base de datos, dependencias, deploy o escalabilidad.

## Reglas principales

1. InventoryCore empezará como monolito modular, no como microservicios.
2. Frontend, backend y base de datos deben estar separados.
3. La lógica crítica de negocio debe vivir en el backend.
4. El frontend no debe ser la única capa que valida reglas importantes.
5. La base de datos principal será PostgreSQL.
6. Los módulos deben estar separados por responsabilidad.
7. No mezclar lógica de ventas, compras e inventario en archivos genéricos sin criterio.
8. No crear abstracciones grandes sin necesidad real.
9. No diseñar solo para tienda pequeña si una decisión simple puede preparar el sistema para crecer.
10. No implementar funciones tipo Amazon antes del MVP.

## Módulos iniciales esperados

- auth
- users
- roles
- products
- categories
- skus
- suppliers
- purchases
- sales
- inventory
- reports
- audit

## Regla de crecimiento

El proyecto debe permitir crecer hacia múltiples tiendas y almacenes, pero sin implementar toda la complejidad desde el primer día.

## Regla para decisiones difíciles

Cuando haya conflicto entre rapidez y consistencia del inventario, priorizar consistencia, trazabilidad y seguridad.
