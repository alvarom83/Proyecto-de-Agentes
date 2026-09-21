# M1-03 · El agente clasifica y responde (FAQs + productos estándar)

**Depende de:** M1-01, M1-02

## 1. Qué queremos lograr

Cuando un cliente le escribe al WhatsApp del negocio preguntando por un producto estándar o algo
frecuente del negocio, recibe la respuesta correcta al instante — y si el agente no sabe la
respuesta, avisa que un vendedor va a contestar, sin inventar nada. Esta pieza es la que cumple,
de punta a punta, las condiciones de "terminado" de M1 en `docs/prd.md`.

## 2. Qué hay hoy

El canal de WhatsApp ya recibe y puede responder mensajes (M1-01, hoy con una respuesta fija); la
base de datos ya tiene las tablas de catálogo, FAQs y conversaciones (M1-02). Esta pieza conecta
ambas cosas con la inteligencia real (Claude, según `docs/arquitectura/integraciones-ia.md`) y
reemplaza la respuesta fija de M1-01 por la respuesta real.

## 3. Qué tiene que hacer (y cómo comprobamos cada cosa)

| # | Qué hace | Cómo se comprueba |
|---|---|---|
| 1 | Al recibir un mensaje, lo guarda en `conversaciones` y lee todo el hilo previo con ese mismo número antes de responder (regla de atención #1 del PRD) | Mandar dos mensajes seguidos donde el segundo depende del contexto del primero (ej. "¿tienen chapa acanalada?" y luego "¿y el horario de hoy?") y verificar que responde ambos correctamente sin perder el hilo |
| 2 | Clasifica el mensaje entre: pregunta frecuente del negocio, producto estándar, o "no puedo resolverlo" (`docs/arquitectura/user-flow.md` §3) | Probar un mensaje de cada tipo y verificar que activa la rama correcta |
| 3 | Si es una pregunta frecuente, responde con la información cargada en `faqs`, incluyendo la tarjeta del colocador o el enlace de Google cuando corresponde | Preguntar "¿tienen garantía?" y "¿dónde están ubicados?" y verificar que la respuesta coincide con lo cargado en `faqs` |
| 4 | Si preguntan por un descuento por forma de pago, responde que los descuentos son solo en mostrador (regla de atención #10 del PRD) | Preguntar "¿hacen descuento si pago en efectivo?" y verificar esa respuesta puntual |
| 5 | Si es un producto estándar, responde con el precio y el stock reales de `productos_estandar`, en lenguaje simple, sin exigir vocabulario técnico | Preguntar "se me voló una chapa, ¿tienen para reponer?" y verificar que la respuesta trae el precio/stock correcto del producto que mejor coincide |
| 6 | Si no encuentra el producto o la pregunta no coincide con ninguna FAQ cargada, responde que un vendedor va a confirmar — nunca inventa un precio, stock o dato | Preguntar por un producto inexistente en el catálogo de prueba y verificar que la respuesta es la de derivación, sin ningún dato inventado |
| 7 | Todas las respuestas son cortas, un solo tema por mensaje, y en tono institucional en plural (reglas de atención #5 y #6 del PRD, `docs/arquitectura/marca.md`) | Revisar las respuestas de las pruebas anteriores y confirmar que ninguna mezcla dos temas ni usa primera persona del singular |
| 8 | El agente nunca responde como asistente de conversación general — ante algo fuera del negocio (clima, noticias, charla suelta) no lo resuelve como si fuera su tema (regla **#7** de la constitución) | Preguntar algo fuera de tema (ej. "¿va a llover mañana?") y verificar que el agente no lo responde como una consulta general, sino que redirige al negocio o deriva |
| 9 | Antes de cada llamada a Claude, se chequea que no se superó el techo de gasto mensual configurado; si se superó, responde con el mensaje fijo de derivación en vez de llamar a la IA (regla **#3**) | Simular que el techo ya fue alcanzado y verificar que no se hace la llamada a Claude, y que igual se responde con la derivación a un vendedor |
| 10 | Cierra la conversación de forma cordial cuando no quedan más preguntas pendientes (regla de atención #14 del PRD) | Verificar que, tras una respuesta sin seguimiento del cliente, el agente agrega un cierre cordial coherente con `docs/arquitectura/marca.md` |

## 5. Qué queda afuera

- Pedidos a medida, cotización estimativa y derivación a un vendedor por un pedido concreto —
  todo eso es M2 (`docs/prd.md`).
- Interpretar o "leer" el contenido de fotos, videos o bosquejos — fuera de alcance del MVP
  (`docs/prd.md` §5).
- Identificar un único interlocutor cuando escriben varias personas por el mismo pedido —
  concepto de M2, no aplica a consultas de M1.

## 6. Reglas del proyecto que toca

- **#5** — terminada = las 3 verificaciones en verde (ver anexo).
- **#1** — lee el hilo completo de `conversaciones`, aislado por número de cliente.
- **#3** — chequea el techo de gasto de Claude antes de cada llamada.
- **#7** — nunca actúa como asistente de conversación general; siempre deriva lo que no puede
  resolver dentro del alcance del negocio.

---

## Anexo técnico (para los agentes)

- **Las 3 verificaciones, en orden (regla #5):** 1) tests unitarios + linter (clasificación de
  intención, lógica de fallback, chequeo del techo de gasto) → 2) integración (con la base de
  datos de M1-02 poblada con datos de ejemplo y el webhook de M1-01) → 3) el recorrido completo
  de punta a punta: una conversación real de WhatsApp con al menos un caso de cada rama (FAQ,
  producto estándar, fuera de tema/sin dato) contra el número de prueba de Meta. No se avanza a
  una capa con la anterior en rojo; si una falla: causa raíz → arreglo mínimo → re-correr.
- **Test primero:** el test se escribe antes de la implementación y se lo ve fallar por la razón
  correcta antes de darlo por bueno.
- **Entregables:** la función/handler que reemplaza la respuesta fija de M1-01 por la respuesta
  real (llamando a Claude según `docs/arquitectura/integraciones-ia.md`), la lógica de
  clasificación de intención, y el mecanismo del techo de gasto (contador + corte, según ese
  mismo documento).
- **Diseño y alcance no se deciden acá:** el modelo de IA y el techo de gasto ya están en
  `docs/arquitectura/integraciones-ia.md`; el tono y el flujo de conversación, en
  `docs/arquitectura/marca.md` y `docs/arquitectura/user-flow.md`; el si entra o no, en
  `docs/prd.md`.
