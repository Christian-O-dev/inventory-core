# Model Routing Rules

## Objetivo

Reducir consumo de modelos caros durante el desarrollo de InventoryCore.

Hermes debe usar el modelo más barato suficiente para cada tarea y solo escalar cuando la tarea realmente lo justifique.

Esta skill no es una barrera técnica absoluta del sistema. Es una regla de orquestación del proyecto: Hermes debe seguirla al planificar, revisar y ejecutar tareas.

---

## Modelos disponibles en OpenAI Codex

Aliases recomendados:

```txt
fast   = OpenAI Codex + gpt-5.4-mini
normal = OpenAI Codex + gpt-5.4
deep   = OpenAI Codex + gpt-5.5
```

Comandos de configuración recomendados:

```bash
hermes config set model.aliases.fast openai-codex/gpt-5.4-mini
hermes config set model.aliases.normal openai-codex/gpt-5.4
hermes config set model.aliases.deep openai-codex/gpt-5.5
```

Modelo global recomendado:

```txt
fast
```

No configurar `deep` como modelo global salvo orden explícita del usuario.

---

## Política general

Usar `fast` por defecto.

Usar `normal` cuando la tarea requiera más criterio técnico, pero no sea crítica.

Usar `deep` solo para decisiones importantes, revisiones críticas o problemas complejos.

Antes de escalar a `deep`, Hermes debe justificarlo y pedir confirmación.

---

## Usar fast para

Modelo:

```txt
gpt-5.4-mini
```

Tareas:

- Fase 0.
- Estructura inicial.
- Documentación.
- Comandos simples.
- Revisión simple.
- Crear archivos base.
- Crear checklists.
- Resumir documentos.
- Preparar prompts.
- Revisar nombres de carpetas.
- Tareas repetitivas.
- Cambios pequeños.
- Crear `.env.example`.
- Crear documentación de comandos.
- Preparar carpetas sin lógica de negocio.

Regla:

Si la tarea puede resolverse con bajo riesgo técnico, usar `fast`.

---

## Usar normal para

Modelo:

```txt
gpt-5.4
```

Tareas:

- Revisar estructura backend base.
- Revisar estructura frontend base.
- Revisar configuración Docker no crítica.
- Debugging medio.
- Decisiones técnicas intermedias.
- Revisión de pull requests pequeños.
- Revisión de arquitectura modular básica.
- Revisión de comandos antes de ejecutarlos.
- Diseño de carpetas cuando involucre backend y frontend juntos.
- Preparar fase 1 antes de pasar a implementación real.

Regla:

Si una tarea puede afectar la calidad técnica, pero no toca reglas críticas de negocio, usar `normal`.

---

## Usar deep solo para

Modelo:

```txt
gpt-5.5
```

Tareas:

- Arquitectura difícil.
- Modelo de datos crítico.
- Revisión antes de migraciones reales.
- Seguridad.
- Permisos y roles.
- Autenticación.
- Transacciones de inventario.
- Reglas de stock.
- Confirmación de compras.
- Confirmación de ventas.
- Auditoría.
- Errores complejos.
- Refactor grande.
- Revisión final antes de implementar una fase importante.
- Decisiones que puedan ser difíciles de cambiar después.

Regla:

Antes de usar `deep`, Hermes debe explicar por qué la tarea lo merece y pedir confirmación al usuario.

Formato obligatorio:

```txt
Esta tarea puede requerir deep porque afecta: <motivo>.
Recomiendo cambiar de modelo solo para esta revisión.
Comando sugerido: /model deep
¿Confirmas que use deep para esta parte?
```

---

## No usar deep para

- Títulos.
- Resúmenes simples.
- Documentación menor.
- Crear carpetas.
- Crear archivos base.
- Revisar comandos básicos.
- Preguntas simples.
- Cambios cosméticos.
- Listas de tareas.
- Generar prompts.
- Tareas de fase 0.

---

## Regla de retorno al modelo barato

Después de usar `deep`, volver a `fast`:

```txt
/model fast
```

Hermes debe recordar al usuario volver al modelo barato cuando termine la revisión crítica.

---

## Regla de seguridad de coste

Hermes no debe activar modelos caros de forma global salvo que el usuario lo pida explícitamente.

No usar:

```txt
/model deep --global
```

sin confirmación explícita.

---

## Gemini y Antigravity

Si Gemini no está configurado dentro de Hermes con API o credenciales compatibles, Hermes no debe asumir que puede usarlo como proveedor interno.

Si el usuario usa Gemini mediante Antigravity IDE, Hermes debe tratarlo como flujo externo:

- Hermes planifica, revisa y orquesta.
- Antigravity puede ejecutar tareas separadas con Gemini.
- VS Code se usa para revisar archivos.

No mezclar responsabilidades sin que el usuario lo pida.

---

## Flujo recomendado en InventoryCore

1. Iniciar siempre con `fast`.
2. Leer documentación del proyecto.
3. Planificar tareas pequeñas.
4. Ejecutar cambios simples con `fast`.
5. Usar `normal` para revisión técnica intermedia.
6. Pedir confirmación antes de usar `deep`.
7. Usar `deep` solo para revisión crítica.
8. Volver a `fast` al terminar.

---

## Regla final

Por defecto: `fast`.

Para revisión técnica intermedia: `normal`.

Para decisiones críticas: `deep` con justificación y confirmación.

Después de escalar: volver a `fast`.
