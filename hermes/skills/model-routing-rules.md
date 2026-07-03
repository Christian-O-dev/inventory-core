# Model Routing Rules

## Objetivo

Reducir consumo de modelos caros durante el desarrollo de InventoryCore.

Hermes debe usar el modelo barato por defecto y solo escalar a un modelo más potente cuando la tarea realmente lo justifique.

---

## Política general

Modelo normal recomendado:

- OpenAI Codex + gpt-5.4-mini.

Modelo avanzado recomendado:

- OpenAI Codex + gpt-5.5.

Gemini:

- Usar solo si está configurado como proveedor real en Hermes mediante API/credenciales compatibles.
- Si se usa Antigravity IDE, tratarlo como herramienta separada, no como proveedor interno de Hermes.

---

## Usar gpt-5.4-mini para

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

Regla:

Si la tarea puede resolverse con bajo riesgo técnico, usar gpt-5.4-mini.

---

## Escalar a gpt-5.5 solo para

- Arquitectura difícil.
- Modelo de datos crítico.
- Errores complejos.
- Revisión antes de migraciones reales.
- Decisiones de seguridad.
- Refactor grande.
- Diseño de transacciones de inventario.
- Cambios que afecten compras, ventas, stock o auditoría.
- Revisión final antes de implementar una fase importante.

Regla:

Antes de usar gpt-5.5, Hermes debe explicar por qué la tarea lo merece y pedir confirmación al usuario.

Formato obligatorio:

```txt
Esta tarea puede requerir GPT-5.5 porque afecta: <motivo>.
Recomiendo cambiar de modelo solo para esta revisión.
Comando sugerido: /model gpt-5.5 --provider openai-codex
```

---

## No usar gpt-5.5 para

- Títulos.
- Resúmenes simples.
- Documentación menor.
- Crear carpetas.
- Crear archivos base.
- Revisar comandos básicos.
- Preguntas simples.
- Cambios cosméticos.
- Listas de tareas.

---

## Regla de retorno al modelo barato

Después de usar gpt-5.5, volver al modelo barato:

```txt
/model gpt-5.4-mini --provider openai-codex
```

Hermes debe recordar al usuario volver al modelo barato cuando termine la revisión crítica.

---

## Regla de seguridad de coste

Hermes no debe activar modelos caros de forma global salvo que el usuario lo pida explícitamente.

No usar:

```txt
/model gpt-5.5 --provider openai-codex --global
```

sin confirmación explícita.

---

## Uso de Gemini

Si Gemini no está configurado dentro de Hermes con API/credenciales compatibles, Hermes no debe asumir que puede usarlo.

Si el usuario usa Gemini mediante Antigravity IDE, Hermes debe tratarlo como flujo externo:

- Hermes planifica y revisa.
- Antigravity puede ejecutar tareas separadas con Gemini.
- VS Code se usa para revisar archivos.

---

## Regla final

Por defecto: gpt-5.4-mini.

Solo escalar: gpt-5.5 con justificación y confirmación.

Después de escalar: volver a gpt-5.4-mini.
