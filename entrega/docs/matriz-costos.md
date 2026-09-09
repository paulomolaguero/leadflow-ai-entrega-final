# Matriz de decisión y costos

La ruta principal usa Groq en el plan gratuito con costo objetivo cero. La disponibilidad y los límites del plan Free pueden variar, por lo que el flujo debe registrar errores y permitir un fallback controlado.

| Tarea | Modelo recomendado | Motivo | Control de costo |
|---|---|---|---|
| Extraer campos | `openai/gpt-oss-20b` vía Groq | JSON corto y repetitivo | Max 350 tokens |
| Clasificar lead | `openai/gpt-oss-20b` vía Groq | Reglas y score simples | Temperature baja |
| Redactar respuesta | `openai/gpt-oss-20b` vía Groq | Texto comercial breve | Max 350 tokens |
| Lead VIP o baja confianza | `openai/gpt-oss-20b` vía Groq Free | Mantener costo cero en esta entrega | Revisión humana obligatoria |
| Mensaje inválido o duplicado | Sin modelo | Evita consumo innecesario | Filtros previos a Groq |
| Procesamiento masivo | No habilitado | Fuera del alcance operativo actual | Evaluar Batch en una versión paga |

## Decisión

La ruta normal usa `openai/gpt-oss-20b` vía Groq para clasificación y redacción breve. En esta entrega no se habilita una ruta paga: si se necesitara mayor confiabilidad, se documentaría como alternativa separada y requeriría aprobación.

Registrar por ejecución: `modelo`, `tokens_entrada_estimados`, `tokens_salida_estimados`, `costo_estimado`, `duración_ms` y `ruta`. Para la ruta actual: `modelo=openai/gpt-oss-20b`, `proveedor=Groq Free`, `max_tokens=700`, `temperature=0.1`, `costo_estimado=0`.
