# Backend Modules v0.1

## 1. Objetivo del documento

Este documento define la primera planificación del backend para InventoryCore.

Todavía no es código. Su objetivo es decidir módulos, responsabilidades, endpoints candidatos, permisos y validaciones antes de crear la estructura real del backend con Node.js, Express y PostgreSQL.

El backend debe construirse como un monolito modular: un solo backend al inicio, pero organizado por dominios para poder crecer sin convertirse en un bloque desordenado.

---

## 2. Stack backend decidido

- Runtime: Node.js.
- Framework HTTP: Express.
- Base de datos: PostgreSQL.
- API: REST.
- Documentación: OpenAPI / Swagger.
- Autenticación: JWT.
- Autorización: roles y permisos.
- Arquitectura inicial: monolito modular.

---

## 3. Principios backend

1. Los controladores no deben contener lógica de negocio compleja.
2. La lógica de negocio debe vivir en servicios.
3. El acceso a datos debe estar separado de controladores.
4. Toda ruta crítica debe validar autenticación.
5. Toda acción sensible debe validar permisos.
6. Las validaciones de entrada deben ejecutarse antes de tocar la base de datos.
7. Todo cambio de stock debe pasar por el módulo de inventario.
8. Las ventas no deben modificar stock directamente sin generar movimiento.
9. Las compras no deben modificar stock directamente sin generar movimiento.
10. Los errores deben ser claros pero no filtrar información sensible.
11. Las operaciones críticas deben registrar auditoría.
12. El backend no debe depender de reglas críticas ejecutadas solo en frontend.

---

## 4. Estructura modular candidata

Módulos iniciales:

1. auth
2. users
3. roles-permissions
4. organizations
5. stores
6. warehouses
7. categories
8. products
9. skus
10. suppliers
11. customers
12. inventory
13. purchases
14. sales
15. payments
16. reports
17. audit
18. health

No todos los módulos necesitan interfaz completa en el MVP, pero deben tener límites claros.

---

## 5. Módulo: auth

### Responsabilidad

Gestionar autenticación y sesión lógica.

### Funciones

- Login.
- Validación de credenciales.
- Emisión de token JWT.
- Lectura del usuario autenticado.
- Protección de rutas privadas.

### Endpoints candidatos

- POST /auth/login
- GET /auth/me
- POST /auth/logout opcional, si se maneja lista de tokens o cierre lógico

### Validaciones

- Email obligatorio.
- Password obligatorio.
- Usuario debe estar activo.
- Password nunca se devuelve en respuestas.

### Permisos

- Login es público.
- /auth/me requiere usuario autenticado.

### Riesgos

- Guardar password en texto plano.
- No validar usuario activo.
- Usar JWT sin expiración.
- Exponer demasiada información en errores de login.

---

## 6. Módulo: users

### Responsabilidad

Gestionar usuarios internos del sistema.

### Funciones

- Crear usuarios.
- Editar usuarios.
- Desactivar usuarios.
- Consultar usuarios.
- Asignar roles.

### Endpoints candidatos

- GET /users
- GET /users/:id
- POST /users
- PATCH /users/:id
- PATCH /users/:id/status
- POST /users/:id/roles

### Validaciones

- Email único por organización.
- Password requerido al crear usuario.
- Role válido.
- No permitir que un vendedor gestione usuarios.

### Permisos candidatos

- user.view
- user.create
- user.update
- user.disable
- user.manage_roles

### Auditoría

Registrar:

- Creación de usuario.
- Cambio de rol.
- Desactivación de usuario.
- Cambio de email.

---

## 7. Módulo: roles-permissions

### Responsabilidad

Gestionar roles y permisos del sistema.

### Funciones

- Listar roles.
- Listar permisos.
- Asignar permisos a roles.
- Permitir roles personalizados en futuras versiones.

### Endpoints candidatos

- GET /roles
- GET /permissions
- GET /roles/:id/permissions
- PUT /roles/:id/permissions

