# LeadFlow AI

Ecosistema autónomo de calificación y seguimiento de leads para AutomatizaPro.

## Flujo

1. Gmail detecta un correo nuevo con la etiqueta `LEAD_NUEVO`.
2. n8n valida datos mínimos y evita reprocesar el mismo `message_id`.
3. Airtable aporta memoria del contacto y del historial.
4. Groq ejecuta `openai/gpt-oss-20b` para clasificar el lead en JSON estructurado.
5. Airtable guarda el resultado y el borrador.
6. Telegram solicita aprobación humana.
7. Solo con aprobación, Gmail responde dentro del mismo hilo.
8. Las rutas inválidas se envían al registro de errores.

## Entregables

- [Arquitectura PDF](docs/arquitectura-leadflow-ai.pdf)
- [Manual operativo](docs/manual-operativo.md)
- [Matriz de costos](docs/matriz-costos.md)
- [Seguridad y resiliencia](docs/seguridad-resiliencia.md)
- [Exportación definitiva n8n](n8n/leadflow-ai-final.json)

## Dashboard público

[Abrir Dashboard KPIs - LeadFlow AI en Airtable](https://airtable.com/app6PrhsqGVfwPsZG/shrVqnyBYEfrSVFdb)

## Video Demo

[Ver demostración del flujo en Google Drive](https://drive.google.com/file/d/16QCogZNhmfWv1JO907116svgum0dwNqW/view?usp=sharing)

## Evidencias

Las capturas reales están disponibles en la carpeta [capturas](capturas/): flujo n8n, configuración Groq, HITL en Telegram, estructura Airtable, corrección del campo Name, ejecución exitosa y camino de error.

## Estado de implementación

- n8n está conectado a Gmail, Airtable, Groq vía credencial compatible y Telegram.
- La prueba con datos incompletos produce `False Branch` y no llama a Groq.
- La prueba completa guarda la clasificación, solicita aprobación, espera el HITL y responde en el hilo de Gmail.
- El blueprint publicado no contiene claves ni tokens.
- El repositorio contiene las evidencias técnicas y el export definitivo.

## Repositorio

[Abrir repositorio GitHub](https://github.com/paulomolaguero/leadflow-ai-entrega-final)

## Modelo

La ruta principal usa Groq con `openai/gpt-oss-20b`, dentro del plan gratuito y con límites conservadores.
