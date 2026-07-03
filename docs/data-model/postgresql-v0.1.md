# Modelo de Datos PostgreSQL v0.1

## 1. Objetivo del documento

Este documento define el primer modelo conceptual de datos para InventoryCore usando PostgreSQL.

Todavía no es SQL ejecutable. Es una guía de diseño para decidir entidades, relaciones, reglas de consistencia y límites del MVP antes de crear migraciones reales.

El objetivo es construir una base que sirva para una tienda pequeña, pero que no bloquee crecimiento futuro hacia múltiples tiendas, múltiples almacenes, ventas online, reportes avanzados y operación empresarial.

---

## 2. Principios de diseño de datos

1. PostgreSQL será la base de datos oficial del proyecto.
2. El sistema debe usar una base relacional bien normalizada.
3. El stock no debe depender únicamente de un número editable en producto.
4. Todo cambio de stock debe generar un movimiento de inventario.
5. Las ventas confirmadas no se deben borrar físicamente.
6. Las compras confirmadas no se deben borrar físicamente.
7. Los productos, usuarios, clientes y proveedores deben poder desactivarse sin perder historial.
8. El modelo debe soportar una sola tienda en el MVP, pero estar preparado para múltiples tiendas y almacenes.
9. El sistema debe evitar stock negativo salvo que una configuración futura lo permita explícitamente.
10. Las entidades críticas deben tener auditoría.
11. Las cantidades monetarias deben manejarse con tipos decimales, no con flotantes.
12. Las fechas críticas deben guardar fecha de creación, actualización y usuario responsable cuando aplique.
13. El diseño inicial debe evitar microservicios y mantener un monolito modular.

---

## 3. Decisiones PostgreSQL iniciales

### 3.1 Identificadores

Para una versión escalable, se recomienda usar identificadores tipo UUID en entidades principales.

Entidades candidatas para UUID:

- organizations
- users
- products
- skus
- warehouses
- inventory_movements
- purchases
- sales
- customers
- suppliers

Motivo:

- Facilita futuras integraciones.
- Reduce exposición de IDs secuenciales.
- Funciona mejor si el sistema crece hacia múltiples servicios o sincronización externa.

Para tablas puente o catálogos simples, se puede evaluar UUID o entero según complejidad.

Decisión actual:

**Usar UUID como decisión base en entidades principales.**

---

### 3.2 Timestamps

Toda entidad importante debe incluir:

- created_at
- updated_at
- deleted_at opcional si se usa soft delete

Para entidades críticas también puede incluir:

- created_by
- updated_by
- deleted_by

---

### 3.3 Estados en vez de eliminación

Entidades comerciales críticas deben usar estados.

Ejemplos:

- active
- inactive
- draft
- pending
- confirmed
- cancelled
- completed
- returned

Regla:

**No se deben borrar físicamente registros que tengan impacto en inventario, ventas, compras o auditoría.**

---

### 3.4 Dinero

Los importes deben usar decimal.

Ejemplos conceptuales:

- unit_price
- unit_cost
- subtotal
- discount_amount
- tax_amount
- total_amount

No usar float para dinero.

---

### 3.5 Cantidades

Las cantidades de inventario deben soportar enteros o decimales según tipo de negocio.

Ejemplos:

- Tienda de ropa: unidades enteras.
- Tienda de alimentos por peso: cantidades decimales.

Decisión inicial:

**Usar cantidades decimales con escala controlada para permitir productos por peso o volumen en el futuro.**

---

## 4. Agrupación por dominios

El modelo se organizará por dominios funcionales.

Dominios iniciales:

1. Identidad y permisos.
2. Organización y ubicaciones.
3. Catálogo de productos.
4. Inventario.
5. Compras.
6. Ventas.
7. Clientes.
8. Proveedores.
9. Pagos.
10. Auditoría.
11. Reportes derivados.

---

## 5. Dominio: identidad y permisos

### 5.1 users

Representa usuarios internos del sistema.

Ejemplos:

- Administrador.
- Vendedor.
- Encargado de almacén.
- Supervisor.

Campos conceptuales:

- id
- organization_id
- name
- email
- password_hash
- status
- last_login_at
- created_at
- updated_at

Reglas:

- El email debe ser único dentro de la organización.
- No guardar contraseñas en texto plano.
- Los usuarios se desactivan, no se borran físicamente.

---

### 5.2 roles