### Validaciones

- No eliminar roles de sistema.
- No dejar al sistema sin administrador.
- No permitir cambios de permisos sin permiso administrativo.

### Permisos candidatos

- role.view
- role.manage
- permission.view
- permission.manage

---

## 8. Módulo: organizations

### Responsabilidad

Representar la empresa, tienda o tenant.

### Funciones MVP

- Crear organización inicial.
- Consultar datos de organización.
- Actualizar datos básicos.

### Endpoints candidatos

- GET /organization
- PATCH /organization

### Validaciones

- Nombre obligatorio.
- Estado activo para operar.

### Permisos candidatos

- organization.view
- organization.update

### Nota

En el MVP puede existir una sola organización visible, pero el modelo debe estar preparado para multiempresa.

---

## 9. Módulo: stores

### Responsabilidad

Gestionar tiendas o sucursales comerciales.

### Funciones MVP

- Crear tienda principal.
- Consultar tiendas.
- Editar tienda.

### Endpoints candidatos

- GET /stores
- GET /stores/:id
- POST /stores
- PATCH /stores/:id
- PATCH /stores/:id/status

### Permisos candidatos

- store.view
- store.create
- store.update
- store.disable

### Nota

En MVP puede existir solo una tienda principal.

---

## 10. Módulo: warehouses

### Responsabilidad

Gestionar almacenes físicos o lógicos donde existe stock.

### Funciones

- Crear almacén.
- Editar almacén.
- Consultar almacenes.
- Desactivar almacén.

### Endpoints candidatos

- GET /warehouses
- GET /warehouses/:id
- POST /warehouses
- PATCH /warehouses/:id
- PATCH /warehouses/:id/status

### Validaciones

- No desactivar almacén con stock activo sin revisión.
- Código de almacén único por organización.

### Permisos candidatos

- warehouse.view
- warehouse.create
- warehouse.update
- warehouse.disable

---

## 11. Módulo: categories

### Responsabilidad

Gestionar categorías de productos.

### Funciones

- Crear categoría.
- Editar categoría.
- Listar categorías.
- Desactivar categoría.

### Endpoints candidatos

- GET /categories
- GET /categories/:id
- POST /categories
- PATCH /categories/:id
- PATCH /categories/:id/status

### Validaciones

- Nombre obligatorio.
- Slug único por organización si se usa.
- No borrar físicamente categorías con productos asociados.

### Permisos candidatos

- category.view
- category.create
- category.update
- category.disable

---

## 12. Módulo: products

### Responsabilidad

Gestionar productos base del catálogo.

### Funciones

- Crear producto.
- Editar producto.
- Listar productos.
- Buscar productos.
- Desactivar producto.
- Consultar SKUs del producto.

### Endpoints candidatos

- GET /products
- GET /products/:id
- POST /products
- PATCH /products/:id
- PATCH /products/:id/status
- GET /products/:id/skus

### Validaciones

- Nombre obligatorio.
- Categoría válida si se asigna.
- Producto no debe almacenar stock real directamente.
- No eliminar producto con ventas, compras o movimientos.

### Permisos candidatos

- product.view
- product.create
- product.update
- product.disable

### Auditoría

Registrar:

- Creación.
- Edición.
- Desactivación.

---

## 13. Módulo: skus

### Responsabilidad

Gestionar unidades vendibles reales.

### Funciones

- Crear SKU.
- Editar SKU.
- Listar SKUs.
- Buscar por código SKU o código de barras.
- Consultar stock por SKU.
- Desactivar SKU.

### Endpoints candidatos

- GET /skus
- GET /skus/:id
- POST /skus
- PATCH /skus/:id
- PATCH /skus/:id/status
- GET /skus/:id/stock
- GET /skus/search

### Validaciones

