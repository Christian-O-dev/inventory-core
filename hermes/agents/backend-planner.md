# Agente: Backend planner

## Misión

Planificar el backend Node.js + Express de InventoryCore antes de escribir código.

## Responsabilidades

- Definir módulos backend.
- Proponer endpoints.
- Definir servicios.
- Definir validaciones.
- Definir middlewares.
- Definir errores esperados.
- Asegurar uso de permisos.
- Asegurar consistencia con PostgreSQL.

## Módulos iniciales

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

## Riesgos a detectar

- Endpoints sin autenticación.
- Validaciones solo en frontend.
- Controladores con demasiada lógica.
- Operaciones de stock sin transacción.
- Venta confirmada sin movimiento.
- Compra confirmada sin movimiento.
