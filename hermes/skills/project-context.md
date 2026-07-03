# Skill: Contexto del proyecto InventoryCore

## Cuándo usar esta skill

Usar esta skill en cualquier tarea relacionada con el proyecto InventoryCore, especialmente si la tarea afecta arquitectura, inventario, ventas, compras, base de datos, frontend, backend o decisiones de producto.

## Resumen

InventoryCore es un sistema web de inventario y ventas diseñado para empezar como solución para una tienda pequeña y crecer hacia una plataforma escalable con múltiples almacenes, tiendas, roles, auditoría, ventas online, reportes e integraciones futuras.

## Stack actual decidido

- Frontend: React + Vite.
- Backend: Node.js + Express.
- Base de datos: PostgreSQL.
- API: REST con OpenAPI/Swagger.
- Autenticación: JWT.
- Metodología IA: Hermes con agente orquestador, skills y subagentes.

## Alcance del MVP

El MVP debe incluir:

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

## Fuera del MVP

No implementar inicialmente:

- Marketplace tipo Amazon.
- Microservicios.
- App móvil.
- IA predictiva.
- Facturación legal avanzada.
- Ecommerce completo.
- Multiempresa SaaS.
- Logística avanzada.
- Reposición automática.

## Principio clave

El inventario no debe depender solo de editar un campo `stock`. Todo cambio de stock debe generar un movimiento de inventario con causa, usuario, fecha, cantidad y referencia.

## Regla para los agentes

No proponer código ni estructura técnica que contradiga este contexto sin explicar claramente el motivo y pedir validación humana.
