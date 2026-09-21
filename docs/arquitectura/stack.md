# Stack, deploy y costo

> **Dueño de:** con qué se construye el agente, dónde corre, y el costo mensual estimado.
> **No cubre:** qué datos existen (→ [`modelo-de-datos.md`](modelo-de-datos.md)) · las claves
> (→ [`secretos.md`](secretos.md)) · el modelo de IA y su techo de gasto
> (→ [`integraciones-ia.md`](integraciones-ia.md)).

## Qué es esto, en criollo

No hay ninguna "app" con pantallas en este MVP — el producto entero vive dentro de la
conversación de WhatsApp. Lo que se construye es un servicio que:
1. recibe cada mensaje que le escriben al WhatsApp de la zinguería,
2. lo procesa con IA siguiendo las reglas del PRD,
3. responde por el mismo WhatsApp (o avisa a un vendedor cuando corresponde).

No hay login, ni panel, ni pantallas — eso está explícitamente fuera de alcance del MVP (ver
`docs/prd.md` §5).

## Con qué se construye

| Pieza | Elección | Por qué |
|---|---|---|
| Backend | Next.js (API routes), TypeScript | Recibe los mensajes de WhatsApp (webhook) y corre la lógica del agente. Se eligió por ser simple de desplegar y dejar la puerta abierta a sumar pantallas más adelante (ej. un futuro panel de administración) sin cambiar de framework. |
| Hosting | Vercel | Cero mantenimiento de servidores, capa gratuita generosa para el volumen de esta zinguería, integra directo con Next.js. |
| Base de datos | Supabase (Postgres administrado) | Guarda el catálogo, las FAQs y los pedidos a medida (ver `modelo-de-datos.md`). Capa gratuita suficiente para un solo negocio; administrado, sin que nadie tenga que mantener un servidor de base de datos. |
| Canal de mensajería | WhatsApp Business Platform — **API oficial de Meta (Cloud API), conexión directa** | Es la opción más barata: no suma un proveedor intermediario (BSP) que cobra un costo extra por mensaje además de lo que ya cobra Meta. La complejidad extra de conectar directo (verificar la cuenta de negocio en Meta) es un trámite único, no recurrente. |
| IA / modelo de lenguaje | Claude (Anthropic) | Mismo proveedor con el que se está construyendo este proyecto — buena calidad conversacional en español y buen soporte para limitar el gasto por uso (ver `integraciones-ia.md`, regla #3). |

## Dónde corre y se publica

- El código vive en este mismo repositorio de GitHub.
- Vercel despliega automáticamente cada vez que se sube un cambio a la rama principal
  (`main`) — no hay un paso manual de "subir el servidor".
- El webhook de WhatsApp (Meta) apunta a una URL de este proyecto en Vercel; Meta le manda ahí
  cada mensaje nuevo.

## Costo mensual estimado (primer esbozo — `/costo` lo afina)

Con el volumen esperado de una sola zinguería (unas pocas decenas de conversaciones por día),
las piezas de infraestructura entran cómodas en las capas gratuitas de sus proveedores:

- **Vercel:** gratis en este volumen.
- **Supabase:** gratis en este volumen.
- **WhatsApp Cloud API (Meta):** las conversaciones que inicia el cliente (que son la mayoría de
  los casos de este producto) hoy no tienen costo directo por mensaje bajo el modelo vigente de
  Meta; solo se paga si el negocio inicia la conversación fuera de la ventana de 24hs con una
  plantilla aprobada (ver pregunta abierta #6 del PRD).
- **Claude (Anthropic):** se cobra por uso (tokens) — es la partida más variable, y depende
  directamente del volumen real de mensajes.

**El número final de "cuánto sale por mes" lo calcula `/costo`** en
`docs/arquitectura/costos.md`, una vez que se defina el volumen de mensajes esperado — este doc
solo dice con qué se arma esa cuenta.

## Abierto

- El dueño de la zinguería tiene que completar la verificación de su Cuenta de Negocio en Meta
  (Meta Business Manager) para poder usar la API oficial de WhatsApp — es un trámite que solo
  puede hacer el dueño (con sus datos reales de negocio), no algo que se resuelva en código.
  Mientras eso se tramita, se puede desarrollar y probar con un número de prueba de Meta.
