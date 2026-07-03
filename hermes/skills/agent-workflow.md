# Skill: Flujo de trabajo con agentes

## Cuándo usar esta skill

Usar en cualquier tarea donde Hermes deba decidir qué agente utilizar, cómo revisar una decisión o cómo convertir una idea en trabajo seguro.

## Flujo estándar

1. El usuario define una necesidad.
2. El orquestador identifica el objetivo real.
3. El orquestador decide si hace falta usar un agente especializado.
4. El planner divide la tarea en pasos pequeños.
5. El agente especializado diseña la solución.
6. El reviewer busca riesgos, errores y sobreingeniería.
7. El tester/QA propone casos de prueba.
8. Solo después se implementa.
9. Se revisa el resultado.
10. Se actualiza documentación si la decisión cambia el proyecto.

## Reglas para el orquestador

- No dejar que varios agentes tomen decisiones contradictorias.
- No programar sin tarea clara.
- No aprobar cambios destructivos sin confirmación humana.
- No permitir que un agente ignore el documento maestro.
- No usar GPT-5.5 para tareas simples si un modelo auxiliar basta.
- Usar GPT-5.5 para arquitectura, revisión profunda y decisiones críticas.

## Reglas para agentes especializados

Cada agente debe devolver:

- Resumen de la tarea.
- Decisión o propuesta.
- Riesgos.
- Supuestos.
- Próximo paso recomendado.

Cuando sea revisión técnica, también debe devolver:

- Bloqueante: sí/no.
- Severidad.
- Archivos o módulos afectados si aplica.

## Acciones que requieren confirmación humana

- Borrar archivos.
- Borrar datos.
- Cambiar esquema de base de datos de forma destructiva.
- Hacer git push.
- Modificar producción.
- Cambiar arquitectura principal.
- Cambiar stack principal.
- Eliminar historial o auditoría.