Define roles del sistema.

Roles iniciales:

- admin
- seller
- warehouse_manager
- supervisor

Campos conceptuales:

- id
- organization_id opcional
- name
- description
- is_system_role
- created_at
- updated_at

Reglas:

- Algunos roles pueden ser globales del sistema.
- En una fase futura, una empresa podría crear roles personalizados.

---

### 5.3 permissions

Define permisos granulares.

Ejemplos:

- product.create
- product.update
- stock.adjust
- sale.create
- sale.cancel
- purchase.confirm
- report.view
- user.manage

Campos conceptuales:

- id
- code
- description
- module

---

### 5.4 role_permissions

Relaciona roles con permisos.

Campos conceptuales:

- role_id
- permission_id

Reglas:

- Permite separar rol de permisos reales.
- Evita hardcodear autorización solo por nombre de rol.

---

### 5.5 user_roles

Relaciona usuarios con roles.

Campos conceptuales:

- user_id
- role_id

Reglas:

- Un usuario puede tener uno o varios roles.
- En el MVP se puede usar un rol por usuario, pero la tabla deja preparado el crecimiento.

---

## 6. Dominio: organización y ubicaciones

### 6.1 organizations

Representa la empresa, tienda o tenant.

En el MVP puede existir una sola organización.

Campos conceptuales:

- id
- name
- legal_name
- tax_id opcional
- status
- created_at
- updated_at

Motivo para incluirla desde el inicio:

- Permite crecer a SaaS o multiempresa.
- Permite que una tienda pequeña sea una organización.
- Evita rediseñar todas las tablas en el futuro.

Regla:

**Aunque el MVP use una sola organización, las entidades principales deben poder asociarse a organization_id.**

---

### 6.2 stores

Representa una tienda o sucursal comercial.

Ejemplos:

- Tienda principal.
- Sucursal centro.
- Sucursal norte.
- Tienda online.

Campos conceptuales:

- id
- organization_id
- name
- code
- address
- status
- created_at
- updated_at

Reglas:

- En el MVP puede existir una sola tienda por defecto.
- En el futuro una organización puede tener varias tiendas.

---

### 6.3 warehouses

Representa un almacén físico o lógico donde existe stock.

Ejemplos:

- Almacén principal.
- Trastienda.
- Depósito externo.
- Almacén de ecommerce.

Campos conceptuales:

- id
- organization_id
- store_id opcional
- name
- code
- address
- status
- created_at
- updated_at

Reglas:

- En el MVP puede existir un almacén llamado Principal.
- Toda cantidad de stock debe pertenecer a un almacén.
- Una tienda puede tener uno o varios almacenes.

---

## 7. Dominio: catálogo de productos

### 7.1 categories

Agrupa productos.

Campos conceptuales:

- id
- organization_id
- parent_id opcional
- name
- slug
- description
- status
- created_at
- updated_at

Reglas:

- Puede soportar categorías jerárquicas en el futuro.
- En el MVP se puede usar una sola profundidad.

---

### 7.2 products

Representa el producto base, no necesariamente la unidad vendible final.

Ejemplo:

- Camiseta básica.
- Botella de agua.
- Laptop Lenovo.

Campos conceptuales:

- id
- organization_id
- category_id
- name
- description
- brand opcional
- status
- created_at
- updated_at

Reglas:

- El producto puede tener uno o varios SKUs.
- El producto no debe almacenar el stock real directamente.
- El producto puede desactivarse.

---

### 7.3 skus

Representa la unidad vendible real.

Ejemplo:

- Camiseta básica negra talla M.
- Botella de agua 1.5 L.
- Laptop Lenovo 16GB RAM 512GB SSD.

Campos conceptuales:

- id
- organization_id
- product_id
- sku_code
- barcode opcional
- name
- attributes opcional
- unit_of_measure
- cost_price
- sale_price
- min_stock_level
- status
- created_at
- updated_at

Reglas:

- sku_code debe ser único dentro de la organización.
- barcode debe ser único si existe.
- Las ventas y compras deben trabajar principalmente contra SKUs.
- El stock se calcula o consulta por SKU y almacén.

---

### 7.4 product_images

Entidad futura o secundaria para imágenes de productos.

Campos conceptuales:

- id
- product_id
- sku_id opcional
- url
- alt_text
- sort_order
- created_at

Regla:

