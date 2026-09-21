# M1 — El agente responde consultas de productos estándar y preguntas frecuentes del negocio — plan

## 1. Qué logramos con este milestone

Que ningún mensaje de WhatsApp sobre precio, stock, horario, ubicación o cualquier otra pregunta
frecuente del negocio se quede sin responder — el agente contesta solo, al instante, sin que
nadie del local tenga que soltar lo que está haciendo (`docs/prd.md`, M1).

## 2. Qué queda afuera

- Todo lo de M2: pedidos a medida, cotización estimativa, derivación a un vendedor (`docs/prd.md`, M2).
- Todo lo del PRD §5 "Fuera de alcance" (pagos, panel de administración, integración con el
  sistema de stock existente, generalizar a otros rubros, interpretar imágenes, etc.).
- El research manual del historial de WhatsApp de 6 meses para armar la lista real de FAQs — es
  una tarea de datos que hace el equipo, no código; este plan construye el mecanismo para
  cargarlas, no hace el research en sí.

## 3. Preguntas y riesgos

**Ya resueltas por el PRD y la arquitectura (no se re-preguntan):**
- Canal y modelo: WhatsApp Cloud API directa + Claude Haiku (`docs/arquitectura/stack.md`).
- Tono y reglas de atención: `docs/arquitectura/marca.md`, `docs/arquitectura/user-flow.md`,
  PRD §"Reglas de atención transversales".

**Abiertas — no bloquean construir, se resuelven con datos reales antes de ir a producción:**
- Número de WhatsApp real (PRD, pregunta abierta #1) — se desarrolla contra el número de prueba
  de Meta mientras tanto.
- Fuente real de la lista de precios/stock (PRD, pregunta abierta #2) — este plan construye el
  mecanismo de carga; la data real se suma cuando el dueño la pase.
- Export del historial de 6 meses y el research de FAQs (PRD, pregunta abierta #5) — ídem.
- Enlace de Google y contacto del colocador (PRD, pregunta abierta #8) — ídem.

**Riesgos que aplican a este milestone** (detalle en `docs/prd.md` §4): #2 (lista desactualizada),
#4 (el dueño no confía en el agente), #5 (el particular no percibe diferencia de velocidad),
#6 (research de FAQs impreciso), #7 (cumplimiento de la política de Meta).

## 4. Reglas del proyecto que toca este milestone

- **#1** — cada conversación se guarda aislada por número de cliente; ninguno ve la de otro (M1-02).
- **#2** — las claves de WhatsApp y de Claude salen de variables de entorno (M1-01, M1-03).
- **#3** — el techo de gasto de Claude se chequea antes de cada llamada (M1-03).
- **#7** — el agente responde solo sobre el negocio y deriva lo que no sabe, nunca actúa como
  asistente general (M1-03).
- **#8** — nunca se carga el historial crudo de WhatsApp; solo la lista de FAQs ya curada (M1-02).

## 5. Las piezas de trabajo (ESTA TABLA ES EL ESTADO)

| ID | Pieza | Estado | Depende de | Ficha |
|---|---|---|---|---|
| M1-01 | Conexión con WhatsApp (recibir y enviar mensajes) | ⬜ pendiente | — | `docs/sdd/specs/M1-01-conexion-whatsapp.md` |
| M1-02 | Base de datos: negocio, catálogo y FAQs | ⬜ pendiente | — | `docs/sdd/specs/M1-02-modelo-de-datos.md` |
| M1-03 | El agente clasifica y responde (FAQs + productos estándar) | ⬜ pendiente | M1-01, M1-02 | `docs/sdd/specs/M1-03-agente-responde.md` |

## 6. Cuándo está terminado el milestone

- [ ] Todas las filas de la tabla en ✅ (cada una con sus 3 verificaciones en verde, regla #5).
- [ ] Las condiciones de M1 en `docs/prd.md` se cumplen y se pueden demostrar.
- [ ] **El owner lo probó con sus ojos:** el agente le dejó el número de prueba de WhatsApp
      funcionando y le mostró 1-2 conversaciones reales, y él confirmó que lo que ve está bien.
- [ ] Este plan movido a `docs/sdd/roadmaps/archive/`.
