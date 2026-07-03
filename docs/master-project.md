# Documento Maestro del Proyecto v0.2

## 1. Nombre provisional

**InventoryCore**

Nombre provisional para representar un sistema central de inventario que pueda empezar como solución para una tienda pequeña y crecer hacia una plataforma compleja de ventas, stock, almacenes y operaciones.

## 2. Visión del proyecto

InventoryCore será un sistema web de inventario y ventas diseñado para funcionar desde una tienda pequeña de barrio hasta una empresa grande con múltiples almacenes, múltiples tiendas, ventas online, usuarios con roles, reportes, auditoría y procesos avanzados.

La idea principal es construir primero una base sólida, simple y funcional, pero preparada para crecer sin tener que rehacer toda la arquitectura.

El proyecto no debe nacer como un sistema gigante desde el primer día. Debe empezar con un MVP serio y escalable.

## 3. Problema que resuelve

Muchas tiendas pequeñas controlan su inventario con libretas, Excel, sistemas viejos o aplicaciones limitadas. Eso genera problemas como:

- No saber cuánto stock real queda.
- Vender productos que ya no están disponibles.
- No registrar correctamente compras y ventas.
- No saber qué productos se venden más.
- No tener historial de movimientos.
- No controlar proveedores.
- No tener permisos por usuario.
- No poder crecer a varias tiendas o almacenes.

InventoryCore busca resolver estos problemas con una plataforma clara, modular y preparada para crecer.

## 4. Objetivo principal

Crear un sistema de inventario y ventas que permita:

- Registrar productos.
- Gestionar categorías.
- Controlar stock.
- Registrar entradas y salidas de inventario.
- Registrar compras a proveedores.
- Registrar ventas a clientes.
- Consultar reportes básicos.
- Manejar usuarios y roles.
- Mantener historial de movimientos.
- Prepararse para múltiples tiendas, almacenes y ventas online en futuras fases.

## 5. Tipo de producto

InventoryCore será inicialmente una aplicación web full stack:

- Frontend web.
- Backend API REST.
- Base de datos relacional PostgreSQL.
- Autenticación con usuarios y roles.
- Panel administrativo.
- Sistema de inventario y ventas.

Futuras versiones:

- Multiempresa.
- Multialmacén.
- Ecommerce.
- Marketplace.
- API pública.
- Analítica avanzada.
- Integraciones externas.
- Facturación, logística y predicción de demanda.

## 6. Usuarios principales

### 6.1 Administrador

Usuario con control total del sistema. Puede crear y editar productos, gestionar usuarios, ver reportes, registrar compras, registrar ventas, hacer ajustes de inventario, gestionar proveedores, configurar categorías, ver auditoría y administrar roles/permisos.

### 6.2 Vendedor

Usuario de caja o ventas. Puede ver productos disponibles, registrar ventas, consultar stock, registrar clientes básicos si aplica y ver historial de ventas propias.

No debe eliminar productos, modificar precios críticos sin permiso, hacer ajustes manuales de stock, gestionar usuarios ni acceder a configuración avanzada.

### 6.3 Encargado de almacén

Usuario responsable del inventario físico. Puede registrar entradas, salidas internas, conteos, pérdidas, transferencias futuras entre almacenes y alertas de stock bajo.

No debe modificar ventas cerradas, gestionar usuarios ni cambiar configuración financiera crítica.

### 6.4 Supervisor o gerente

Usuario con permisos de análisis y control. Puede ver reportes, revisar movimientos, aprobar ajustes, consultar ventas, consultar compras y revisar actividad de usuarios.

### 6.5 Cliente

En la primera versión puede existir solo como registro interno de venta. En futuras versiones puede tener cuenta propia para ecommerce o marketplace.

## 7. Alcance inicial del MVP

La primera versión debe incluir:

- Login.
- Usuarios.
- Roles básicos.
- Productos.
- Categorías.
- SKUs simples.
- Proveedores.
- Compras.
- Ventas.
- Stock actual.
- Movimientos de inventario.
- Reporte de stock bajo.
- Reporte básico de ventas.
- Historial de movimientos.
- Permisos básicos.

