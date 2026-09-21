# Agente WhatsApp para Zinguería — PRD

> **Este documento manda sobre el *qué*: qué construimos, en qué orden, y qué significa
> «terminado».** El *cómo* técnico vive en `docs/arquitectura/` (un documento por tema) y no se decide acá.
>
> **Última revisión:** `2026-09-07`
> **Insumos:** [`docs/icp/`](icp/README.md) (ICP-01 particular, ICP-02 contratista, ICP-03 dueño
> de pyme) · [`docs/solucion.md`](solucion.md) — esta es la primera implementación real del
> producto ahí descripto.

---

## 1. Qué es Agente WhatsApp para Zinguería

**Agente WhatsApp para Zinguería es un asistente que atiende por WhatsApp a los clientes de una zinguería (venta de chapa y productos para techo, estándar y a medida, para la industria de la construcción).**

- **Para quién:** el dueño de la zinguería (ICP-03) — quien hoy hace de mostrador, compras y WhatsApp al mismo tiempo — y, del otro lado del chat, sus clientes: el particular con una urgencia doméstica (ICP-01) y el contratista con una obra en curso (ICP-02).
- **Qué promete:** ningún cliente que escribe por WhatsApp se queda sin respuesta inmediata, sin que nadie del local tenga que soltar lo que está haciendo para contestar. Para el dueño, dejar de sentir que "se le escapan" ventas por no llegar a todo (ICP-03).
- **Qué problema resuelve:** el dueño ES el sistema de atención — mientras más movimiento tiene el local, más se posterga el WhatsApp, y ahí se pierden ventas que ni siquiera llega a ver. Ese cliente sin respuesta se va a otro corralón o zinguería que sí le contesta (ICP-01, ICP-02).

## 2. El MVP — un recorrido completo, no una lista de features

```
Un cliente escribe por WhatsApp preguntando por un producto o algo del negocio
  → si es un producto estándar (precio/stock fijo, típico de ICP-01) o una pregunta frecuente sobre el negocio (medios de pago, envíos, garantías, redes sociales), el agente responde al instante
  → si es un pedido a medida (típico de ICP-02, contratista con obra en curso), el agente responde primero con una cotización estimativa (chapa galvanizada y el calibre que se considere adecuado si no se especificó el material) en vez de abrir con preguntas
  → si el cliente quiere avanzar, el agente junta las medidas exactas y los datos de contacto necesarios
  → el pedido a medida llega a un vendedor humano, que confirma el precio final y cierra la venta
  → el dueño (ICP-03) ve que el WhatsApp del negocio ya no depende de que él lo atienda personalmente
```

Si eso corre de punta a punta **una vez** — un cliente real recibe respuesta automática y, si pidió algo a medida, un vendedor recibe ese pedido listo para cotizar — el MVP está probado.

## 3. Milestones (en orden)

### Reglas de atención transversales (aplican a M1 y M2)

Son las reglas reales con las que hoy atiende el equipo de venta de la zinguería — el agente
tiene que comportarse igual, no inventar un estilo propio. Cruzan ambos milestones, por eso van
acá y no adentro de uno solo.

1. **Lee toda la conversación antes de responder**, y la relee si algo no le queda claro — nunca contesta a partir de un solo mensaje suelto sin ver el hilo completo.
2. **Todo cambio o modificación de un pedido queda registrado por escrito** en la conversación — nada se acuerda "de palabra" sin que quede el mensaje.
3. **Responde con una solución antes de preguntar.** Ante una consulta general de un pedido a medida, da primero una cotización **estimativa** (regla 4) y recién si el cliente quiere avanzar pide los datos exactos — nunca abre con una lista de preguntas.
4. **Si no se especifica el material, cotiza en chapa galvanizada con el calibre que se considere adecuado** — son los valores por defecto para poder dar la estimación de la regla 3, siempre aclarando que es un valor aproximado sujeto a confirmación de un vendedor.
5. **Responde un solo tema por mensaje** — no acumula varias respuestas distintas en un mismo mensaje largo (coincide con la buena práctica de Meta ya anotada en M1: mensajes cortos).
6. **Atención institucional, en plural ("nosotros")** — nunca habla en primera persona del singular ni se presenta como un empleado individual. *(Insumo directo para `marca.md` y `user-flow.md`, que arma el Diseñador en `/arquitectura`.)*
7. **Reutiliza las respuestas automáticas que ya existen en el WhatsApp Business del local**, en vez de redactar de cero cuando una de esas respuestas ya aplica. *(Detalle técnico para `/arquitectura`.)*
8. **Si varias personas escriben por el mismo pedido u oportunidad, identifica y fija un único interlocutor** para seguir esa conversación — no le sigue respondiendo a los demás sobre el mismo pedido.
9. **Agradece siempre que el cliente comparte una foto, video, dibujo o bosquejo** — el agente acepta y agradece el archivo, lo suma a los datos del pedido a medida (M2) y lo deriva al vendedor junto con el resto; no necesita "entender" la imagen, solo recibirla y agradecerla.
10. **Ante un pedido de descuento por forma de pago, informa que los descuentos son solo para compras en mostrador** — se suma como respuesta fija dentro de las preguntas frecuentes de M1.
11. **Comparte el contacto del colocador (instalador) mediante tarjeta de contacto** cuando corresponde — se suma como respuesta de M1, igual que compartir el horario o la ubicación.
12. **Comparte el enlace oficial de Google del local** cuando corresponde — se suma a la respuesta de ubicación ya prevista en M1.
13. **Identifica y registra el tipo de cliente** (Consumidor Final, Gremio, Mayorista, Profesional o Empresa Constructora) al tomar un pedido a medida (M2) — la clasificación final la confirma el vendedor humano al cerrar la venta, no el agente.
14. **Cierra siempre la conversación de forma cordial** — nunca deja un intercambio "cortado" sin un cierre.

