# Phase 0 Setup

## 1. Objetivo

Este documento define la fase 0 de preparación local para InventoryCore.

La fase 0 no implementa lógica de negocio. Su objetivo es dejar preparado el entorno, la estructura del proyecto, las decisiones técnicas iniciales y el flujo de trabajo antes de empezar a programar backend, frontend y base de datos.

---

## 2. Resultado esperado de la fase 0

Al terminar esta fase, el proyecto debe tener:

- Repositorio clonado localmente.
- Rama de trabajo creada.
- Estructura base preparada.
- PostgreSQL disponible para desarrollo local.
- Decisión tomada entre JavaScript y TypeScript.
- Backend preparado para inicializarse.
- Frontend preparado para inicializarse.
- Archivos de entorno definidos como plantilla.
- Documentación revisada con Hermes.
- Ninguna lógica de inventario implementada todavía.

---

## 3. Herramientas necesarias

Herramientas recomendadas para desarrollo local:

- Git.
- GitHub.
- VS Code.
- Node.js versión LTS.
- npm o pnpm.
- PostgreSQL local o PostgreSQL en Docker.
- Docker Desktop opcional pero recomendado.
- Hermes configurado con el proyecto.
- Cliente de base de datos como DBeaver, TablePlus, pgAdmin o extensión de VS Code.

---

## 4. Clonar el repositorio

Comando base:

```bash
git clone https://github.com/Christian-O-dev/inventory-core.git
cd inventory-core
```

Actualizar si ya está clonado:

```bash
git pull origin main
```

---

## 5. Flujo de ramas

No trabajar directamente en main cuando empiece la implementación.

Ramas recomendadas:

```txt
main
  docs/*
  setup/*
  backend/*
  frontend/*
  database/*
  feature/*
```

Ejemplos:

```bash
git switch -c setup/project-structure
git switch -c database/initial-schema
git switch -c backend/base-api
git switch -c frontend/base-app
```

Regla:

- main debe mantenerse estable.
- Cada tarea importante debe hacerse en una rama.
- Cada commit debe tener un mensaje claro.

---

## 6. Estructura objetivo inicial

Estructura actual esperada:

```txt
inventory-core/
  README.md
  docs/
  hermes/
  evals/
  backend/
  frontend/
  database/
```

Estructura futura cuando se inicialice código:

```txt
inventory-core/
  backend/
    src/
    tests/
    package.json
    .env.example
  frontend/
    src/
    public/
    package.json
    .env.example
  database/
    migrations/
    seeds/
    docs/
  docs/
  hermes/
  evals/
```

---

## 7. Decisión pendiente: JavaScript o TypeScript

Antes de crear backend y frontend se debe decidir:

- Opción A: JavaScript.
- Opción B: TypeScript.

Recomendación:

Usar TypeScript.

Motivos:

- Mejor control de modelos de datos.
- Menos errores al consumir la API.
- Mejor mantenimiento cuando crezcan módulos.
- Mejor integración con DTOs, formularios y servicios.
- Más profesional para un sistema de inventario escalable.

Decisión propuesta:

**InventoryCore debería usar TypeScript en backend y frontend.**

Esta decisión debe confirmarse antes de crear código.

---

## 8. PostgreSQL local

PostgreSQL será la base de datos oficial.

Opciones de desarrollo:

### Opción A: PostgreSQL instalado localmente

Ventajas:

- Simple si ya está instalado.
- Menos dependencia de Docker.

Desventajas:

- Puede variar entre equipos.
- Más difícil replicar entorno exacto.

### Opción B: PostgreSQL con Docker

Ventajas:

- Entorno más reproducible.
- Fácil de reiniciar.
- Fácil de compartir entre casa e instituto.

Desventajas:

- Requiere Docker funcionando.

Recomendación:

Usar PostgreSQL con Docker para desarrollo.

---

## 9. Base de datos de desarrollo

Nombre recomendado para la base local:

```txt
inventory_core_dev
```

Usuarios y valores locales se definirán en archivos de entorno, nunca en el código.

Regla:

No subir archivos reales de entorno al repositorio.

Sí subir plantillas:

```txt
backend/.env.example
frontend/.env.example
```

---

## 10. Variables de entorno candidatas

### Backend

Variables candidatas para plantilla:

```txt
NODE_ENV=development
PORT=3000
DATABASE_URL=postgresql://local_user:local_value@localhost:5432/inventory_core_dev
JWT_EXPIRES_IN=1d
CORS_ORIGIN=http://localhost:5173
```

Notas:

- `DATABASE_URL` será solo ejemplo.
- Los valores reales deben ir en `.env` local.
- `.env` debe estar ignorado por Git.

### Frontend

Variables candidatas para plantilla:

```txt
VITE_API_URL=http://localhost:3000
```

---

## 11. .gitignore esperado

El proyecto debe ignorar:

```txt
node_modules/
.env
.env.local
.env.*.local
dist/
build/
coverage/
.DS_Store
```

Regla:

No subir dependencias, builds, claves locales ni archivos temporales.

---

## 12. Orden de inicialización técnica

Orden recomendado cuando se empiece a crear código:

1. Crear rama `setup/project-structure`.
2. Confirmar TypeScript o JavaScript.
3. Crear configuración base de PostgreSQL local.
4. Crear plantilla de entorno backend.
5. Crear plantilla de entorno frontend.
6. Inicializar backend.
7. Inicializar frontend.
8. Crear endpoint básico de health.
9. Probar conexión backend.
10. Preparar estructura de migraciones.
11. Crear primera migración después de validar modelo de datos.
12. Crear documentación de comandos.

---

## 13. Hermes en fase 0

Antes de escribir código, Hermes debe revisar:

- `docs/master-project.md`
- `docs/data-model/postgresql-v0.1.md`
- `docs/api/backend-modules-v0.1.md`
- `docs/frontend/frontend-modules-v0.1.md`
- `hermes/skills/`
- `hermes/agents/`

Agentes recomendados para esta fase:

- Orquestador.
- Arquitecto de software.
- Diseñador de base de datos.
- Backend planner.
- Frontend planner.
- Reviewer.

Objetivo de la revisión:

- Detectar contradicciones.
- Detectar módulos faltantes.
- Confirmar si el MVP está demasiado grande.
- Confirmar si PostgreSQL está bien integrado en las decisiones.
- Confirmar si el orden de implementación es correcto.

---

## 14. Checklist de fase 0

Antes de pasar a fase 1, verificar:

- [ ] Repo clonado correctamente.
- [ ] Rama de trabajo creada.
- [ ] Documentación principal leída.
- [ ] PostgreSQL disponible localmente.
- [ ] Decisión TypeScript o JavaScript tomada.
- [ ] Estrategia de entorno local decidida.
- [ ] `.env.example` planificado.
- [ ] `.env` real no se subirá al repo.
- [ ] Hermes configurado con skills y agentes.
- [ ] Modelo de datos revisado.
- [ ] Módulos backend revisados.
- [ ] Módulos frontend revisados.

---

## 15. Criterios para pasar a fase 1

Se puede pasar a fase 1 cuando:

1. El entorno local esté claro.
2. Se haya decidido TypeScript o JavaScript.
3. PostgreSQL esté disponible.
4. La documentación base esté validada.
5. El equipo tenga claro el orden de implementación.

Fase 1 propuesta:

`docs/implementation/phase-1-project-skeleton.md`

Esa fase definirá cómo crear la estructura real de backend, frontend y database, ya con comandos concretos y primeros archivos técnicos, pero sin lógica completa de negocio.