- sku_code obligatorio.
- sku_code único por organización.
- barcode único si existe.
- Precio de venta mayor o igual a cero.
- Costo mayor o igual a cero.
- Stock mínimo mayor o igual a cero.

### Permisos candidatos

- sku.view
- sku.create
- sku.update
- sku.disable
- sku.price_update

### Auditoría

Registrar cambios de precio y costo.

---

## 14. Módulo: suppliers

### Responsabilidad

Gestionar proveedores.

### Funciones

- Crear proveedor.
- Editar proveedor.
- Listar proveedores.
- Ver historial de compras por proveedor.
- Desactivar proveedor.

### Endpoints candidatos

- GET /suppliers
- GET /suppliers/:id
- POST /suppliers
- PATCH /suppliers/:id
- PATCH /suppliers/:id/status
- GET /suppliers/:id/purchases

### Validaciones

- Nombre obligatorio.
- No borrar proveedor con compras asociadas.

### Permisos candidatos

- supplier.view
- supplier.create
- supplier.update
- supplier.disable

---

## 15. Módulo: customers

### Responsabilidad

Gestionar clientes.

### Funciones

- Crear cliente.
- Editar cliente.
- Listar clientes.
- Ver historial de ventas por cliente.
- Desactivar cliente.

### Endpoints candidatos

- GET /customers
- GET /customers/:id
- POST /customers
- PATCH /customers/:id
- PATCH /customers/:id/status
- GET /customers/:id/sales

### Validaciones

- Cliente puede ser opcional en ventas de mostrador.
- Si se usa email o documento, validar formato y posible unicidad por organización.

### Permisos candidatos

- customer.view
- customer.create
- customer.update
- customer.disable

---

## 16. Módulo: inventory

### Responsabilidad

Gestionar stock actual, movimientos y ajustes.

Este es uno de los módulos más críticos del sistema.

### Funciones

- Consultar stock actual.
- Consultar stock por almacén.
- Consultar movimientos.
- Registrar ajuste manual.
- Ver stock bajo.
- Centralizar cambios de inventario generados por compras y ventas.

### Endpoints candidatos

- GET /inventory/stock
- GET /inventory/stock/:skuId
- GET /inventory/movements
- GET /inventory/movements/:id
- POST /inventory/adjustments
- GET /inventory/low-stock

### Validaciones

- No permitir stock negativo en el MVP.
- Todo ajuste manual requiere motivo.
- Todo movimiento requiere SKU, almacén, cantidad, tipo y usuario.
- No permitir editar movimientos históricos libremente.

### Permisos candidatos

- inventory.view
- inventory.movement_view
- inventory.adjust
- inventory.low_stock_view

### Reglas críticas

- Las compras confirmadas deben llamar al servicio de inventario para aumentar stock.
- Las ventas confirmadas deben llamar al servicio de inventario para reducir stock.
- El módulo de inventario debe crear inventory_movements.
- Ningún otro módulo debe cambiar stock sin pasar por las reglas de inventario.

### Auditoría

Registrar:

- Ajustes manuales.
- Cambios correctivos.
- Movimientos sensibles.

---

## 17. Módulo: purchases

### Responsabilidad

Gestionar compras a proveedores.

### Funciones

- Crear compra en borrador.
- Agregar items.
- Editar compra no confirmada.
- Confirmar compra.
- Cancelar compra no confirmada.
- Consultar compras.

### Endpoints candidatos

- GET /purchases
- GET /purchases/:id
- POST /purchases
- PATCH /purchases/:id
- POST /purchases/:id/items
- PATCH /purchases/:id/items/:itemId
- DELETE /purchases/:id/items/:itemId
- POST /purchases/:id/confirm
- POST /purchases/:id/cancel

### Validaciones

- Proveedor obligatorio.
- Almacén obligatorio.
- Compra debe tener al menos un item para confirmarse.
- Cantidades positivas.
- Costos mayores o iguales a cero.
- Una compra confirmada no se edita libremente.
- Una compra confirmada aumenta stock y genera movimientos.