**Esto ajusta dos decisiones anteriores de este PRD** (por las reglas 3 y 4):
- El **Riesgo #1** deja de ser "cero tolerancia: el agente nunca pone precio a lo que es a medida" — ahora el agente puede dar una **estimación** con material/calibre por defecto, siempre aclarando que es aproximada. Cero tolerancia se mantiene sobre presentarla como si fuera un precio firme: el precio final y su confirmación siempre los da un vendedor humano.
- **M2** ya no es solo "el agente junta los datos y deriva" — ahora primero responde con una estimación, y recién pide medidas exactas si el cliente quiere avanzar.

### M1 — El agente responde consultas de productos estándar y preguntas frecuentes del negocio

**Objetivo:** que ningún mensaje sobre precio, stock, horario, ubicación o cualquier otra pregunta frecuente del negocio (medios de pago, envíos, garantías, redes sociales, CUIT) se quede sin responder.
**Por qué primero:** es el caso de mayor volumen diario y el más simple — sin esto no hay nada que mostrar, y valida el canal de WhatsApp antes de sumarle complejidad.
**Perfil que lo valida:** ICP-01 (el particular con una urgencia puntual).

**De dónde sale la lista de preguntas frecuentes:** se arma con un research puntual (una sola
vez, no un proceso corriendo en producción) sobre el historial real de WhatsApp de los últimos
6 meses del local, para detectar qué preguntas se repiten de verdad — no se inventan a ojo.
**Regla de privacidad (constitución #1):** de ese research solo queda guardada la lista de
preguntas y respuestas genéricas; el historial crudo (nombres, teléfonos, conversaciones
privadas de clientes) no se carga ni queda conectado al agente.
**Buena práctica de Meta a seguir:** las respuestas son cortas y directas (WhatsApp es chat, no
un blog) y usan botones o listas de WhatsApp en vez de texto libre cuando ayudan a guiar al
cliente — esto lo define en detalle `/arquitectura` al diseñar el recorrido del usuario.

**Está terminado cuando:**
- [ ] Un cliente real escribe algo como *"se me voló una chapa, ¿tienen para reponer?"* o *"¿cuánto sale la chapa acanalada?"* y el agente responde precio y stock correctamente, en lenguaje simple (sin exigirle términos técnicos — ver ICP-01 §7).
- [ ] Un cliente pregunta el horario o la dirección del local y el agente responde correctamente.
- [ ] Un cliente pregunta algo frecuente del negocio que no es de un producto puntual (ej. *"¿aceptan tarjeta?"*, *"¿hacen envíos?"*, *"¿tienen garantía?"*, *"¿tienen descuento por pago en efectivo?"*, *"¿me pasan a alguien que instale?"*, *"¿dónde están ubicados?"*) y el agente responde con la información pública real del negocio — incluyendo la política de descuentos (solo en mostrador), la tarjeta de contacto del colocador, y el enlace oficial de Google del local cuando corresponde.
- [ ] Si el agente no tiene la respuesta, avisa que un vendedor va a contestar — nunca inventa un dato.

### M2 — El agente cotiza a medida en forma estimativa y deriva a un vendedor para confirmar

**Objetivo:** capturar el caso de mayor valor (los pedidos a medida) respondiendo primero con una solución, no con un interrogatorio.
**Por qué segundo:** depende de que el canal de M1 ya esté andando; sumar acá la lógica de "a medida" sin tocar lo que ya funciona.
**Perfil que lo valida:** ICP-02 (el contratista con una obra en curso).

**Está terminado cuando:**
- [ ] Un cliente escribe algo como *"necesito chapa a medida para una obra, ¿me cotizan?"* y el agente responde primero con una **cotización estimativa** — en chapa galvanizada y el calibre que se considere adecuado si no especificó el material — aclarando siempre que es un valor aproximado a confirmar (reglas de atención #3 y #4).
- [ ] Si el cliente quiere avanzar, el agente pide las medidas exactas, los datos de contacto y el tipo de cliente (Consumidor Final, Gremio, Mayorista, Profesional o Empresa Constructora), sin exigir un formato técnico (ver ICP-02 §7).
- [ ] Si el cliente manda una foto, video o bosquejo con las medidas, el agente lo agradece y lo suma al pedido, sin necesidad de "leer" la imagen (regla de atención #9).
- [ ] El pedido completo (estimación dada + medidas + datos de contacto + tipo de cliente) le llega a un vendedor humano de forma clara, listo para confirmar el precio final y cerrar.
- [ ] El cliente recibe confirmación de que su pedido fue recibido y en cuánto tiempo un vendedor lo va a contactar para confirmar el precio — la espera es la principal objeción de este perfil (ICP-02 §6).
- [ ] Si varias personas escriben por el mismo pedido, el agente identifica y sigue la conversación con un único interlocutor (regla de atención #8).

## 4. Riesgos, con su límite aceptado

| # | Riesgo | Hasta dónde se tolera / qué se hace al cruzarlo |
|---|---|---|
| 1 | El agente presenta su estimación de a medida como si fuera un precio firme, o la calcula muy alejada de la realidad | Cero tolerancia sobre presentarla como precio firme: siempre debe aclarar que es aproximada y que un vendedor la confirma. La estimación en sí se ajusta con feedback real; si un vendedor detecta que se aleja mucho del precio final, se revisan los supuestos de material/calibre por defecto (regla de atención #4). |
| 2 | La lista de precios/stock de productos estándar que usa el agente queda desactualizada | Se tolera hasta 1 semana de desfasaje; pasado ese punto se pausa el agente hasta actualizar la lista. |
| 3 | Entran más pedidos a medida de los que un vendedor puede cotizar a mano | Se tolera mientras el volumen sea manejable (referencia: hasta ~20 pedidos a medida por día); si se supera, se evalúa sumar más gente o algún tipo de precotización. |
| 4 | El dueño no confía en que el agente no va a "quedar mal" frente a sus clientes (barrera de ICP-03 §7, sin validar) | Se tolera hasta el cierre de M1: si al mostrarle el agente funcionando el dueño sigue sin confiar, se revisa el tono/comportamiento antes de avanzar a M2. |
| 5 | El particular (ICP-01) no percibe diferencia real de velocidad — el WhatsApp del local ya respondía "bastante rápido" antes del agente | Se tolera si de todos modos hay al menos una venta rescatada demostrable en el "probalo vos" de M1; si no hay ninguna, se revisa si el problema es tan urgente como se asumió (ICP-01, supuesto 🔴). |
| 6 | El research sobre el historial de WhatsApp no detecta bien las preguntas frecuentes reales, o la lista de FAQs queda desactualizada con el tiempo | Se tolera una revisión inicial imperfecta si se corrige con el feedback de las primeras conversaciones reales; la lista de FAQs se revisa junto con la lista de precios (riesgo #2), no por separado. |
| 7 | El agente se sale del uso "de negocio" que exige la política de Meta para WhatsApp Business (vigente desde enero 2026, prohíbe chatbots de propósito general) y arriesga que Meta restrinja o banee el número | Cero tolerancia: el agente solo responde sobre el negocio (productos, pedidos, preguntas frecuentes del local) y siempre ofrece una salida clara a un vendedor cuando no puede resolver algo — nunca actúa como asistente de conversación abierta. Se valida explícitamente en `/arquitectura` y antes de cada "probalo vos". |

## 5. Fuera de alcance

- Pagos online — el agente no cobra ni procesa pagos.
- Dar el precio **final** de un pedido a medida como si fuera definitivo — el agente puede dar una cotización **estimativa** (reglas de atención #3 y #4), pero el precio final y su confirmación siempre los da un vendedor humano.
- Que el agente "entienda" o interprete el contenido de fotos, videos o bosquejos que manda el cliente — los recibe, agradece y deriva tal cual al vendedor (regla de atención #9), sin procesar la imagen.
- Que el agente clasifique definitivamente el tipo de cliente — solo pregunta y registra el dato (regla de atención #13); la clasificación final la confirma el vendedor al cerrar la venta.
- Integración con el sistema de stock/precios que ya usa la zinguería — por ahora la lista de productos estándar se carga aparte, sin conectar sistemas.
- Panel de administración para el dueño — en esta primera versión, el agente se configura directamente con el equipo que lo construye, no desde una pantalla propia.
- Generalizar el producto a otros rubros de PyME con local a la calle — `docs/solucion.md` lo describe como producto general, pero este MVP construye y valida un solo caso real (la zinguería) antes de intentar reutilizarlo en otro rubro.
- Conectar el historial completo de WhatsApp como fuente de conocimiento en vivo — el agente nunca consulta el historial crudo; solo usa la lista de FAQs ya curada (ver M1), respetando la regla #1 de la constitución (datos de usuarios).
- Preguntas frecuentes de temas externos al negocio (clima, cotizaciones, noticias) o conversación abierta tipo asistente general — el alcance es estrictamente el negocio (productos, pedidos, preguntas del local). Esto no es solo una elección de producto: **desde enero de 2026, la política de Meta para WhatsApp Business prohíbe los chatbots de "propósito general"** y solo permite automatización de negocio (soporte, FAQs, pedidos, seguimiento) con salida clara a un humano — quedarse fuera de eso arriesga que Meta restrinja el número (ver riesgo #7).

## 6. Preguntas abiertas

1. **¿Qué número de WhatsApp va a usar el agente — uno nuevo o el que ya usa hoy el local?** — *cerrar antes de M1.*
2. **¿De dónde sale la lista de precios y stock de productos estándar que el agente va a consultar?** (por ejemplo, si ya existe una lista de precios actualizada que se pueda reutilizar) — *cerrar antes de M1.*
3. **¿Quién recibe los pedidos a medida — el dueño, un vendedor puntual, o varios?** — *cerrar antes de M2.*
4. **¿El dueño real de la zinguería acepta pagar ~USD 30-50/mes por esto una vez que funcione?** (`docs/solucion.md` §9 lo deja como precio ancla sin validar) — *cerrar antes de ofrecerlo comercialmente, no bloquea construir el MVP.*
5. **¿Cómo se consigue el export del historial de WhatsApp de los últimos 6 meses, y quién lo hace?** (necesita que el dueño lo exporte o dé acceso, y que el research se haga antes de cargar el agente) — *cerrar antes de M1.*
6. **¿Cómo le avisa el vendedor al cliente que su pedido a medida ya está cotizado, si pasaron más de 24hs desde su último mensaje?** Meta exige mensajes de plantilla aprobados fuera de esa ventana — *cerrar antes de M2, lo resuelve `/arquitectura`.*
7. **¿Qué calibre y qué otros valores "por defecto" usamos para la estimación cuando el cliente no especifica el material?** (regla de atención #4) — necesita que el dueño o un vendedor lo defina por tipo de producto — *cerrar antes de M2.*
8. **¿Tenemos ya el enlace de Google del local y el contacto/tarjeta del colocador para cargarlos?** (reglas de atención #11 y #12) — *cerrar antes de M1.*

## 7. El cómo técnico (lo completa `/arquitectura`)

- **Stack, deploy y costo:** [`docs/arquitectura/stack.md`](arquitectura/stack.md) — Next.js +
  Vercel, Supabase, WhatsApp Cloud API directa (Meta), Claude (Anthropic).
- **Modelo de datos:** [`docs/arquitectura/modelo-de-datos.md`](arquitectura/modelo-de-datos.md)
- **Secretos:** [`docs/arquitectura/secretos.md`](arquitectura/secretos.md)
- **Recorrido del usuario:** [`docs/arquitectura/user-flow.md`](arquitectura/user-flow.md)
- **Marca:** [`docs/arquitectura/marca.md`](arquitectura/marca.md)
- **Integraciones IA:** [`docs/arquitectura/integraciones-ia.md`](arquitectura/integraciones-ia.md)
- **Integraciones con otros sistemas:** [`docs/arquitectura/integraciones.md`](arquitectura/integraciones.md)
- **Auth y permisos:** no aplica — este MVP no tiene ningún login de usuario, se configura
  directamente por el equipo, sin panel (ver §5).

## 8. La regla de gestión

> **El alcance se achica quitando features (que quedan visibles en §5), nunca bajando la vara de
> «terminado».** Un milestone con condiciones recortadas parece cerrado y no lo está: la deuda
> queda escondida en un checkbox tildado.

Si algo se posterga, se anota con milestone destino y dueño, y el porqué va a
[`DECISIONS.md`](../DECISIONS.md). Lo que toque la regla #1 de la
[constitución](sdd/constitucion.md) (datos de usuarios) no se posterga nunca.
