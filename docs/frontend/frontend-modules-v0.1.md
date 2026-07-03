# Frontend Modules v0.1

## 1. Objetivo

Planificar el frontend de InventoryCore antes de escribir código.

Este documento define pantallas, navegación, componentes reutilizables, estados visuales y orden recomendado de implementación para una aplicación React + Vite.

El frontend debe ser simple para una tienda pequeña, pero preparado para crecer hacia múltiples tiendas, almacenes, usuarios, ventas, compras, reportes y auditoría.

---

## 2. Stack frontend

- React.
- Vite.
- API REST.
- Panel administrativo modular.
- Diseño preparado para roles de usuario.

Decisión pendiente:

- Elegir entre JavaScript y TypeScript antes de crear la app.

Recomendación:

- Usar TypeScript si el objetivo es crear un sistema más robusto y escalable.

---

## 3. Principios UX

1. El frontend no será la fuente de verdad del inventario.
2. Las reglas críticas deben validarse en backend.
3. Las pantallas deben ser claras y rápidas.
4. Las tablas deben tener búsqueda, filtros y paginación.
5. Las acciones importantes deben pedir confirmación.
6. Cada pantalla debe tener estado de carga, vacío y error.
7. Los formularios deben validar antes de enviar.
8. Las respuestas del backend deben mostrarse con mensajes claros.
9. La navegación debe ser simple.
10. Los componentes deben ser reutilizables.

---

## 4. Layout principal

Estructura general:

```txt
App
  PublicArea
    AccessPage
  PrivateArea
    DashboardLayout
      Sidebar
      Topbar
      MainContent
      Notifications
      ConfirmDialogs
```

Sidebar inicial:

- Dashboard.
- Productos.
- SKUs.
- Categorías.
- Inventario.
- Compras.
- Ventas.
- Proveedores.
- Clientes.
- Reportes.
- Usuarios.
- Auditoría.
- Configuración.

Topbar inicial:

- Organización activa.
- Usuario actual.
- Acceso a perfil.
- Cerrar sesión.
- Tienda activa futura.
- Almacén activo futuro.

---

## 5. Pantallas principales

### 5.1 AccessPage

Pantalla de entrada al sistema.

Debe incluir:

- Formulario de acceso.
- Mensajes de error.
- Estado de carga.

---

### 5.2 DashboardPage

Vista general de operación.

Widgets candidatos:

- Ventas del día.
- Número de ventas del día.
- Stock bajo.
- Compras recientes.
- Ventas recientes.
- Movimientos recientes.
- Valor aproximado del inventario.

---

### 5.3 ProductsListPage

Lista de productos base.

Debe incluir:

- Tabla.
- Búsqueda por nombre.
- Filtro por categoría.
- Filtro por estado.
- Crear producto.
- Ver detalle.
- Editar.
- Desactivar.

Regla:

El producto base no debe editar stock directamente.

---

### 5.4 ProductFormPage

Formulario de producto.

Campos candidatos:

- Nombre.
- Descripción.
- Categoría.
- Marca.
- Estado.

---

### 5.5 SkusListPage

Lista de unidades vendibles.

Debe incluir:

- Tabla.
- Búsqueda por SKU.
- Búsqueda por código de barras.
- Filtro por producto.
- Filtro por estado.
- Filtro por stock bajo.

Columnas candidatas:

- SKU.
- Nombre.
- Producto.
- Precio.
- Costo.
- Stock disponible.
- Stock mínimo.
- Estado.

---

### 5.6 SkuFormPage

Formulario de SKU.

Campos candidatos:

- Producto asociado.
- Código SKU.
- Código de barras.
- Nombre de variante.
- Unidad de medida.
- Precio.
- Costo.
- Stock mínimo.
- Estado.

Regla:

Crear un SKU no debe aumentar stock automáticamente.

---

### 5.7 InventoryStockPage

Consulta de stock actual.

Debe incluir:

- Tabla de stock.
- Filtro por almacén.
- Filtro por categoría.
- Filtro por stock bajo.
- Búsqueda por SKU o producto.

Columnas candidatas:

- SKU.
- Producto.
- Almacén.
- Stock físico.
- Stock reservado.
- Stock disponible.
- Stock mínimo.

---

### 5.8 InventoryMovementsPage

Historial de movimientos de inventario.

Filtros candidatos:

- Fecha.
- SKU.
- Producto.
- Almacén.
- Tipo de movimiento.
- Usuario.
- Referencia.

Regla:

Los movimientos son históricos y no deben editarse libremente.

---

### 5.9 StockAdjustmentPage

Pantalla para ajuste manual de inventario.

Debe incluir:

- SKU.
- Almacén.
- Cantidad nueva o diferencia.
- Motivo obligatorio.
- Confirmación antes de enviar.

---

### 5.10 PurchasesListPage

Lista de compras a proveedores.

Filtros candidatos:

- Proveedor.
- Estado.
- Fecha.
- Almacén.

Acciones candidatas:

- Ver.
- Crear.
- Editar si no está confirmada.
- Confirmar.
- Cancelar.

---

### 5.11 PurchaseFormPage

Formulario de compra.

Debe incluir:

- Proveedor.
- Almacén destino.
- Items de compra.
- Cantidad.
- Costo unitario.
- Subtotal.
- Total.

Regla:

Confirmar una compra aumenta stock y genera movimientos.

