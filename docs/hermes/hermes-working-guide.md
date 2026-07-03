# Hermes Working Guide

## 1. Objetivo

Esta guía explica cómo continuar trabajando en InventoryCore usando Hermes como herramienta principal.

El objetivo es que Hermes planifique, revise y ejecute tareas pequeñas, mientras VS Code se usa principalmente para inspeccionar archivos, revisar cambios y resolver detalles manuales.

---

## 2. Modelos configurados

Aliases recomendados:

```txt
fast   = gpt-5.4-mini
normal = gpt-5.4
deep   = gpt-5.5
```

Uso recomendado:

```txt
fast   = tareas normales y baratas
normal = revisión técnica intermedia
deep   = revisión crítica con confirmación
```

Regla principal:

Empezar siempre con `fast`.

---

## 3. Comandos para configurar aliases

Ejecutar una vez en terminal:

```bash
hermes config set model.aliases.fast openai-codex/gpt-5.4-mini
hermes config set model.aliases.normal openai-codex/gpt-5.4
hermes config set model.aliases.deep openai-codex/gpt-5.5
```

Después seleccionar `gpt-5.4-mini` como modelo principal global:

```bash
hermes model
```

Seleccionar:

```txt
OpenAI Codex -> gpt-5.4-mini
```

---

## 4. Abrir Hermes desde el proyecto

Siempre entrar primero al repo:

```bash
cd /ruta/a/inventory-core
```

Actualizar:

```bash
git pull origin main
```

Crear o entrar en rama de trabajo:

```bash
git switch -c setup/typescript-docker
```

Si ya existe:

```bash
git switch setup/typescript-docker
```

Abrir Hermes:

```bash
hermes chat
```

---

## 5. Prompt inicial para Hermes

Pegar al iniciar una sesión nueva:

```txt
Trabaja en el proyecto InventoryCore.

Primero lee:
- hermes/skills/model-routing-rules.md
- README.md
- docs/master-project.md
- docs/implementation/phase-0-setup.md
- docs/architecture-decisions/ADR-0001-use-postgresql.md
- docs/architecture-decisions/ADR-0002-use-typescript-and-docker.md
- docs/data-model/postgresql-v0.1.md
- docs/api/backend-modules-v0.1.md
- docs/frontend/frontend-modules-v0.1.md

Reglas de modelos:
- Usa fast por defecto.
- Usa normal solo para revisión técnica intermedia.
- No uses deep sin justificarlo y pedirme confirmación.
- No uses deep como global.
- Después de usar deep, recuérdame volver a fast.

Objetivo actual:
Preparar fase 0 y luego fase 1 usando TypeScript, Docker y PostgreSQL.

Restricciones:
- No implementar lógica de negocio todavía.
- No crear endpoints reales todavía sin plan.
- No crear migraciones reales sin revisión deep.
- No borrar archivos sin confirmación.
- No hacer push sin confirmación.

Antes de modificar archivos, dame el plan exacto.
```

---

## 6. Primeras tareas para Hermes

Orden recomendado:

1. Revisar contradicciones en la documentación.
2. Confirmar que TypeScript y Docker están correctamente definidos.
3. Preparar fase 0 local.
4. Crear docker-compose para PostgreSQL.
5. Crear `.env.example` para backend.
6. Crear `.env.example` para frontend.
7. Crear estructura base de carpetas.
8. Crear documentación de comandos locales.
9. Revisar todo con `normal`.
10. Hacer commit local.

---

## 7. Qué debe hacer Hermes con fast

Usar `fast` para:

- Crear carpetas.
- Crear archivos base.
- Escribir documentación simple.
- Preparar checklists.
- Preparar `.env.example`.
- Preparar `docker-compose.yml` simple.
- Revisar comandos básicos.
- Resumir documentos.

---

## 8. Qué debe hacer Hermes con normal

Usar `normal` para:

- Revisar estructura completa de fase 0.
- Validar que Docker y TypeScript estén bien integrados.
- Revisar que no se haya creado lógica de negocio antes de tiempo.
- Revisar configuración backend/frontend inicial.
- Preparar fase 1.

Comando:

```txt
/model normal
```

Volver después a:

```txt
/model fast
```

---

## 9. Qué debe hacer Hermes con deep

Usar `deep` solo para:

- Modelo de datos final.
- Migraciones reales.
- Seguridad.
- Roles y permisos.
- Reglas de stock.
- Compras y ventas confirmadas.
- Transacciones.
- Refactor grande.
- Revisión crítica antes de una fase importante.

Comando:

```txt
/model deep
```

Volver después a:

```txt
/model fast
```

Regla:

No usar `deep` sin confirmación explícita.

---

## 10. Uso de VS Code

VS Code se usará para:

- Revisar archivos.
- Ver cambios con Git.
- Editar detalles pequeños si hace falta.
- Revisar estructura.
- Abrir terminal.
- Comparar cambios.

No es necesario usar Codex en VS Code si Hermes será la herramienta principal.

---

## 11. Uso de este chat externo

Usar este chat solo cuando:

- El proyecto haya avanzado bastante.
- Se necesite revisión profunda de la repo.
- Haya errores complejos.
- Se quiera auditar arquitectura.
- Se quiera validar decisiones grandes.

Para trabajo diario, usar Hermes.

---

## 12. Primer comando recomendado dentro de Hermes

Después del prompt inicial, pedir:

```txt
Ejecuta una revisión de fase 0 usando fast. No modifiques archivos todavía. Dime qué tareas exactas harás y en qué orden.
```

Después de aprobar el plan:

```txt
Ejecuta solo la primera tarea del plan. No avances a la siguiente sin mostrarme los cambios.
```

Regla:

Trabajar en pasos pequeños, no en tareas gigantes.
