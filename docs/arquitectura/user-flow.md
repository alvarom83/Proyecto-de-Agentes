# User Flow — Agente WhatsApp para Zinguería

> Derivado de: `docs/prd.md` (MVP, M1, M2, reglas de atención transversales #1-14, riesgo #7) ·
> `docs/icp/icp-01-particular.md` · `docs/icp/icp-02-contratista.md` · `docs/icp/icp-03-dueno-pyme.md`.
> No hay pantallas: la única superficie de producto es la conversación de WhatsApp y el perfil de
> WhatsApp Business del negocio. Cada afirmación se traza a uno de esos documentos.

## 1. Principio general del flujo

El "recorrido" es una conversación de WhatsApp, no una secuencia de pantallas. Por eso el flujo
se define en términos de **turnos de mensaje** (qué llega, qué responde el agente) y no de
vistas. Todo turno respeta las reglas de atención transversales del PRD (§"Reglas de atención
transversales"), en particular:

- **Regla #1** (leer todo el hilo antes de responder) aplica en cada turno, no solo al primero.
- **Regla #5** (un solo tema por mensaje) — el agente nunca junta dos respuestas distintas en un
  mismo mensaje, aunque eso signifique mandar dos mensajes seguidos.
- **Regla #6** (institucional, en plural) — ningún mensaje del agente habla en primera persona
  del singular.
- **Regla #7** (uso "de negocio", nunca de propósito general — riesgo #7 del PRD) — el agente
  nunca sostiene una conversación fuera de productos/pedidos/preguntas del local; ante cualquier
  desvío, redirige o deriva.

## 2. Primer contacto (cliente nuevo)

**Qué dispara este paso:** un número que nunca escribió antes le manda un mensaje al WhatsApp del
negocio (PRD §2, "un cliente escribe por WhatsApp preguntando por un producto o algo del
negocio").

| Paso | Qué hace el agente | Qué dice/pregunta |
|---|---|---|
| 1 | Recibe el primer mensaje, sea cual sea su contenido | No hay menú de bienvenida — el agente lee el mensaje y responde directo a lo que preguntan (regla #1) |
| 2 | Clasifica la intención del mensaje (ver §3) | — |
| 3 | Responde en el registro institucional (`marca.md`), adaptando el nivel de lenguaje al perfil que reconoce (simple si suena a ICP-01, más técnico si suena a ICP-02 — ver `marca.md` §2) | — |

**Decisión: no hay flujo de "onboarding" ni menú inicial.** No se le pide al cliente que se
registre ni que elija una opción antes de poder preguntar algo — responde directo al primer
mensaje. Es consistente con que WhatsApp ya es el canal habitual del negocio (`docs/solucion.md`
§5) y con que ICP-01 valora sobre todo la velocidad, no la fricción de un menú (ICP-01 §4, §7).

## 3. La bifurcación: cómo el agente distingue una rama de otra

No es el cliente quien elige una rama desde un menú — el agente interpreta el contenido del
mensaje. Señales de cada rama (tomadas de PRD M1/M2 e ICP §"Señales de fit"):

| Rama | Señales que la activan | Ejemplos |
|---|---|---|
| **A. Pregunta frecuente del negocio** (M1) | No menciona un producto puntual; pregunta sobre el negocio en sí | "¿tienen tarjeta?", "¿hacen envíos?", "¿dónde están ubicados?", "¿tienen garantía?" |
| **B. Producto estándar** (M1) | Menciona un producto de catálogo, cantidad chica, sin plano ni obra en curso | "se me voló una chapa, ¿tienen para reponer?", "¿cuánto sale la chapa acanalada?" |
| **C. Pedido a medida** (M2) | Menciona medidas específicas, "a medida", una obra o proyecto en curso, o pide cotización para varios ítems | "necesito chapa a medida para una obra, ¿me cotizan?" |

**Regla de desambiguación para el caso ambiguo** (ICP-02 §V3: un contratista chico que a veces
compra estándar para stock): la señal que decide no es *quién* pregunta ni *qué tipo de
producto* pide, sino **si el mensaje menciona una obra o proyecto en curso**. Sin esa mención, se
trata como Rama B aunque quien escriba sea, por su forma de hablar, un contratista.

**Si el agente no puede clasificar el mensaje o no tiene la respuesta:** nunca inventa un dato —
avisa que un vendedor va a contestar (PRD M1, último ítem de "Está terminado cuando").

## 4. Rama A — Pregunta frecuente del negocio (M1)

| Paso | Qué hace el agente | Qué dice/pregunta |
|---|---|---|
| 1 | Identifica el tema puntual (horario, ubicación, medios de pago, envíos, garantía, descuentos, colocador, redes, CUIT) | — |
| 2 | Responde con la información pública real del negocio, un solo tema por mensaje (regla #5) | Respuesta corta y directa (buena práctica de Meta, PRD M1) |
| 3 | Si corresponde, agrega el recurso asociado a esa pregunta | Tarjeta de contacto del colocador (regla #11) · enlace oficial de Google del local junto con la ubicación (regla #12) · aclaración de que los descuentos son solo en mostrador (regla #10) |
| 4 | Si no tiene el dato | "Un vendedor te va a confirmar esto en breve" (nunca inventa, PRD M1) |
| 5 | Cierra el intercambio de forma cordial si no hay más preguntas (regla #14) | Ver `marca.md` §4 |

## 5. Rama B — Producto estándar (M1)

| Paso | Qué hace el agente | Qué dice/pregunta |
|---|---|---|
| 1 | Identifica el producto mencionado, en el lenguaje del cliente, sin exigir vocabulario técnico (ICP-01 §7) | Puede preguntar qué material tiene hoy en el techo para orientarlo, en lenguaje simple (ICP-01 §7) |
| 2 | Responde precio y stock si los tiene cargados | Respuesta corta, un tema por mensaje |
| 3 | Si el precio/stock no está disponible o desactualizado | Deriva a un vendedor, nunca inventa (PRD riesgo #2) |
| 4 | Si el cliente manda foto/video del problema (ej. la chapa rota) | Agradece el archivo (regla #9) antes de seguir respondiendo |
| 5 | Cierra de forma cordial si el cliente no sigue con un pedido a medida | Ver `marca.md` §4 |

## 6. Rama C — Pedido a medida (M2)

Esta rama es la que más se aparta de un flujo de FAQ y donde más pesan las reglas de atención.

| Paso | Qué hace el agente | Qué dice/pregunta |
|---|---|---|
| 1 | Reconoce la intención de "a medida" (§3, Rama C) | — |
| 2 | **Responde primero con una cotización estimativa**, sin abrir con preguntas (regla #3) — en chapa galvanizada y el calibre por defecto si no se especificó material (regla #4) | Aclara siempre que es un valor aproximado sujeto a confirmación de un vendedor (regla #4, riesgo #1 del PRD — cero tolerancia a presentarlo como precio firme) |
| 3 | Si el cliente quiere avanzar, recién ahí pide lo que falta — de a un dato por mensaje si hace falta (regla #5) | Medidas exactas · datos de contacto · tipo de cliente: Consumidor Final, Gremio, Mayorista, Profesional o Empresa Constructora (regla #13; la clasificación final la confirma el vendedor, no el agente) |
| 4 | Si el cliente manda foto, video o bosquejo con las medidas | Agradece y lo suma al pedido tal cual, sin "leer" la imagen (regla #9; fuera de alcance interpretar el contenido — PRD §5) |
| 5 | Si escriben varias personas por el mismo pedido | Identifica y fija un único interlocutor; no le sigue respondiendo a los demás sobre ese mismo pedido (regla #8) |
| 6 | Todo cambio o ajuste del pedido que se acuerde en el chat | Queda registrado por escrito en la conversación misma, nunca "de palabra" (regla #2) |
| 7 | **Derivación al vendedor** (ver §7) | — |
| 8 | Cierre cordial | Ver `marca.md` §4 |

## 7. El punto de derivación a un vendedor humano

**Qué se le pasa al vendedor** (PRD M2, "Está terminado cuando"):
- La estimación ya dada (material, calibre, precio aproximado).
- Las medidas exactas.
- Los datos de contacto del interlocutor único identificado.
- El tipo de cliente que declaró (Consumidor Final / Gremio / Mayorista / Profesional / Empresa
  Constructora).
- Cualquier foto/video/bosquejo recibido, sin procesar.

**Qué le confirma el agente al cliente mientras espera** (PRD M2, ICP-02 §7):
- Que el pedido fue recibido.
- En cuánto tiempo aproximado un vendedor lo va a contactar para confirmar el precio final — la
  espera es la principal objeción de este perfil (ICP-02 §6).

El tiempo exacto que se comunica (ej. "dentro de 24hs" o "en el día") lo tiene que fijar el
dueño según su capacidad real de respuesta — ver "Abierto" más abajo; el agente usa ese valor
configurado, nunca un número inventado.

**Nota:** cómo el vendedor recibe esa derivación (mismo WhatsApp, otro medio) y qué pasa si
pasaron más de 24hs desde el último mensaje del cliente (ventana de mensajes de plantilla de
Meta) se resuelve en `integraciones.md` y `secretos.md` (PRD, pregunta abierta #6).

## 8. Cierre cordial

Aplica a toda rama (regla #14: "nunca deja un intercambio cortado sin un cierre"). Ver `marca.md`
§4 para el texto/tono exacto. El cierre no es un paso opcional — es parte de "terminado" del
recorrido tanto como responder la consulta.

## 9. Esquema de referencia rápida

```
Cliente escribe (primer mensaje o mensaje dentro de un hilo existente)
        │
        ▼
El agente lee TODO el hilo (regla #1) ── ¿mensaje ambiguo? ──► pide una sola aclaración (regla #5)
        │
        ▼
  Clasifica intención
        │
        ├── A. Pregunta frecuente del negocio ──► responde info pública
        │        (horario, pago, envíos, garantía,       + tarjeta colocador / link Google
        │         descuentos, ubicación, colocador)          si corresponde (#11, #12)
        │
        ├── B. Producto estándar ──► responde precio/stock directo
        │        (catálogo, cantidad chica,                (lenguaje simple si es ICP-01)
        │         sin obra en curso)
        │
        └── C. Pedido a medida ──► responde ESTIMACIÓN primero (#3, #4)
                 (medidas, obra en curso, "a medida")│
                                                      ▼
                                          ¿cliente quiere avanzar?
                                                 │           │
                                                 sí          no → cierre cordial
                                                 ▼
                                    pide medidas exactas + contacto
                                    + tipo de cliente (#13), de a un
                                    tema por mensaje (#5)
                                                 │
                                    ¿varias personas por el mismo pedido?
                                    → fija un único interlocutor (#8)
                                                 │
                                    ¿manda foto/video/bosquejo?
                                    → agradece y suma sin interpretar (#9)
                                                 │
                                                 ▼
                                    DERIVA a vendedor humano con todo
                                    el pedido armado + confirma tiempo
                                    de espera al cliente
                                                 │
                                                 ▼
                                        cierre cordial (#14)
        │
  (cualquier rama sin respuesta posible)
        │
        ▼
  "un vendedor te va a confirmar esto" — nunca inventa un dato
        │
        ▼
  cierre cordial (#14)
```

## Abierto

1. El tiempo estimado exacto que se le comunica al cliente que espera un pedido a medida (ej.
   "en el día", "dentro de 24hs") — depende de la capacidad real de respuesta del equipo de
   venta; lo define el dueño antes de M2 (ligado a la pregunta abierta #3 del PRD).
2. Texto exacto de la aclaración que da el agente cuando el mensaje es ambiguo entre Rama B y
   Rama C — se resuelve al escribir las instrucciones del agente (spec de M1/M2), no acá.