- No es crítica para el MVP de inventario, pero puede añadirse cuando se trabaje frontend/ecommerce.

---

## 8. Dominio: inventario

### 8.1 stock_levels

Representa el stock actual de un SKU en un almacén.

Campos conceptuales:

- id
- organization_id
- warehouse_id
- sku_id
- quantity_on_hand
- quantity_reserved
- quantity_available
- updated_at

Definiciones:

- quantity_on_hand: cantidad física o registrada total.
- quantity_reserved: cantidad separada para pedidos o ventas pendientes.
- quantity_available: cantidad realmente vendible.

Regla recomendada:

quantity_available = quantity_on_hand - quantity_reserved

Reglas:

- Debe existir una combinación única por warehouse_id + sku_id.
- No debe permitirse stock disponible negativo en el MVP.
- Esta tabla es una vista operativa del stock actual, no el historial.
- El historial vive en inventory_movements.

---

### 8.2 inventory_movements

Tabla crítica del sistema.

Registra todo cambio de inventario.

Tipos de movimiento iniciales:

- purchase_in
- sale_out
- manual_adjustment
- return_in
- return_out
- transfer_in
- transfer_out
- cancellation_in
- cancellation_out
- damaged_out
- loss_out

Campos conceptuales:

- id
- organization_id
- warehouse_id
- sku_id
- movement_type
- quantity
- direction
- reason
- reference_type
- reference_id
- created_by
- created_at

Definiciones:

- direction puede ser in o out.
- reference_type puede ser purchase, sale, return, adjustment, transfer, cancellation.
- reference_id apunta al registro origen según reference_type.

Reglas:

- Todo cambio de stock debe crear un movimiento.
- No debe existir movimiento sin SKU.
- No debe existir movimiento sin almacén.
- No debe existir ajuste manual sin motivo.
- Los movimientos no deben editarse libremente después de creados.
- Si hay error, debe crearse un movimiento correctivo.

---

### 8.3 stock_adjustments

Representa ajustes manuales justificados.

Campos conceptuales:

- id
- organization_id
- warehouse_id
- sku_id
- quantity_before
- quantity_after
- difference
- reason
- status
- created_by
- approved_by opcional
- created_at
- approved_at opcional

Reglas:

- En MVP puede confirmarse directamente por administrador.
- En versión avanzada puede requerir aprobación.
- Todo ajuste confirmado debe crear inventory_movement.

---

### 8.4 stock_transfers

Entidad preparada para futuro multialmacén.

Campos conceptuales:

- id
- organization_id
- source_warehouse_id
- target_warehouse_id
- status
- created_by
- confirmed_by
- created_at
- confirmed_at

Regla:

- No es obligatoria en el MVP si solo hay un almacén, pero el modelo debe permitirla después.

---

### 8.5 stock_transfer_items

Detalle de productos transferidos entre almacenes.

Campos conceptuales:

- id
- transfer_id
- sku_id
- quantity

Reglas:

- Una transferencia confirmada genera salida en almacén origen y entrada en almacén destino.

---

## 9. Dominio: proveedores y compras

### 9.1 suppliers

Representa proveedores.

Campos conceptuales:

- id
- organization_id
- name
- contact_name
- email
- phone
- address
- tax_id opcional
- status
- created_at
- updated_at

Reglas:

- Un proveedor puede tener muchas compras.
- No se debe borrar si tiene compras asociadas.

---

### 9.2 purchases

Representa una compra a proveedor.

Estados iniciales:

- draft
- pending
- confirmed
- cancelled

Campos conceptuales:

- id
- organization_id
- supplier_id
- warehouse_id
- purchase_number
- status
- subtotal
- discount_amount
- tax_amount
- total_amount
- notes
- created_by
- confirmed_by
- created_at
- confirmed_at
- cancelled_at

Reglas:

- Una compra en borrador no aumenta stock.
- Una compra confirmada aumenta stock.
- Una compra confirmada genera movimientos purchase_in.
- Una compra confirmada no se borra.
- Una compra cancelada debe seguir visible para auditoría.

---

### 9.3 purchase_items

Detalle de SKUs comprados.

Campos conceptuales:

- id
- purchase_id
- sku_id
- quantity
- unit_cost
- subtotal

Reglas:

- Una compra debe tener al menos un item.
- Las cantidades deben ser positivas.
- Los costos deben ser mayores o iguales a cero.

---

## 10. Dominio: clientes y ventas

