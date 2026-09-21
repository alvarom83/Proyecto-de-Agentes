# Integraciones con IA

> **Dueño de:** qué modelo se usa, para qué, y el techo de gasto que se chequea ANTES de gastar
> (regla **#3**).

## Qué modelo se usa y para qué

**Claude (Anthropic)** es el modelo que interpreta cada mensaje del cliente y redacta la
respuesta del agente. Tareas concretas que resuelve:

1. **Clasificar la intención** del mensaje: pregunta frecuente del negocio (Rama A), producto
   estándar (Rama B) o pedido a medida (Rama C) — ver `user-flow.md` §3.
2. **Redactar la respuesta** siguiendo las reglas de atención transversales del PRD y el tono de
   `marca.md` (institucional, en plural, un tema por mensaje, etc.).
3. **Extraer datos estructurados** de un pedido a medida (medidas, tipo de cliente, datos de
   contacto) a partir de lo que el cliente escribe en lenguaje natural, sin exigirle un formato.
4. **Calcular la estimación** de un pedido a medida usando material/calibre por defecto cuando
   no se especifica (regla de atención #4).

**Qué modelo puntual:** se recomienda **Claude Haiku** — es el modelo más económico de la
familia y alcanza de sobra para esta tarea, porque es un dominio acotado (un negocio, un
catálogo, reglas de atención bien definidas), no una conversación abierta. Si en el uso real se
detectan errores de clasificación o de cálculo de la estimación, se puede subir a un modelo más
capaz (Claude Sonnet) para esa tarea puntual sin cambiar el resto de la arquitectura.

## El techo de gasto (regla #3), en criollo

La regla dice que **antes** de que el agente le pida algo a la IA, el sistema tiene que chequear
que no se pasó de un límite de gasto — no alcanza con enterarse después de la factura.

**Mecanismo propuesto:**
1. **Límite de tamaño por respuesta:** cada respuesta del agente tiene un tope de longitud (ligado
   a la buena práctica de Meta de mensajes cortos, PRD M1) — esto además de ser mejor para el
   cliente, acota el costo de cada llamada individual.
2. **Techo de gasto mensual con corte automático:** se configura un límite de gasto mensual en la
   plataforma de Anthropic (o un contador propio en la base de datos que se revisa antes de cada
   llamada). Si se alcanza el techo, el agente deja de responder con IA y en su lugar manda un
   mensaje fijo avisando que un vendedor va a responder — nunca deja al cliente sin respuesta,
   solo deja de usar IA hasta el mes siguiente o hasta que el dueño lo reactive.
3. El número exacto del techo mensual lo termina de fijar `/costo` en
   `docs/arquitectura/costos.md`, una vez estimado el volumen real de conversaciones.

## Abierto

- El valor concreto del techo de gasto mensual — depende de `/costo`.
- Si conviene sumar una revisión periódica de conversaciones reales para ajustar el prompt del
  agente (relacionado con el riesgo #6 del PRD, sobre la calidad de las FAQs con el tiempo).