## 8. Fuera de alcance en la primera versión

No se implementará inicialmente:

- Marketplace tipo Amazon.
- App móvil.
- Microservicios.
- Inteligencia artificial predictiva.
- Facturación legal avanzada.
- Integraciones con bancos.
- Integración con transportistas.
- Multiempresa SaaS.
- Ecommerce completo.
- Múltiples monedas.
- Sistema avanzado de impuestos.
- Picking y packing.
- Gestión logística compleja.
- Sistema de devoluciones avanzado.
- Reposición automática.

Estos módulos se dejan preparados a nivel de arquitectura, pero no se construyen en la primera versión.

## 9. Principios de arquitectura

1. Empezar simple, pero no mal diseñado.
2. Separar frontend, backend y base de datos.
3. Mantener la lógica de negocio en el backend.
4. Evitar lógica crítica solo en el frontend.
5. Usar PostgreSQL como base de datos relacional principal.
6. Registrar todo movimiento importante.
7. No borrar información crítica si puede afectar historial.
8. Usar estados en vez de eliminar registros importantes.
9. Diseñar pensando en múltiples almacenes aunque el MVP empiece con uno.
10. Diseñar pensando en múltiples tiendas aunque el MVP empiece con una.
11. No usar microservicios al inicio.
12. No sobreingenierizar el MVP.
13. Mantener documentación clara desde el primer día.
14. Revisar cada cambio importante con un agente reviewer.
15. Crear pruebas y checklists para flujos críticos.

## 10. Principio clave de inventario

El sistema no debe depender únicamente de un campo `stock` dentro de producto.

Un sistema serio de inventario debe tener historial de movimientos. El stock puede mostrarse como cantidad actual, pero cada cambio debe tener una razón registrada.

Ejemplos de movimientos:

- Compra a proveedor.
- Venta a cliente.
- Devolución.
- Ajuste manual.
- Pérdida.
- Robo.
- Producto dañado.
- Transferencia entre almacenes.
- Cancelación de venta.
- Corrección administrativa.

Regla central:

**Todo cambio de stock debe generar un movimiento de inventario.**

Esto permite auditoría, reportes, trazabilidad y corrección de errores.

## 11. Conceptos principales del dominio

### Producto

Artículo general. Ejemplo: Camiseta básica, botella de agua, laptop Lenovo o pan integral.

### SKU

Unidad vendible específica. Ejemplo: Camiseta básica negra talla M, botella de agua 500 ml o laptop Lenovo 16 GB RAM.

El SKU será más importante que el producto para ventas e inventario.

### Categoría

Agrupa productos: bebidas, ropa, tecnología, alimentos, limpieza, etc.

### Proveedor

Empresa o persona que suministra productos. Debe poder relacionarse con compras.

### Compra

Entrada de productos desde proveedor. Una compra aumenta stock cuando se confirma.

### Venta

Salida de productos hacia cliente. Una venta reduce stock cuando se confirma.

### Stock

Cantidad disponible de un SKU en una ubicación. En el MVP puede existir una sola ubicación o almacén principal.

### Movimiento de inventario

Registro histórico de todo cambio de stock. Debe incluir tipo de movimiento, SKU afectado, cantidad, usuario, fecha, motivo y referencia a compra, venta, devolución o ajuste si aplica.

### Almacén

Lugar físico o lógico donde existe stock. En el MVP puede haber solo un almacén llamado “Principal”. En futuras versiones habrá múltiples almacenes y tiendas.

## 12. Módulos principales

### Autenticación

Login, logout lógico desde frontend, protección de rutas, validación de usuario activo y manejo de roles.

### Usuarios y roles

Crear usuarios, editar usuarios, activar/desactivar usuarios, asignar roles y controlar permisos.

Roles iniciales:

- Administrador.
- Vendedor.
- Encargado de almacén.
- Supervisor.

### Productos