---

### 5.12 SalesListPage

Lista de ventas.

Filtros candidatos:

- Fecha.
- Estado.
- Cliente.
- Vendedor.

Acciones candidatas:

- Ver.
- Crear.
- Editar si no está confirmada.
- Confirmar.
- Cancelar.

---

### 5.13 SaleFormPage

Formulario de venta.

Debe incluir:

- Cliente opcional.
- Almacén o tienda activa.
- Buscador de SKU.
- Items de venta.
- Cantidad.
- Precio unitario.
- Descuento opcional.
- Total.
- Método de pago.

Regla:

Confirmar una venta reduce stock y genera movimientos.

---

### 5.14 SuppliersPage

Gestión de proveedores.

Debe incluir:

- Lista.
- Búsqueda.
- Crear.
- Editar.
- Desactivar.
- Ver compras asociadas.

---

### 5.15 CustomersPage

Gestión de clientes.

Debe incluir:

- Lista.
- Búsqueda.
- Crear.
- Editar.
- Desactivar.
- Ver ventas asociadas.

Regla:

La venta de mostrador puede funcionar sin cliente identificado.

---

### 5.16 UsersPage

Gestión de usuarios internos.

Debe incluir:

- Lista.
- Búsqueda.
- Filtro por rol.
- Filtro por estado.
- Crear.
- Editar.
- Desactivar.

---

### 5.17 ReportsPage

Centro de reportes.

Reportes MVP:

- Stock bajo.
- Ventas por fecha.
- Compras por fecha.
- Productos más vendidos.
- Movimientos de inventario.
- Valor aproximado del inventario.

---

### 5.18 AuditLogsPage

Consulta de acciones críticas.

Filtros candidatos:

- Usuario.
- Acción.
- Entidad.
- Fecha.

Regla:

La auditoría debe ser solo consulta desde la interfaz normal.

---

## 6. Componentes reutilizables

Componentes base:

- AppLayout.
- Sidebar.
- Topbar.
- DataTable.
- SearchInput.
- FilterPanel.
- StatusBadge.
- ConfirmDialog.
- FormField.
- SelectField.
- MoneyInput.
- QuantityInput.
- DateRangeFilter.
- ToastNotification.
- EmptyState.
- LoadingState.
- ErrorState.
- PageHeader.
- ActionMenu.
- Pagination.

Componentes específicos:

- ProductSelector.
- SkuSelector.
- WarehouseSelector.
- SupplierSelector.
- CustomerSelector.
- SaleCart.
- PurchaseItemsTable.
- StockBadge.
- MovementTypeBadge.

---

## 7. Servicios API candidatos

- authApi.
- usersApi.
- productsApi.
- skusApi.
- categoriesApi.
- suppliersApi.
- customersApi.
- inventoryApi.
- purchasesApi.
- salesApi.
- paymentsApi.
- reportsApi.
- auditApi.

Regla:

Los componentes no deben construir llamadas de API directamente. Deben usar servicios o hooks dedicados.

---

## 8. Estados de interfaz obligatorios

Cada pantalla importante debe manejar:

- Loading.
- Error.
- Empty state.
- Success state.
- Validation errors.
- Unauthorized.
- Forbidden.

---

## 9. Acciones críticas con confirmación

Deben pedir confirmación:

- Confirmar venta.
- Cancelar venta.
- Confirmar compra.
- Cancelar compra.
- Ajustar stock.
- Cambiar permisos.
- Desactivar usuario.
- Desactivar producto o SKU.
- Cambiar precio si se considera crítico.

El modal debe explicar la consecuencia real de la acción.

---

## 10. Orden recomendado de implementación futura

1. Crear app React + Vite.
2. Definir estructura de carpetas.
3. Crear layout base.
4. Crear routing.
5. Crear pantalla de acceso.
6. Crear estado de sesión.
7. Crear rutas privadas.
8. Crear componentes base.
9. Crear dashboard inicial.
10. Crear productos.
11. Crear SKUs.
12. Crear categorías.
13. Crear inventario stock.
14. Crear movimientos.
15. Crear proveedores.
16. Crear compras.
17. Crear ventas.
18. Crear clientes.
19. Crear usuarios.
20. Crear reportes.
21. Crear auditoría.

---

## 11. Evals para frontend planner

El agente frontend planner debe detectar errores como:

1. Pantalla de producto que permite editar stock directamente.
2. Botón de ajustar stock visible para usuario sin autorización.
3. Venta que parece confirmada sin esperar respuesta del backend.
4. Formulario de compra que no explica que confirmar aumenta stock.
5. Tabla sin estado empty, error o loading.
6. Acciones críticas sin confirmación.
7. Dashboard igual para todos los roles sin filtrar información sensible.
8. Cliente obligatorio en venta de mostrador cuando el modelo permite cliente opcional.
9. Componentes con lógica crítica de inventario en vez de depender del backend.
10. Servicios API mezclados dentro de componentes visuales.

---

## 12. Decisión actual

El frontend se diseñará como panel administrativo modular.

La prioridad será claridad operativa: vender rápido, comprar bien, consultar stock de forma confiable y evitar acciones peligrosas sin confirmación.

No se debe crear código todavía hasta validar este documento junto con:

- docs/master-project.md
- docs/data-model/postgresql-v0.1.md
- docs/api/backend-modules-v0.1.md
