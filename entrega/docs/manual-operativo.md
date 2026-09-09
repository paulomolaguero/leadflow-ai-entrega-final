# Manual operativo de datos

## Caso de uso

AutomatizaPro recibe consultas comerciales por Gmail. LeadFlow AI interpreta el mensaje, guarda memoria en Airtable, propone una respuesta y espera aprobación en Telegram antes de contactar al cliente.

## Tablas Airtable

### Leads

`Lead ID` (texto, único), `Nombre`, `Email`, `Necesidad`, `Segmento`, `Estado`, `Urgencia`, `Gmail Thread ID`, `Score` (número 0-100), `Última interacción`.

### Interacciones

`Interacción ID`, `Lead` (enlace), `Tipo` (Entrada/Borrador/Respuesta/Error), `Message ID`, `Thread ID`, `Contenido resumido`, `Modelo`, `Tokens estimados`, `Fecha`, `Resultado`.

### Aprobaciones

`Aprobación ID`, `Lead` (enlace), `Estado` (Pendiente/Aprobado/Rechazado/Expirada), `Canal`, `Telegram Message ID`, `Revisor`, `Comentario`, `Fecha de solicitud`, `Fecha de decisión`.

### Errores

`Error ID`, `Lead` (enlace opcional), `Nodo`, `Código`, `Mensaje`, `Payload mínimo`, `Reintentos`, `Estado` (Nuevo/Resuelto), `Fecha`.

## Esquema de transferencia IA

```json
{
  "lead_id": "lead_{{messageId}}",
  "contact": {"name": "string", "email": "string", "company": "string"},
  "need": "string",
  "budget": 0,
  "urgency": "low|medium|high",
  "segment": "vip|qualified|follow_up|discarded",
  "score": 0,
  "missing_fields": [],
  "draft_reply": "string",
  "confidence": 0.0
}
```

## Reglas

- Solo procesar mensajes con etiqueta `LEAD_NUEVO`.
- Si el `snippet` tiene 20 caracteres o menos, registrar error y no contactar.
- Si `message_id` ya existe en Interacciones, terminar el flujo.
- Toda respuesta pasa por aprobación humana antes del envío.
- El envío final usa el `Thread ID` original.

## Dashboard de control

Vista pública de lectura: [Dashboard KPIs - LeadFlow AI](https://airtable.com/app6PrhsqGVfwPsZG/shrVqnyBYEfrSVFdb)

La vista compartida concentra los registros de `Leads`, permite revisar el estado, la urgencia y el score, y sirve como evidencia pública del monitoreo. La tasa de errores se obtiene desde la tabla `Errores` y se documenta junto con el historial de ejecuciones de n8n.