### 10.1 customers

Representa clientes.

En el MVP puede ser opcional.

Campos conceptuales:

- id
- organization_id
- name
- email opcional
- phone opcional
- document_id opcional
- address opcional
- status
- created_at
- updated_at

Reglas:

- Una venta puede existir sin cliente identificado si es venta de mostrador.
- En ecommerce futuro, el cliente será más importante.

---

### 10.2 sales

Representa una venta.

Estados iniciales:

- draft
- pending
- confirmed
- cancelled
- partially_returned
- returned

Campos conceptuales:

- id
- organization_id
- store_id
- warehouse_id
- customer_id opcional
- sale_number
- status
- subtotal
- discount_amount
- tax_amount
- total_amount
- notes
- created_by
- confirmed_by
- created_at
- confirmed_at
- cancelled_at

Reglas:

- Una venta en borrador no reduce stock.
- Una venta confirmada reduce stock.
- Una venta confirmada genera movimientos sale_out.
- No se puede confirmar una venta sin stock disponible suficiente.
- Una venta confirmada no se borra.
- Si se cancela una venta confirmada, debe generar movimiento correctivo si corresponde.

---

### 10.3 sale_items

Detalle de SKUs vendidos.

Campos conceptuales:

- id
- sale_id
- sku_id
- quantity
- unit_price
- discount_amount
- subtotal

Reglas:

- Una venta debe tener al menos un item.
- Las cantidades deben ser positivas.
- El precio debe ser mayor o igual a cero.
- La venta debe validar stock por SKU y almacén.

---

## 11. Dominio: pagos

### 11.1 payment_methods

Catálogo de métodos de pago.

Ejemplos:

- cash
- card
- bank_transfer
- online
- other

Campos conceptuales:

- id
- organization_id opcional
- name
- code
- status

---

### 11.2 sale_payments

Pagos asociados a ventas.

Campos conceptuales:

- id
- sale_id
- payment_method_id
- amount
- reference
- created_at

Reglas:

- Una venta puede tener uno o varios pagos.
- En el MVP se puede usar un pago único.
- En el futuro permite pagos mixtos.

---

## 12. Dominio: devoluciones

Las devoluciones pueden ser complejas. Para el MVP se pueden preparar conceptualmente, pero no implementarlas completas al inicio.

### 12.1 sale_returns

Representa devolución de cliente.

Campos conceptuales:

- id
- organization_id
- sale_id
- status
- reason
- created_by
- confirmed_by
- created_at
- confirmed_at

Reglas:

- Una devolución confirmada puede aumentar stock si el producto vuelve en buen estado.
- Una devolución puede no aumentar stock si el producto está dañado.

---

### 12.2 sale_return_items

Detalle de SKUs devueltos.

Campos conceptuales:

- id
- sale_return_id
- sale_item_id
- sku_id
- quantity
- condition
- restock

Reglas:

- Si restock es verdadero, debe crear movimiento return_in.
- Si restock es falso, puede crear registro de pérdida o daño.

---

## 13. Dominio: auditoría

### 13.1 audit_logs

Registra acciones críticas.

Campos conceptuales:

- id
- organization_id
- user_id
- action
- entity_type
- entity_id
- before_data opcional
- after_data opcional
- ip_address opcional
- user_agent opcional
- created_at

Acciones críticas:

- Crear producto.
- Modificar producto.
- Cambiar precio.
- Confirmar venta.
- Cancelar venta.
- Confirmar compra.
- Ajustar stock.
- Cambiar permisos.
- Desactivar usuario.

Reglas:

- La auditoría no debe bloquear operaciones normales si hay error secundario, pero debe ser altamente confiable.
- Los registros de auditoría no deben editarse desde la aplicación normal.

---

## 14. Relaciones principales

Relaciones base:

- organization tiene muchos users.
- organization tiene muchas stores.
- organization tiene muchos warehouses.
- organization tiene muchas categories.
- organization tiene muchos products.
- product tiene muchos skus.
- category tiene muchos products.
- warehouse tiene muchos stock_levels.
- sku tiene muchos stock_levels.
- sku tiene muchos inventory_movements.
- supplier tiene muchas purchases.
- purchase tiene muchos purchase_items.
- purchase_item pertenece a sku.
- sale tiene muchos sale_items.
- sale_item pertenece a sku.
- customer puede tener muchas sales.
- user puede crear ventas, compras, ajustes y movimientos.

