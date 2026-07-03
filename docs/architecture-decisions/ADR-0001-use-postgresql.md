# ADR-0001: Usar PostgreSQL como base de datos principal

## Estado

Aceptado.

## Contexto

InventoryCore necesita una base de datos relacional sólida para manejar inventario, ventas, compras, usuarios, roles, permisos, auditoría y reportes.

El sistema debe empezar como MVP, pero debe estar preparado para crecer hacia múltiples tiendas, almacenes, reportes complejos e integraciones futuras.

## Decisión

Usaremos **PostgreSQL** como base de datos principal del proyecto.

## Motivos

- Excelente integridad relacional.
- Buen soporte de transacciones para ventas, compras y movimientos de inventario.
- Constraints más robustos para proteger reglas críticas.
- Índices avanzados para búsquedas y reportes.
- Buen rendimiento en consultas complejas.
- Mejor base para escalar hacia una plataforma empresarial.
- Ecosistema maduro para Node.js, ORMs, migraciones y despliegues cloud.

## Consecuencias

- Las reglas de datos se diseñarán pensando en PostgreSQL.
- Las migraciones deberán ser compatibles con PostgreSQL.
- Se podrá usar Docker local con imagen oficial de PostgreSQL.
- Se evitarán decisiones muy acopladas a MySQL/MariaDB.
- Si más adelante se usa Prisma, Drizzle o Sequelize, debe configurarse para PostgreSQL desde el inicio.

## Riesgos

- Si el desarrollador está más acostumbrado a MySQL, habrá curva de aprendizaje.
- Algunas consultas y tipos cambian respecto a MySQL.
- Los despliegues gratuitos pueden tener límites, por lo que se debe elegir proveedor con PostgreSQL disponible.

## Regla práctica

Toda decisión de diseño de base de datos debe priorizar integridad, trazabilidad y consistencia del inventario antes que velocidad de implementación.
