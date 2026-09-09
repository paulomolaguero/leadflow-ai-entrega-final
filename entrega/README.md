# LeadFlow AI

Ecosistema autónomo de calificación y seguimiento de leads para AutomatizaPro.

## Flujo

1. Gmail detecta un correo nuevo con la etiqueta `LEAD_NUEVO`.
2. n8n valida datos mínimos y evita reprocesar el mismo `message_id`.
3. Airtable aporta memoria del contacto y del historial.
4. Groq ejecuta `openai/gpt-oss-20b` para extraer datos y clasificar el lead en JSON estructurado.
5. Airtable guarda el resultado y el borrador.
6. Telegram solicita aprobación humana.
7. Solo con aprobación, Gmail responde dentro del mismo hilo.
8. La ruta de aprobación responde en el mismo hilo de Gmail; las rutas inválidas se envían al registro de errores.

## Entregables

- [Arquitectura PDF](docs/arquitectura-leadflow-ai.pdf)
- [Manual operativo](docs/manual-operativo.md)
- [Matriz de costos](docs/matriz-costos.md)
- [Seguridad y resiliencia](docs/seguridad-resiliencia.md)
- [Blueprint n8n](n8n/leadflow-ai.json)

## Dashboard público

[Abrir Dashboard KPIs - LeadFlow AI en Airtable](https://airtable.com/app6PrhsqGVfwPsZG/shrVqnyBYEfrSVFdb)

## Repositorio

[Abrir repositorio GitHub](https://github.com/paulomolaguero/leadflow-ai-entrega-final)

La vista compartida permite revisar los leads procesados, el estado, la urgencia y el score. La tabla `Errores` concentra la tasa de fallos.

## Estado de implementación

- n8n está conectado a Gmail, Airtable, Groq vía credencial compatible y Telegram.
- La prueba con datos incompletos produce `False Branch` y no llama a Groq.
- La prueba completa guarda la clasificación, solicita aprobación, espera el HITL y responde en el hilo de Gmail.
- El flujo usa el chat de Telegram configurado en n8n y no expone claves en el blueprint.

## Pendientes de entrega

- Anexar capturas reales de las pruebas y del dashboard.
- Exportar el JSON definitivo desde n8n y sustituir este respaldo técnico.
- Generar el PDF final cuando la evidencia esté cerrada.
- Publicar el enlace del repositorio GitHub final.

## Modelo

La ruta principal usa Groq con `openai/gpt-oss-20b`, dentro del plan gratuito y con límites conservadores. No se incluyen claves ni tokens.
