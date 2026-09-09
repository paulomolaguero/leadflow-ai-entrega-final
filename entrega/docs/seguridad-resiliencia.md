# Seguridad y resiliencia

## Minimización de datos

- Enviar al modelo solo nombre, empresa, email, necesidad, presupuesto y urgencia.
- No enviar contraseñas, claves, adjuntos innecesarios ni datos sensibles.
- Guardar en Airtable un resumen operativo, no el contenido completo salvo necesidad de auditoría.
- Mantener credenciales en Credentials de n8n, nunca en campos del workflow.

## Rutas de error

- Validación: `snippet` vacío o de hasta 20 caracteres -> rama `False` -> registro en Errores; no se llama al modelo.
- Groq: reintento limitado; después registrar código y payload mínimo.
- Airtable/Gmail/Telegram: reintento con backoff y registro del nodo fallido.
- Aprobación expirada: marcar `Expirada`, no enviar y notificar al responsable.

## Human-in-the-loop

Telegram recibe el mensaje, el resumen y enlaces firmados `APROBAR` / `RECHAZAR`. El flujo se detiene antes del envío final. Solo la decisión `APROBAR` habilita Gmail Reply.

## Prevención de bucles

- Etiqueta de entrada `LEAD_NUEVO` y filtro de ejecución controlada.
- Consulta por `Message ID` antes de crear interacción.
- Si el `Message ID` ya existe, terminar.
- El correo de respuesta nunca recibe la etiqueta de entrada.