### Permisos candidatos

- purchase.view
- purchase.create
- purchase.update
- purchase.confirm
- purchase.cancel

### Auditoría

Registrar:

- Creación.
- Confirmación.
- Cancelación.
- Cambios importantes.

---

## 18. Módulo: sales

### Responsabilidad

Gestionar ventas.

### Funciones

- Crear venta.
- Agregar items.
- Validar stock.
- Confirmar venta.
- Cancelar venta.
- Consultar ventas.
- Consultar ventas por fecha, vendedor o cliente.

### Endpoints candidatos

- GET /sales
- GET /sales/:id
- POST /sales
- PATCH /sales/:id
- POST /sales/:id/items
- PATCH /sales/:id/items/:itemId
- DELETE /sales/:id/items/:itemId
- POST /sales/:id/confirm
- POST /sales/:id/cancel

### Validaciones

- Venta debe tener al menos un item para confirmarse.
- Cada item debe tener SKU válido.
- Cantidad positiva.
- Precio mayor o igual a cero.
- No confirmar si no hay stock suficiente.
- Una venta confirmada no se borra físicamente.
- Cancelar una venta confirmada debe generar movimiento correctivo si el stock salió.

### Permisos candidatos

- sale.view
- sale.create
- sale.update
- sale.confirm
- sale.cancel

### Auditoría

Registrar:

- Creación.
- Confirmación.
- Cancelación.
- Cambios de total.

---

## 19. Módulo: payments

### Responsabilidad

Gestionar métodos de pago y pagos de ventas.

### Funciones MVP

- Listar métodos de pago.
- Registrar pago de venta.
- Consultar pagos de una venta.

### Endpoints candidatos

- GET /payment-methods
- POST /payment-methods
- PATCH /payment-methods/:id
- GET /sales/:id/payments
- POST /sales/:id/payments

### Validaciones

- El monto del pago debe ser positivo.
- El pago debe asociarse a una venta válida.
- En MVP se puede requerir que el total pagado coincida con total de venta al confirmar.

### Permisos candidatos

- payment_method.view
- payment_method.manage
- sale_payment.view
- sale_payment.create

---

## 20. Módulo: reports

### Responsabilidad

Proveer reportes operativos.

### Reportes MVP

- Stock bajo.
- Ventas por fecha.
- Compras por fecha.
- Productos más vendidos.
- Movimientos de inventario.
- Valor aproximado del inventario.

### Endpoints candidatos

- GET /reports/low-stock
- GET /reports/sales-summary
- GET /reports/purchases-summary
- GET /reports/top-selling-products
- GET /reports/inventory-movements
- GET /reports/inventory-valuation

### Validaciones

- Fechas válidas.
- Rango de fechas razonable.
- Permiso para ver reportes.

### Permisos candidatos

- report.view
- report.sales
- report.purchases
- report.inventory

---

## 21. Módulo: audit

### Responsabilidad

Consultar auditoría de acciones críticas.

### Funciones

- Ver logs de auditoría.
- Filtrar por usuario.
- Filtrar por entidad.
- Filtrar por fecha.
- Filtrar por acción.

### Endpoints candidatos

- GET /audit-logs
- GET /audit-logs/:id

### Validaciones

- Solo usuarios autorizados pueden ver auditoría.
- No permitir editar logs desde la API normal.

### Permisos candidatos

- audit.view

---

## 22. Módulo: health

### Responsabilidad

Permitir revisar estado básico del backend.

### Funciones

- Verificar que la API responde.
- Verificar conexión a base de datos en una fase posterior.

### Endpoints candidatos

- GET /health
- GET /health/db

### Permisos

- /health puede ser público.
- /health/db puede ser privado o limitado según entorno.

---

## 23. Servicios internos críticos

Además de rutas HTTP, el backend debe tener servicios internos.

### 23.1 InventoryService

Responsabilidad:

