# ICP-03 — El dueño de zinguería/corralón que decide adoptar el agente

**Estado: Hipótesis — a validar · Última actualización: 2026-09-07**

> Este ICP describe a **quien compra/adopta** el producto (el agente de WhatsApp), no a
> quien le escribe al local — eso ya está cubierto por [`icp-01-particular.md`](icp-01-particular.md)
> y [`icp-02-contratista.md`](icp-02-contratista.md).

## 1. Snapshot

| Campo | Valor |
|---|---|
| Código | ICP-03 |
| Perfil en una línea | El dueño que hace de todo en su zinguería — mostrador, compras, gestión — y el WhatsApp es lo primero que se le acumula sin contestar |
| Estado | Hipótesis — a validar |
| Rol / tipo | Dueño o encargado general de una zinguería/corralón de materiales de construcción |
| Datos descriptivos | Negocio chico/mediano, pocos empleados; el dueño pasa la mayor parte del día en el mostrador o el depósito |
| Contexto comercial | Es quien decide y paga la adopción de herramientas para el negocio; presupuesto acotado, sensible al costo mensual |
| Conciencia (Lente A) | Problem-aware — Estimado: siente que pierde ventas por no contestar rápido, no necesariamente sabe que existen agentes de WhatsApp con IA como solución |
| Corresponde a | La decisión de adoptar el producto descrito en `docs/solucion.md` |

## 2. Perfil y contexto

Es el dueño (o encargado general) de un local de zinguería: atiende el mostrador, coordina
compras y logística, y muchas veces también responde el WhatsApp del negocio — todo al mismo
tiempo. No tiene un equipo de atención al cliente dedicado; el negocio corre con pocas personas
que hacen de todo. *(Hipótesis — a validar.)*

**La trampa que lo define:** el dueño ES el sistema de atención — mientras más crece el
negocio, más mensajes se le acumulan sin contestar, y no tiene a quién delegárselo sin gastar en
una persona más. *(Hipótesis — a validar.)*

## 3. Jobs To Be Done (JTBD)

| Tipo | JTBD |
|---|---|
| Funcional | Cuando un cliente me escribe por WhatsApp y estoy ocupado atendiendo el local, quiero que igual reciba una respuesta rápida y útil, para no perder la venta. |
| Emocional | Quiero dejar de sentir que "se me escapan" ventas por no llegar a todo, y la culpa de saber que hay mensajes sin contestar. |
| Social | Quiero que mi negocio se vea profesional y responda rápido, no como "el típico local que tarda una eternidad en contestar". |

**Priorizado:** el funcional — no perder la venta por no responder a tiempo. Es lo que
justifica pagar por una solución. *(Hipótesis — a validar.)*

## 4. Problema núcleo

El dueño es, a la vez, quien vende en el mostrador y quien debería contestar el WhatsApp —
cuando el negocio tiene más movimiento, el WhatsApp es lo primero que se posterga, y ahí se
pierden ventas que nunca llega a ver.

**Error de razonamiento que lo perpetúa:** cree que la solución es "contestar más rápido yo
mismo" en vez de delegar la primera respuesta a algo o alguien que le libere el tiempo.
*(Hipótesis — a validar.)*

## 5. Dolores

- **Económicos:** pierde ventas concretas por mensajes sin responder a tiempo (ver ICP-01/02);
  no puede costear un empleado extra solo para atender WhatsApp.
- **Emocionales:** culpa y frustración de saber que hay mensajes pendientes, sensación de estar
  siempre "atrás" de la demanda.
- **Sociales / de identidad:** le preocupa la reputación del local si se corre la voz de que "no
  contestan".

*(Todos — Hipótesis — a validar; no hay citas textuales todavía.)*

## 6. Situación disparadora

| Disparador | Fuente |
|---|---|
| Un cliente le reclama o se queja de que tardó en responder | Hipótesis |
| Se da cuenta de que perdió una venta concreta que se fue a la competencia por no contestar a tiempo | Hipótesis |
| Un pico de demanda (temporada de lluvias/obra) lo desborda y decide que necesita ayuda | Hipótesis |

## 7. Barreras y creencias

| Creencia errónea | Verdad que ofrecemos | Microacción |
|---|---|---|
| Un agente de WhatsApp con IA es caro o complicado de mantener | Existen soluciones simples y accesibles para un negocio de su tamaño, sin necesitar un equipo técnico propio | Mostrarle el costo mensual real y que no necesita saber nada técnico para usarlo |
| Un bot va a responder mal y hacer quedar mal al negocio | El agente deriva a un humano en cuanto no sabe algo — nunca inventa una respuesta | Mostrarle un ejemplo real de una conversación donde el agente deriva correctamente |
| Perder algunas ventas por WhatsApp es "normal" en su rubro | Sus competidores que responden rápido se están quedando con esas ventas ahora mismo | Mostrarle cuántos mensajes reciben hoy sin responder a tiempo, si se puede medir |

## 8. Lenguaje que resuena

- **Frases supuestas** *(lenguaje supuesto — a validar)*: "no doy abasto", "se me acumulan los
  mensajes", "no tengo quien me atienda el WhatsApp", "capaz que estoy perdiendo ventas y ni me
  entero"
- **Conceptos que conectan:** "no perder ni una venta", "que alguien te cubra el WhatsApp"
- **Registro / tono:** directo, práctico, orientado a resultado de negocio, sin jerga de
  tecnología
- **Nunca usar con este ICP:** "IA generativa", "LLM", "automatización de flujos" — lo aleja;
  hablarle en resultado ("no perdés más ventas"), no en tecnología

## Módulo Ventas

**V1 — Quién decide:** el dueño mismo, sin comité de compra — decisión rápida si ve el valor y
el precio es accesible.

**V2 — Señales de fit (avanzar):**
- Negocio con local a la calle y atención presencial.
- WhatsApp activo con volumen de consultas diario.
- El dueño (o pocos empleados) atiende todo a la vez, sin rol dedicado a atención al cliente.
- Menciona explícitamente "no doy abasto" o "se me pierden mensajes".

**V3 — Señales de no-fit (frenar):**
- Ya tiene un equipo dedicado de atención al cliente.
- Volumen de WhatsApp muy bajo (pocas consultas por semana) — el problema no es urgente.
- Negocio 100% online sin local físico — cambia el caso de uso que resuelve el producto.

**V4 — Prueba económica / ROI:** si el costo del agente es menor a lo que representa una sola
venta perdida por semana, el retorno es evidente. Hay que cuantificar con el dueño cuántas
ventas cree que pierde hoy por no responder a tiempo. *(Hipótesis — el costo real del producto
lo define `/costo` más adelante.)*

**V5 — Pitch de referencia:** "Vos atendés el local y no das abasto con el WhatsApp. Este
agente contesta al instante lo que ya sabés que te preguntan siempre, y te avisa apenas hay algo
que necesita que decidas vos. No perdés más ventas por no llegar a responder, y vos seguís
enfocado en el mostrador."

## Supuestos abiertos

1. 🔴 **Que el dueño realmente identifica "no contestar a tiempo" como un problema urgente** —
   podría considerarlo un costo aceptado del negocio y no algo que valga la pena resolver ya.
2. 🔴 **Que el precio de un agente así se percibe como accesible** para el presupuesto de una
   pyme de este tamaño — sin dato real de costo todavía; se define en `docs/arquitectura/costos.md`.
3. 🔴 **Que el dueño confía en que un agente automático no va a "quedar mal"** frente a sus
   clientes — es la barrera de confianza en IA más fuerte a testear primero.
