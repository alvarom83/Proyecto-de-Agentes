> Basado en: `docs/icp/icp-03-dueno-pyme.md` (comprador) + `docs/icp/icp-01-particular.md` y
> `docs/icp/icp-02-contratista.md` (usuarios finales del agente) | Tipo: producto / agentes |
> Supuestos abiertos: 3 | Fecha: 2026-09-07

# Agente WhatsApp para PyMEs con local a la calle

## 1. Qué es

Un agente conversacional que se conecta al WhatsApp de una PyME con local a la calle y
responde automáticamente a los clientes que escriben, sin que nadie del negocio tenga que
soltar lo que está haciendo para contestar. Resuelve el problema de negocios chicos que pierden
ventas porque, mientras atienden gente en el mostrador, no llegan a responder los mensajes de
WhatsApp a tiempo.

## 2. La promesa / resultado

**De** un dueño (o su único empleado) tratando de cubrir el mostrador y el WhatsApp al mismo
tiempo, perdiendo ventas por responder tarde — **a** que cada mensaje recibe respuesta
inmediata: automática para lo simple (precio, stock, horario), derivada a una persona con todos
los datos ya juntados para lo que necesita criterio humano (un pedido a medida). El cambio se
siente en días desde que se conecta el número, no en meses de implementación.

## 3. Para quién es (y para quién no)

**Es para:**
- PyMEs con local físico a la calle, atención presencial, y WhatsApp activo como canal de venta.
- Negocios donde el dueño o unos pocos empleados atienden todo a la vez, sin un rol dedicado a
  atención al cliente.
- Rubros con productos "de catálogo" (precio/stock fijo) y, opcionalmente, productos a medida o
  personalizados que hoy se cotizan a mano — el caso de la zinguería es el primer ejemplo real.

**No es para:**
- Negocios 100% online, sin local físico — el caso de uso está pensado para liberar al dueño
  mientras atiende en persona, no para reemplazar un canal de venta online.
- Negocios que ya tienen un equipo dedicado de atención al cliente respondiendo rápido — el
  problema que resuelve el producto ya está resuelto de otra forma.
- Quien necesita que el agente cobre o cierre ventas de punta a punta ya en esta versión — eso
  queda fuera del MVP (ver `docs/prd.md` §5).

## 4. Qué hace / capacidades

- **Responde consultas frecuentes al instante:** precio, stock, horario y ubicación de
  productos estándar — sin esperar a que alguien del local esté libre.
- **Detecta pedidos a medida o fuera de catálogo** y junta los datos necesarios (medidas, datos
  de contacto) para que un vendedor los cotice — el agente nunca inventa un precio que no sabe.
- **Deriva a un humano en cuanto no tiene la respuesta** — nunca improvisa un dato incierto.
- **Confirma al cliente que su consulta o pedido fue recibido**, para que no sienta que "quedó
  en el aire" mientras espera.

Lo que automatiza: la primera respuesta y la toma de datos. Lo que queda en manos humanas: la
cotización de lo hecho a medida, el cierre de la venta y el cobro.

## 5. Cómo funciona

1. **Alta:** el negocio conecta su número de WhatsApp (nuevo o el que ya usa) y, junto con el
   equipo que lo implementa, carga su catálogo de productos estándar, horarios y datos de
   contacto — en este MVP no es self-serve, lo hace el equipo con el dueño.
2. **Uso:** cada cliente que escribe recibe respuesta automática o es derivado según el tipo de
   consulta.
3. **Resultado:** el dueño ve reducido el volumen de mensajes que tiene que atender
   personalmente, y los vendedores reciben los pedidos a medida ya armados y listos para cotizar.

Por ahora el agente no se conecta a ningún sistema de stock o precios que ya use el negocio
(ver `docs/prd.md` §5) — la lista de productos estándar se carga aparte.

## 6. Qué obtenés / entregables

- El número de WhatsApp del negocio funcionando con el agente activo.
- La configuración inicial (catálogo de productos estándar, horarios, datos de contacto)
  cargada por el equipo que lo implementa.
- **Sin panel de administración propio en esta versión** — un cambio de precio o de horario se
  pide directamente al equipo, no se autogestiona.

## 7. Formato y modelo de entrega

- **Modelo:** producto con **setup asistido**, no self-serve, en este MVP — el equipo conecta
  el WhatsApp y carga la configuración inicial por cada cliente.
- **Soporte:** el equipo que lo implementa resuelve incidentes y actualizaciones de catálogo
  mientras no exista panel propio.
- **SLA:** informal en el MVP — se formaliza más adelante si el producto escala a más clientes.

## 8. Requisitos y costo de herramientas

- Un número de WhatsApp (nuevo o el que ya usa el negocio) habilitado para conectarse al
  agente — esto tiene un costo de mensajería que depende del proveedor elegido.
- Que alguien del negocio pueda recibir las derivaciones a humano (mismo WhatsApp del local).
- El costo real de infraestructura + IA + mensajería todavía no está definido — lo estima
  `/costo` en `docs/arquitectura/costos.md`, una vez que `/arquitectura` defina el stack.

## 9. Precio base

- **Modelo de cobro:** suscripción mensual.
- **Precio ancla:** entre USD 30 y 50 por mes — el número exacto se ajusta una vez que
  `/costo` estime el costo real de correr el agente y se defina el margen deseado.
- Sin tiers, descuentos, trials ni promociones acá — eso es territorio de la oferta.

## Supuestos abiertos

1. 🔴 **Que USD 30-50/mes cubre los costos reales (infra + IA + mensajería) con margen** — se
   valida recién cuando `/costo` estime el costo real de correr el agente.
2. 🔴 **Que un dueño de pyme como ICP-03 acepta pagar una suscripción mensual** sin haber visto
   resultados todavía — el mecanismo para bajar ese riesgo (trial, garantía) es de la oferta,
   no de este documento.
3. 🔴 **Que el producto generaliza bien a otros rubros de "PyME con local a la calle"** más allá
   de la zinguería, sin cambios grandes de producto — todavía no validado con ningún otro rubro.