- Aumentar stock.
- Reducir stock.
- Reservar stock en futuro.
- Liberar reserva en futuro.
- Crear movimientos.
- Validar stock disponible.

No debe permitir cambios sin causa.

---

### 23.2 AuditService

Responsabilidad:

- Registrar acciones críticas.
- Estandarizar formato de auditoría.

---

### 23.3 PermissionService

Responsabilidad:

- Resolver permisos efectivos del usuario.
- Validar acceso a acciones.

---

### 23.4 PricingService futuro

Responsabilidad futura:

- Descuentos.
- Reglas de precio.
- Impuestos.
- Promociones.

En MVP puede ser simple o no existir todavía.

---

## 24. Flujo crítico: confirmar compra

1. Usuario solicita confirmar compra.
2. Backend valida autenticación.
3. Backend valida permiso purchase.confirm.
4. Backend verifica que la compra existe.
5. Backend verifica que la compra no está confirmada.
6. Backend verifica que tiene items.
7. Backend inicia operación transaccional.
8. Backend cambia estado de compra a confirmed.
9. Backend aumenta stock por cada item usando InventoryService.
10. Backend crea inventory_movements.
11. Backend registra audit_logs.
12. Backend confirma transacción.
13. Backend responde con la compra confirmada.

---

## 25. Flujo crítico: confirmar venta

1. Usuario solicita confirmar venta.
2. Backend valida autenticación.
3. Backend valida permiso sale.confirm.
4. Backend verifica que la venta existe.
5. Backend verifica que la venta no está confirmada.
6. Backend verifica que tiene items.
7. Backend valida stock disponible por cada item.
8. Backend inicia operación transaccional.
9. Backend cambia estado de venta a confirmed.
10. Backend reduce stock por cada item usando InventoryService.
11. Backend crea inventory_movements.
12. Backend registra pagos si aplica.
13. Backend registra audit_logs.
14. Backend confirma transacción.
15. Backend responde con la venta confirmada.

---

## 26. Reglas de transacciones

Deben ser transaccionales:

- Confirmar compra.
- Confirmar venta.
- Cancelar venta confirmada.
- Ajustar stock.
- Transferir stock en futuro.
- Procesar devolución en futuro.

Regla:

**Si falla una parte del proceso crítico, no debe quedar stock, venta, compra o movimiento a medias.**

---

## 27. Orden recomendado de implementación futura

Cuando llegue el momento de escribir código, el orden recomendado será:

1. Estructura base del backend.
2. Configuración de PostgreSQL.
3. Sistema de errores y middlewares.
4. Auth básico.
5. Usuarios, roles y permisos.
6. Organización, tienda y almacén por defecto.
7. Categorías.
8. Productos.
9. SKUs.
10. Stock levels.
11. Inventory movements.
12. Proveedores.
13. Compras.
14. Confirmación de compras.
15. Clientes.
16. Ventas.
17. Confirmación de ventas.
18. Pagos básicos.
19. Reportes.
20. Auditoría.

---

## 28. Evals específicos para backend planner

El agente backend planner debe detectar errores como:

1. Endpoint de venta que reduce stock sin crear movimiento.
2. Confirmación de compra sin transacción.
3. Venta confirmada editable libremente.
4. Vendedor con permiso para ajustar stock.
5. Producto con campo stock directo como fuente de verdad.
6. Falta de validación de stock antes de confirmar venta.
7. Falta de auditoría en cambio de precio.
8. Eliminación física de venta confirmada.
9. Falta de permisos por endpoint.
10. Error de diseño donde frontend decide reglas críticas de inventario.

---

## 29. Decisión actual

El backend se diseñará como monolito modular con servicios internos.

La prioridad será garantizar consistencia de inventario, permisos, auditoría y transacciones antes de agregar funciones avanzadas.

No se debe crear código todavía hasta validar este documento junto con el modelo de datos PostgreSQL v0.1.
