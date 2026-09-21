# M1-01 · Conexión con WhatsApp (recibir y enviar mensajes)

**Depende de:** nada

## 1. Qué queremos lograr

Un mensaje real mandado al número de WhatsApp del negocio (o al número de prueba de Meta,
mientras se verifica la cuenta real — ver `docs/arquitectura/stack.md` §Abierto) llega al
backend, y el backend responde por el mismo canal — aunque sea con un texto fijo por ahora. Es
la base de transporte sobre la que se conecta la inteligencia real en M1-03.

## 2. Qué hay hoy

Nada — el proyecto arranca de cero. Se construye sobre el stack ya decidido en
`docs/arquitectura/stack.md` (Next.js en Vercel).

## 3. Qué tiene que hacer (y cómo comprobamos cada cosa)

| # | Qué hace | Cómo se comprueba |
|---|---|---|
| 1 | Expone un endpoint de webhook (`GET`) que responde correctamente al challenge de verificación que pide Meta al dar de alta el webhook | Un test que simula el request de verificación de Meta recibe de vuelta el challenge esperado |
| 2 | Expone un endpoint de webhook (`POST`) que recibe la notificación de un mensaje nuevo de WhatsApp sin romperse | Mandar un payload de ejemplo (formato real de la API de WhatsApp) y verificar que el endpoint responde 200, sin error |
| 3 | Al recibir un mensaje, envía una respuesta fija de acuse de recibo al mismo número, usando la API de WhatsApp | Tras el mensaje de prueba, el número de origen recibe una respuesta (verificable contra la API de WhatsApp en un entorno de prueba, o con un mock de esa API en el test automatizado) |
| 4 | Si la llamada a la API de WhatsApp para enviar el mensaje falla (ej. token inválido, error de red), el sistema no se cae y deja un registro del error | Simular una falla de la API de envío y verificar que el proceso sigue funcionando y el error queda logueado |
| 5 | Las claves de acceso (token de WhatsApp, ID del número, token de verificación del webhook) salen de variables de entorno — ninguna está escrita en el código | Revisar el código fuente y confirmar que no hay ningún valor de clave hardcodeado (regla #2) |

## 5. Qué queda afuera

- Qué dice exactamente la respuesta y cómo se decide (eso es M1-03) — acá la respuesta es un
  texto fijo de acuse de recibo, no la lógica del agente.
- Guardar el mensaje en la base de datos o leer el hilo de la conversación (eso lo hace M1-03,
  apoyándose en las tablas de M1-02).
- Cualquier lógica de reintentos avanzada ante fallas de la API de WhatsApp — alcanza con no
  romperse y dejar el registro del error.

## 6. Reglas del proyecto que toca

- **#5** — terminada = las 3 verificaciones en verde (ver anexo).
- **#2** — ninguna clave de WhatsApp vive en el código; todas salen de variables de entorno
  (ver `docs/arquitectura/secretos.md`).

---

## Anexo técnico (para los agentes)

- **Las 3 verificaciones, en orden (regla #5):** 1) tests unitarios + linter → 2) integración
  (webhook GET/POST con payloads de ejemplo, incluida la simulación de falla de la API de envío)
  → 3) el recorrido completo de punta a punta: un mensaje de prueba real llega al número de
  WhatsApp de prueba y se recibe la respuesta fija. No se avanza a una capa con la anterior en
  rojo; si una falla: causa raíz → arreglo mínimo → re-correr.
- **Test primero:** el test se escribe antes de la implementación y se lo ve fallar por la razón
  correcta antes de darlo por bueno.
- **Entregables:** la ruta del webhook (ej. `app/api/whatsapp/webhook/route.ts` o equivalente
  según cómo esté armado el proyecto), la función reutilizable de envío de mensajes de WhatsApp
  que va a usar M1-03, y las variables de entorno documentadas en `.env.example` (sin valores
  reales).
- **Diseño y alcance no se deciden acá:** el cómo (stack, proveedor de WhatsApp) ya está en
  `docs/arquitectura/stack.md`; el si entra o no, en `docs/prd.md`.