Crear, editar, activar/desactivar, asignar categoría, consultar, buscar y filtrar productos.

### SKUs

Crear variantes vendibles, asignar código SKU, precio, costo, estado y consultar stock por SKU.

### Categorías

Crear, editar, activar/desactivar y organizar productos.

### Proveedores

Crear, editar, consultar historial de compras y activar/desactivar proveedores.

### Compras

Crear compra, seleccionar proveedor, agregar SKUs, registrar cantidades/costos, confirmar compra, aumentar stock y generar movimiento de inventario.

### Ventas

Crear venta, agregar SKUs, validar stock disponible, calcular subtotal, aplicar descuento básico si aplica, confirmar venta, reducir stock y generar movimiento de inventario.

### Inventario

Ver stock actual, stock bajo, movimientos, ajuste manual, historial por SKU y filtros por fecha.

### Reportes

Stock bajo, productos más vendidos, ventas por fecha, compras por fecha, movimientos de inventario y valor aproximado del inventario.

### Auditoría

Registrar acciones críticas: crear/modificar producto, confirmar/cancelar venta, confirmar compra, ajustar stock, cambiar permisos, desactivar usuarios y cambiar precios.

## 13. Flujos principales

### Creación de producto

1. Administrador entra al panel.
2. Abre módulo de productos.
3. Crea producto base.
4. Asigna categoría.
5. Crea uno o varios SKUs.
6. Define precio y costo.
7. Guarda producto.
8. El sistema registra auditoría.

### Compra

1. Usuario autorizado crea una compra.
2. Selecciona proveedor.
3. Agrega SKUs.
4. Define cantidades y costos.
5. Guarda compra como borrador o pendiente.
6. Confirma compra.
7. El sistema aumenta stock.
8. El sistema genera movimientos de inventario.
9. El sistema registra auditoría.

### Venta

1. Vendedor crea una venta.
2. Busca productos o SKUs.
3. Agrega productos al carrito.
4. El sistema valida stock.
5. El vendedor confirma venta.
6. El sistema reduce stock.
7. El sistema genera movimientos de inventario.
8. El sistema registra auditoría.
9. El sistema muestra resumen de venta.

### Ajuste de inventario

1. Usuario autorizado selecciona SKU.
2. Indica cantidad de ajuste.
3. Indica motivo obligatorio.
4. El sistema valida permiso.
5. El sistema actualiza stock.
6. El sistema registra movimiento de ajuste.
7. El sistema registra auditoría.

### Stock bajo

1. El sistema compara stock actual con stock mínimo.
2. Detecta productos/SKUs por debajo del mínimo.
3. Los muestra en dashboard o reporte.
4. El encargado puede decidir crear una compra.
5. En futuras versiones, el sistema podrá sugerir reposición.

## 14. Reglas de negocio iniciales

1. No se puede vender un SKU sin stock suficiente, salvo configuración futura de preventa.
2. Toda venta confirmada reduce stock.
3. Toda compra confirmada aumenta stock.
4. Toda modificación manual de stock requiere motivo.
5. No se deben borrar ventas confirmadas.
6. No se deben borrar compras confirmadas.
7. Los productos se desactivan; no se eliminan físicamente si tienen historial.
8. Los usuarios se desactivan; no se eliminan físicamente si tienen historial.
9. Los cambios de precio deben quedar auditados.
10. Un SKU debe ser único.
11. Una venta debe tener al menos un producto.
12. Una compra debe tener al menos un producto.
13. Un movimiento de inventario debe estar asociado a una causa.
14. El administrador tiene acceso completo.
15. El vendedor no puede modificar stock manualmente.
16. El encargado de almacén no puede modificar configuración financiera crítica.
17. El supervisor puede revisar, pero no necesariamente modificar todo.
18. El sistema debe evitar stock negativo.
19. El sistema debe estar preparado para múltiples almacenes.
20. El sistema debe estar preparado para múltiples tiendas.

## 15. Entidades iniciales candidatas

