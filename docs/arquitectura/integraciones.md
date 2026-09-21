# Integraciones con otros sistemas

> **Dueño de:** qué sistemas externos toca el proyecto y cómo se cumple la regla **#4** — toda
> acción con efecto hacia afuera pasa por aprobación humana explícita.

## Por qué existe este documento en un proyecto sin pagos ni mails

Este proyecto no tiene pasarela de pago, ni envío de mails, ni ninguna API de terceros más allá
de WhatsApp (documentado como el canal principal en `stack.md`) y la IA (`integraciones-ia.md`).
Aun así, la regla #4 es central acá: el diseño completo de M2 existe justamente para que el
agente **nunca cierre una venta ni confirme un precio final por su cuenta**. Este documento es
el que deja explícito cómo se cumple esa regla en la práctica.

## Las acciones con efecto hacia afuera de este proyecto

| Acción | ¿Requiere aprobación humana previa? | Por qué |
|---|---|---|
| Responder una pregunta frecuente o el precio/stock de un producto estándar (M1) | No | Es información pública y fija (catálogo, horario) — no es una promesa nueva, solo se informa lo que ya está cargado. |
| Dar una **cotización estimativa** de un pedido a medida (M2) | No, pero siempre debe aclarar que es aproximada | Está diseñada explícitamente para no ser un compromiso firme (PRD, riesgo #1) — es información orientativa, no una decisión que comprometa al negocio. |
| **Confirmar el precio final y cerrar una venta a medida** | **Sí, siempre** — lo hace exclusivamente un vendedor humano | Esta es la acción real "hacia afuera" que compromete al negocio (cobra, entrega, factura). Cumple la regla #4 de punta a punta: el agente nunca la ejecuta. |
| Notificar a un vendedor sobre un pedido nuevo | No | Es una acción interna del negocio hacia su propio equipo, no hacia un tercero externo. |

## Cómo se notifica al vendedor

El pedido armado en `pedidos_a_medida` (ver `modelo-de-datos.md`) se le envía al vendedor por
**el mismo canal de WhatsApp** — no se suma un sistema externo nuevo solo para esto. El número o
los números que reciben esa notificación se configuran vía la variable de entorno
`VENDEDOR_WHATSAPP_NUMBER` (ver `secretos.md`).

**La ventana de 24 horas de Meta:** si el vendedor necesita volver a escribirle al cliente
después de más de 24hs desde el último mensaje del cliente (por ejemplo, para avisarle que ya
tiene el precio final), Meta exige usar un **mensaje de plantilla previamente aprobado** — no se
puede mandar un mensaje libre fuera de esa ventana (PRD, pregunta abierta #6). Esto se resuelve
dando de alta al menos una plantilla de "tu cotización ya está lista" en el panel de Meta antes
de cerrar M2.

## Abierto

- Quién exactamente recibe las notificaciones (el dueño, un vendedor puntual, o varios números)
  — PRD, pregunta abierta #3.
- Texto y aprobación de la plantilla de mensaje para avisar fuera de la ventana de 24hs.
