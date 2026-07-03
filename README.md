# InventoryCore

Sistema web de inventario, ventas y operaciones comerciales diseñado para empezar como solución para una tienda pequeña y crecer hacia una plataforma escalable con múltiples tiendas, almacenes, roles, auditoría, ventas, compras, reportes e integraciones futuras.

## Estado actual

Fase inicial: documentación, arquitectura y configuración de trabajo con Hermes.

Todavía no hay código de aplicación. Esta primera versión del repositorio define la base del producto, las reglas del dominio, los agentes, las skills y los criterios de evaluación.

## Stack decidido para el proyecto

- Frontend: React + Vite.
- Backend: Node.js + Express.
- Base de datos: PostgreSQL.
- API: REST con documentación OpenAPI/Swagger.
- Autenticación: JWT.
- Control de versiones: GitHub.
- Metodología IA: Hermes con agente orquestador, skills y subagentes especializados.

## Estructura inicial

```txt
inventory-core/
  README.md
  docs/
    master-project.md
    architecture-decisions/
      ADR-0001-use-postgresql.md
  hermes/
    skills/
      project-context.md
      architecture-rules.md
      inventory-rules.md
      sales-rules.md
      security-rules.md
      scalability-rules.md
      agent-workflow.md
    agents/
      orchestrator.md
      software-architect.md
      domain-analyst.md
      database-designer.md
      backend-planner.md
      frontend-planner.md
      tester-qa.md
      reviewer.md
  evals/
    agent-evals.md
  backend/
    .gitkeep
  frontend/
    .gitkeep
  database/
    .gitkeep
```

## Principio clave del proyecto

El sistema no debe manejar inventario únicamente modificando un campo `stock` en productos. Todo cambio de stock debe generar un movimiento de inventario con causa, usuario, fecha, cantidad y referencia a compra, venta, devolución, ajuste o transferencia.

## Próximo paso

Configurar Hermes usando los archivos de `hermes/skills` y `hermes/agents` como base de trabajo. Después se diseñará el modelo de datos PostgreSQL antes de escribir código.