- users
- roles
- permissions
- role_permissions
- categories
- products
- skus
- suppliers
- purchases
- purchase_items
- sales
- sale_items
- customers
- warehouses
- stock_levels
- inventory_movements
- audit_logs
- payment_methods
- sale_payments

No todas tienen que implementarse completas en la primera versión, pero deben analizarse antes de diseñar la base de datos.

## 16. Decisiones técnicas iniciales

### Frontend

React + Vite.

### Backend

Node.js + Express.

### Base de datos

PostgreSQL.

Motivos:

- Mejor base para integridad relacional avanzada.
- Buen soporte de transacciones.
- Buen soporte de constraints.
- Buen soporte de índices avanzados.
- Buen soporte para reportes y consultas complejas.
- Mejor proyección si el sistema crece hacia plataforma empresarial.

### Autenticación

JWT con middleware de autenticación y autorización por rol/permisos.

### Documentación API

OpenAPI / Swagger desde fases tempranas.

### Control de versiones

GitHub. Toda tarea importante debe hacerse en rama separada.

## 17. Metodología con Hermes

Hermes se usará como sistema de agentes.

Agentes iniciales:

- Orquestador principal.
- Arquitecto de software.
- Analista de dominio.
- Diseñador de base de datos.
- Backend planner.
- Frontend planner.
- Tester / QA.
- Reviewer.

El orquestador decide qué agente debe intervenir según la tarea.

## 18. Evaluaciones iniciales para los agentes

Los agentes deben superar casos de prueba antes de ser usados para cambios importantes:

- Detectar stock sin movimiento.
- Bloquear venta con stock insuficiente.
- Detectar compra confirmada sin movimiento.
- Detectar vendedor con permisos excesivos.
- Rechazar eliminación de ventas confirmadas.
- Exigir SKU único.
- Exigir auditoría para acciones críticas.

## 19. Riesgos del proyecto

### Sobreingeniería

Intentar construir algo tipo Amazon desde el inicio puede hacer que el proyecto nunca termine.

### Inventario mal diseñado

Si el stock se maneja solo como número editable, el sistema no será confiable.

### Permisos débiles

Si cualquier usuario puede modificar stock o ventas, el sistema pierde seguridad.

### Falta de auditoría

Sin historial de acciones, no se puede saber quién hizo qué.

### Agentes sin control

Si cada agente decide cosas por separado, se rompe la coherencia.

### Programar demasiado pronto

Si se empieza a programar sin reglas claras, habrá refactorizaciones grandes.

## 20. Criterios de éxito del MVP

El MVP será exitoso si permite:

- Crear usuarios y roles.
- Iniciar sesión.
- Crear productos.
- Crear SKUs.
- Registrar proveedores.
- Registrar compras.
- Aumentar stock mediante compras.
- Registrar ventas.
- Reducir stock mediante ventas.
- Ver stock actual.
- Ver movimientos de inventario.
- Detectar stock bajo.
- Ver reportes básicos.
- Evitar stock negativo.
- Registrar auditoría básica.
- Mantener permisos por rol.

## 21. Orden de construcción recomendado

1. Documento maestro.
2. Skills de proyecto.
3. Agentes en Hermes.
4. Diseño funcional.
5. Modelo de datos PostgreSQL.
6. Arquitectura backend.
7. Arquitectura frontend.
8. Evals de agentes.
9. Repo base.
10. Backend base.
11. Base de datos.
12. Auth.
13. Productos.
14. SKUs.
15. Inventario.
16. Compras.
17. Ventas.
18. Reportes.
19. Auditoría.
20. Revisión general.

## 22. Decisión actual

El proyecto empezará como un sistema modular monolítico con PostgreSQL.

No se usarán microservicios en la primera versión.

La arquitectura debe permitir separar módulos en el futuro si el proyecto crece, pero sin complicar el MVP.

## 23. Estado actual

Estado: fase de planificación inicial.

Todavía no se ha creado código de aplicación.

El siguiente paso será configurar Hermes con las skills y agentes iniciales del repositorio.