---

## 15. Reglas de consistencia del inventario

### 15.1 Confirmar compra

Cuando una compra se confirma:

1. La compra cambia a estado confirmed.
2. Cada item aumenta stock_levels.quantity_on_hand.
3. Cada item genera un inventory_movement tipo purchase_in.
4. Se registra auditoría.

---

### 15.2 Confirmar venta

Cuando una venta se confirma:

1. Se valida stock disponible por cada SKU.
2. Si hay stock suficiente, la venta cambia a estado confirmed.
3. Cada item reduce stock_levels.quantity_on_hand o quantity_available según estrategia.
4. Cada item genera inventory_movement tipo sale_out.
5. Se registra auditoría.

---

### 15.3 Ajuste manual

Cuando se hace un ajuste:

1. Se exige permiso.
2. Se exige motivo.
3. Se registra quantity_before y quantity_after.
4. Se actualiza stock_levels.
5. Se genera inventory_movement tipo manual_adjustment.
6. Se registra auditoría.

---

### 15.4 Cancelación

Si una venta confirmada se cancela:

1. No se borra la venta.
2. Cambia a estado cancelled.
3. Si el stock había salido, se genera movimiento correctivo cancellation_in.
4. Se registra auditoría.

---

## 16. Índices candidatos

Este documento no define SQL todavía, pero estas búsquedas deben estar optimizadas:

- users por organization_id y email.
- products por organization_id, name y category_id.
- skus por organization_id, sku_code y barcode.
- stock_levels por warehouse_id y sku_id.
- inventory_movements por organization_id, sku_id, warehouse_id y created_at.
- purchases por organization_id, supplier_id, status y created_at.
- sales por organization_id, customer_id, status y created_at.
- audit_logs por organization_id, user_id, entity_type, entity_id y created_at.

Restricciones únicas candidatas:

- users: organization_id + email.
- skus: organization_id + sku_code.
- skus: organization_id + barcode, cuando barcode no sea nulo.
- stock_levels: warehouse_id + sku_id.
- stores: organization_id + code.
- warehouses: organization_id + code.

---

## 17. MVP de base de datos recomendado

Para no sobrecargar el inicio, el MVP debería implementar primero estas entidades:

1. organizations
2. users
3. roles
4. permissions
5. role_permissions
6. user_roles
7. stores
8. warehouses
9. categories
10. products
11. skus
12. suppliers
13. purchases
14. purchase_items
15. customers
16. sales
17. sale_items
18. payment_methods
19. sale_payments
20. stock_levels
21. inventory_movements
22. stock_adjustments
23. audit_logs

No implementar todavía en MVP inicial:

- stock_transfers
- stock_transfer_items
- sale_returns
- sale_return_items
- product_images
- integración ecommerce
- marketplace
- logística avanzada

Estas entidades quedan documentadas para diseño futuro.

---

## 18. Preguntas pendientes antes de crear SQL

Antes de crear migraciones PostgreSQL reales, se deben resolver estas preguntas:

1. ¿El proyecto será SaaS multiempresa desde el MVP o solo quedará preparado?
2. ¿Los productos podrán venderse por peso o solo por unidades?
3. ¿Habrá impuestos desde el MVP o solo campos preparados?
4. ¿La tienda aceptará ventas sin cliente identificado?
5. ¿Se permitirá venta con stock cero en algún caso?
6. ¿Los precios se podrán cambiar libremente o necesitarán auditoría especial?
7. ¿Los ajustes de stock necesitarán aprobación o bastará con permiso de administrador?
8. ¿Se implementará caja desde el MVP o solo pagos básicos?
9. ¿Se necesitará código de barras desde el MVP?
10. ¿Los SKU serán generados automáticamente o escritos por el usuario?

Decisión provisional:

- Multiempresa queda preparada, pero no se expone como función visible del MVP.
- Se permite cliente opcional en ventas.
- No se permite stock negativo.
- Los ajustes requieren motivo obligatorio.
- Los cambios críticos quedan auditados.

---

## 19. Siguiente paso

El siguiente paso será crear el documento:

`docs/api/backend-modules-v0.1.md`

Ese documento definirá módulos backend, responsabilidades, endpoints candidatos, permisos y reglas de validación por módulo.

No se debe crear SQL ni código de aplicación hasta validar este modelo de datos con los agentes de Hermes.
