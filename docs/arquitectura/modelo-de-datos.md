# Modelo de datos

> **Dueño de:** qué datos existen, dónde viven, y cómo se aíslan entre sí (regla **#1**).
> **No cubre:** las claves para acceder a la base (→ [`secretos.md`](secretos.md)).

## Dónde viven los datos

Todo vive en una base Postgres administrada por Supabase (ver `stack.md`). No hay archivos
sueltos ni planillas como fuente de verdad en producción — la lista de precios y las FAQs se
cargan a esta base (aunque su origen, para armarlas, sí sea una planilla o un research manual).

## Qué datos existen

### `negocio`
Datos de configuración de la zinguería — se cargan una sola vez al implementar (no hay panel de
administración en el MVP, ver PRD §5).
- Nombre, horario, dirección, medios de pago aceptados, política de descuentos (solo mostrador).
- Enlace oficial de Google del local (regla de atención #12).
- Contacto/tarjeta del colocador (regla de atención #11).
- Tiempo estimado de respuesta a comunicarle al cliente en un pedido a medida (`user-flow.md` §7).

*Aunque el MVP es de un solo negocio, se modela con un identificador de negocio desde el
principio — es gratis hacerlo ahora y evita un reordenamiento si el producto se generaliza a
otras PyMEs más adelante (`docs/solucion.md`), sin que eso implique construir esa generalización
ahora (sigue fuera de alcance, PRD §5).*

### `productos_estandar`
El catálogo de productos con precio y stock fijo (M1).
- Nombre/descripción del producto, precio, stock, y a qué negocio pertenece.
- Se carga desde la lista de precios real de la zinguería (PRD, pregunta abierta #2) — este doc
  no define de dónde sale esa lista, solo que una vez definida, vive acá.

### `faqs`
La lista de preguntas frecuentes del negocio (M1), armada con el research puntual sobre el
historial de WhatsApp de los últimos 6 meses.
- Pregunta (genérica) + respuesta.
- **Nunca contiene** nombres, teléfonos ni fragmentos de conversaciones reales — el research que
  las genera es un proceso manual, previo y externo a esta base (PRD M1, "regla de privacidad").
  Lo único que entra a esta tabla es el resultado ya anonimizado.

### `pedidos_a_medida`
Cada pedido a medida que el agente arma y deriva a un vendedor (M2).
- Estimación dada (material, calibre, precio aproximado).
- Medidas exactas (cuando el cliente las da).
- Datos de contacto del interlocutor único identificado (regla de atención #8).
- Tipo de cliente declarado: Consumidor Final, Gremio, Mayorista, Profesional o Empresa
  Constructora (regla de atención #13) — este campo guarda lo que el cliente dijo; la
  clasificación **definitiva** la confirma el vendedor por fuera de esta tabla, al cerrar la
  venta (PRD §5).
- Referencia a los archivos multimedia recibidos (foto/video/bosquejo) — se guarda la referencia
  al archivo, no una interpretación de su contenido (PRD §5, regla de atención #9).
- Estado del pedido: nuevo → derivado al vendedor → cerrado.

### `conversaciones` (mensajes de la conversación en curso)
El agente necesita poder "leer toda la conversación antes de responder" (regla de atención #1) y
"no repreguntar un dato que el cliente ya dio" (`marca.md` §4) — para eso guarda el historial de
la conversación **activa** de cada cliente con el negocio.

**Esto es distinto del "historial de los últimos 6 meses" que el PRD prohíbe conectar al agente
en vivo (PRD §5).** La diferencia:
- El historial de 6 meses es un **archivo histórico externo**, de conversaciones ya cerradas,
  que se usó una sola vez para *investigar* qué preguntas frecuentes existen — nunca se carga acá.
- Esta tabla es la **memoria operativa normal** del agente sobre la conversación que está
  teniendo *en este momento* con un cliente — es indispensable para que el agente funcione (sin
  esto, no podría cumplir la regla #1), y es del mismo cliente con el que está hablando, no de
  terceros.

## Cómo se aíslan los datos entre sí (regla #1)

En este MVP hay un solo negocio, así que el aislamiento relevante hoy es entre **distintos
clientes de ese mismo negocio**: la conversación, el pedido y los datos de contacto de un
cliente nunca se mezclan con los de otro — cada fila de `conversaciones` y `pedidos_a_medida`
está atada a un cliente puntual (su número de WhatsApp) y solo el vendedor humano (no otro
cliente) puede verla.

Si el producto se generaliza a más de una PyME más adelante (fuera de alcance de este MVP), el
aislamiento se extiende al nivel de negocio: todas las tablas ya tienen su `negocio_id`, así que
ningún dato de una PyME sería visible para otra.

## Abierto

- De dónde sale exactamente la lista de precios/stock de `productos_estandar` (formato, quién la
  actualiza) — PRD, pregunta abierta #2.
- Quién/qué recibe los registros de `pedidos_a_medida` en la práctica (ver `integraciones.md`) —
  PRD, pregunta abierta #3.
