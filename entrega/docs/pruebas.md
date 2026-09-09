# Registro de pruebas

## Evidencia ejecutada

| Caso | Resultado | Evidencia observada |
|---|---|---|
| Mensaje incompleto | Correcto | `Validar datos mínimos` produjo `False Branch (1 item)` y derivó al manejador de errores. No se ejecutó Groq ni Telegram. |
| Lead válido, primer intento | Fallo controlado | Telegram rechazó un texto vacío. El flujo no envió respuesta al cliente. |
| Lead válido, ejecución corregida | Correcto | Groq terminó, Airtable guardó, Telegram envió la solicitud y el flujo quedó esperando HITL. |
| Aprobación humana | Correcto | `HITL - Esperar decisión`, `Router - Aprobado` y `Gmail - Responder en hilo` terminaron en `Success`. |

## Conteo mínimo de estrés

El historial de ejecuciones de n8n muestra al menos cinco ejecuciones exitosas del flujo, además de ejecuciones de validación y error. Las ejecuciones felices observadas incluyen `09 Sep 16:16`, `09 Sep 16:05`, `09 Sep 16:02`, `08 Sep 18:20` y `08 Sep 18:19`.

## Última ejecución exitosa

- Resultado n8n: `Success in 17m 17.384s`.
- Groq: `Success` con `openai/gpt-oss-20b` vía Groq Free.
- Airtable: `Success in 698ms`.
- Telegram: `Success in 200ms`.
- HITL: reanudado mediante enlace firmado `decision=APROBAR`.
- Gmail: `Success in 1.183s`, respuesta en el hilo original.

## Checklist de seguridad

- [x] Filtro de entrada por etiqueta `LEAD_NUEVO`.
- [x] Detección de duplicados por `Message ID`.
- [x] Validación de longitud antes de IA.
- [x] Aprobación humana antes de Gmail Reply.
- [x] Credenciales fuera del JSON técnico.
- [x] Cinco ejecuciones felices observadas en el historial de n8n.
- [x] Dashboard público de KPIs creado en Airtable.
