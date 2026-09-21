<!--
PLANTILLA — DECISIONS.md (registro de decisiones de diseño)
Captura el "POR QUÉ" que la compactación de contexto suele perder.
Formato mínimo por entrada: qué decisión, por qué, alternativa rechazada, constraint, fecha.
-->

## 2026-09-14: Arquitectura del agente — Next.js/Vercel, Supabase, WhatsApp Cloud API directa, Claude
- **Decisión:** `/arquitectura` fija el stack: Next.js (API routes) en Vercel, Supabase
  (Postgres) como base de datos, conexión **directa** a la API oficial de WhatsApp Business
  (Meta Cloud API, sin BSP intermediario) y **Claude (Anthropic, modelo Haiku)** como el modelo
  que clasifica intención, redacta respuestas y calcula la estimación a medida. Se crean los 5
  documentos siempre obligatorios (`stack.md`, `modelo-de-datos.md`, `secretos.md`,
  `user-flow.md`, `marca.md`) más los dos condicionales que sí aplican (`integraciones-ia.md`,
  `integraciones.md`); `auth-y-permisos.md` no se crea (no hay login de usuario en el MVP). Se
  suman tres reglas nuevas a la constitución: #6 (nunca presentar una estimación como precio
  firme), #7 (el agente nunca actúa como asistente de propósito general, cumpliendo la política
  de Meta), #8 (el historial de 6 meses nunca se conecta en vivo al agente).
- **Razón:** conexión directa a Meta evita el costo extra de un proveedor intermediario (BSP),
  relevante dado que ICP-03 es sensible al costo mensual; Claude Haiku alcanza para un dominio
  acotado (un negocio, reglas de atención bien definidas) al menor costo posible; Next.js/Vercel
  y Supabase son la ruta de menor fricción y costo (capa gratuita) para el volumen de un solo
  negocio, y dejan la puerta abierta a un futuro panel de administración sin migrar de stack.
- **Alternativa rechazada:** un BSP tipo Twilio/360dialog (más rápido de conectar pero suma un
  costo mensual/por mensaje extra sin necesitarse su capa de gestión de equipo/inbox en este
  MVP) · GPT (OpenAI) como alternativa de igual costo aproximado, sin ventaja clara sobre usar
  el mismo proveedor con el que ya se construye el proyecto.
- **Constraint:** el dueño de la zinguería tiene que completar la verificación de su Cuenta de
  Negocio en Meta antes de poder operar en producción (tarea humana, no de código) — mientras
  tanto se puede desarrollar contra el número de prueba de Meta.

## 2026-09-14: El Diseñador define user-flow y marca — sin menú inicial, tono institucional
- **Decisión:** `user-flow.md` fija que no hay menú de bienvenida (el agente responde directo al
  primer mensaje) y una regla de desambiguación entre producto estándar y a medida basada en si
  el mensaje menciona una obra/proyecto en curso, no en quién pregunta. `marca.md` fija el tono
  institucional en plural ("nosotros"), sin mención de IA/chatbot expuesta al cliente, con
  perfil de WhatsApp Business que se ve como un negocio real (no un producto de software).
- **Razón:** ambos documentos los propuso el Diseñador (`ux-reviewer`, modo autor) a partir del
  PRD y los tres ICP; el Arquitecto los revisó y resolvió los puntos técnicos ambiguos
  (desambiguación de ramas) que el Diseñador había marcado como abiertos.
- **Alternativa rechazada:** un menú de opciones al iniciar la conversación — se descartó por ir
  en contra de lo que valora ICP-01 (velocidad por sobre fricción).
- **Constraint:** el nombre/foto exactos del perfil de WhatsApp y el texto exacto de saludo/cierre
  quedan como abiertos — dependen de información real de la zinguería, no de una decisión técnica.

## 2026-09-07: 14 reglas de atención reales del negocio + el agente SÍ puede estimar a medida
- **Decisión:** se incorporan al PRD las 14 reglas de atención con las que hoy trabaja el
  equipo de venta de la zinguería, como una sección transversal ("Reglas de atención
  transversales") bajo Milestones. La más importante cambia una decisión previa: el agente
  **sí puede dar una cotización estimativa** de un pedido a medida (chapa galvanizada + calibre
  por defecto si no se especifica material), aclarando siempre que es aproximada — reemplaza al
  Riesgo #1 anterior ("cero tolerancia, el agente nunca pone precio a lo que es a medida"). El
  precio **final** y su confirmación siguen siendo exclusivos de un vendedor humano. También se
  suma que el agente pregunta y registra el tipo de cliente (Consumidor Final, Gremio,
  Mayorista, Profesional, Empresa Constructora) en M2, pero la clasificación definitiva la hace
  el vendedor al cerrar la venta, no el agente.
- **Razón:** las reglas 3 y 4 que dio el owner ("responder con soluciones, no interrogatorios";
  "si no se especifica el material, cotizar en chapa galvanizada con el calibre adecuado")
  describen la práctica real de venta del negocio — dar una estimación rápida vende mejor que
  frenar la conversación pidiendo datos. Mantener la regla de "cero tolerancia" original hubiera
  ido en contra de cómo realmente atienden hoy.
- **Alternativa rechazada:** mantener la regla original de "nunca dar ningún número" y limitar
  las reglas 3/4 a temas no relacionados con precio — el owner eligió explícitamente que el
  agente sí estime, con la aclaración de que es aproximada.
- **Constraint:** la estimación nunca se presenta como precio firme (eso sigue siendo cero
  tolerancia); falta definir los calibres/valores por defecto por tipo de producto (nueva
  pregunta abierta §6.7 del PRD) antes de poder construir M2.

## 2026-09-07: El PRD incorpora la política de Meta para WhatsApp Business (2026)
- **Decisión:** se suma como riesgo (#7) y como refuerzo de "fuera de alcance" en el PRD que,
  desde enero de 2026, Meta prohíbe los chatbots de "propósito general" en WhatsApp Business y
  solo permite automatización estrictamente de negocio (soporte, FAQs, pedidos, seguimiento)
  con salida clara a un humano — el agente ya estaba diseñado así, pero ahora queda explícito
  como requisito de cumplimiento, no solo como elección de producto. Se agregan también dos
  buenas prácticas de Meta a M1 (respuestas cortas, usar botones/listas de WhatsApp en vez de
  texto libre) y una pregunta abierta sobre la regla de las 24 horas: fuera de esa ventana desde
  el último mensaje del cliente, Meta exige mensajes de plantilla aprobados — relevante para
  cuando el vendedor le avisa al cliente que su pedido a medida (M2) ya está cotizado.
- **Razón:** el owner pidió sumar buenas prácticas de Meta al PRD; investigar la política
  vigente evita que el proyecto quede fuera de cumplimiento (riesgo real: Meta puede restringir
  o banear el número de WhatsApp del negocio).
- **Fuente:** búsqueda web sobre la política de Meta/WhatsApp Business Platform 2026 (chatbots
  de propósito general prohibidos desde el 15 de enero de 2026, vigente para cuentas nuevas
  desde el 15 de octubre de 2025) — no verificado contra la documentación oficial de Meta
  directamente, conviene reconfirmar en `/arquitectura` antes de implementar mensajería.
- **Constraint:** el detalle técnico de plantillas aprobadas y ventana de 24hs lo resuelve
  `/arquitectura` (probablemente en `secretos.md` o un futuro `integraciones.md`), no este PRD.

## 2026-09-07: M1 suma preguntas frecuentes del negocio, armadas del historial de WhatsApp
- **Decisión:** M1 del PRD se amplía de "consultas de productos estándar" a incluir también
  preguntas frecuentes generales del negocio (medios de pago, envíos, garantías, redes
  sociales, CUIT — información pública del negocio, no datos externos). La lista de FAQs se
  arma con un research puntual, una sola vez, sobre el historial real de WhatsApp de los
  últimos 6 meses del local. De ese research solo se guarda la lista de preguntas y respuestas
  genéricas — el historial crudo (nombres, teléfonos, conversaciones privadas) no se carga ni
  queda conectado al agente en producción.
- **Razón:** usar el historial real evita inventar preguntas frecuentes a ojo — refleja lo que
  los clientes preguntan de verdad. El owner aclaró explícitamente que le interesan las
  respuestas resultantes, no los datos personales de los clientes.
- **Alternativa rechazada:** conectar el agente al historial completo como fuente de
  conocimiento en vivo (expone datos de clientes reales en cada uso, y toca la regla #1 de la
  constitución sobre datos de usuarios) · incluir datos públicos externos al negocio (clima,
  cotizaciones) — fuera del alcance de "preguntas frecuentes del negocio".
- **Constraint:** el research del historial es manual y previo a M1, no un pipeline automático;
  queda como pregunta abierta (§6 del PRD) quién hace el export y cuándo.

## 2026-09-07: Solución escrita como producto general para PyMEs, precio USD 30-50/mes
- **Decisión:** `docs/solucion.md` describe el agente como producto para "PyMEs con local a la
  calle" en general (no acotado a zinguerías), con la zinguería como primer caso de uso real.
  Modelo de cobro: suscripción mensual, precio ancla USD 30-50/mes (a ajustar cuando `/costo`
  estime el costo real de infra + IA + mensajería). Setup asistido (no self-serve) en el MVP,
  sin panel de administración propio — coherente con `docs/prd.md` §5.
- **Razón:** coincide con la idea original del owner ("agente para pymes con locales a la
  calle") y con el tipo de solución elegido (Producto/Agentes, no implementación a medida). El
  precio se fijó en el rango que prioriza margen sobre adopción rápida agresiva, dado el
  contexto de ICP-03 (dueño de pyme sensible al costo pero con el problema activo).
- **Alternativa rechazada:** acotar la solución solo a zinguerías/corralones por ahora
  (descartado por el owner) · precio más bajo (USD 10-20/mes, prioriza adopción sobre margen) ·
  pago único sin mensualidad (no cubre costos recurrentes de un producto que corre todo el
  tiempo).
- **Constraint:** el precio exacto queda pendiente de `docs/arquitectura/costos.md` (lo escribe
  `/costo`); la generalización a otros rubros más allá de zinguería es un supuesto sin validar
  (ver "Supuestos abiertos" de `docs/solucion.md`).

## 2026-09-07: /solucion es tipo Producto, y sumó ICP-03 (el comprador) antes de escribirse
- **Decisión:** el agente de WhatsApp se define como tipo `producto`/`agentes` (no
  implementación a medida, ni acompañamiento, ni curso) — el cliente lo usa, nadie ejecuta un
  engagement por cada PyME. Antes de escribir `docs/solucion.md`, se pausó para construir
  ICP-03 (`docs/icp/icp-03-dueno-pyme.md`): el dueño de zinguería/corralón que decidiría
  adoptarlo, porque ICP-01 e ICP-02 describen a los clientes finales del local, no a quien
  compra el producto.
- **Razón:** la idea original ("agente para PyMEs con locales a la calle") apunta a un producto
  reutilizable, con la zinguería como primer caso de uso concreto — no un proyecto cerrado para
  un solo cliente. Y la anatomía de "producto" exige un ICP del comprador, que todavía no existía.
- **Alternativa rechazada:** tratar esto como implementación a medida (DFW) solo para esta
  zinguería · seguir con un ICP-supuesto declarado dentro de `docs/solucion.md` en vez de
  documentar ICP-03 aparte (el owner prefirió correr `/icp` primero).
- **Constraint:** ICP-03 sigue siendo v1-hipótesis — antes de invertir en generalizar el
  producto a otros rubros, conviene validarlo con el dueño real de la zinguería.

## 2026-09-07: Dos ICPs para el agente de zinguería, ambos v1-hipótesis
- **Decisión:** documentar dos perfiles de cliente en paralelo, uno por documento en
  `docs/icp/`: ICP-01 (particular que compra estándar, corresponde a M1 del PRD) e ICP-02
  (contratista que compra a medida, corresponde a M2). Los dos arrancan como v1-hipótesis
  (sin evidencia real todavía) y con el núcleo + módulo Ventas — no Marketing ni Social
  Listening, porque este ICP alimenta el diseño de la conversación del agente, no publicidad.
- **Razón:** el evento disparador, el JTBD priorizado y el lenguaje de ambos perfiles difieren
  lo suficiente como para ser dos perfiles, no uno diluido; y el owner prefirió documentar los
  dos ahora en vez de posponer el segundo.
- **Alternativa rechazada:** un solo ICP unificado (mezclaba señales de fit contradictorias
  entre "urgencia doméstica chica" y "cotización de obra") · empezar solo con uno y posponer el
  otro (descartado por el owner, que pidió ambos ahora).
- **Constraint:** sin evidencia real todavía — las hipótesis marcadas 🔴 en la sección
  "Supuestos abiertos" de cada documento deberían validarse con conversaciones reales del
  WhatsApp de la zinguería antes de darle mucho peso al lenguaje o a las señales de venta.

## 2026-08-18: Arquitectura multi-documento + el PRD como mapa
- **Decisión:** `/arquitectura` vuelve a escribir un documento por tema en `docs/arquitectura/` — siempre: `stack.md` (incluye deploy y costo mensual), `modelo-de-datos.md` (#1), `secretos.md` (#2), `user-flow.md`, `marca.md`; condicionales (si no aplican, no se crean y el índice lo marca): `integraciones-ia.md` (#3), `integraciones.md` (#4), `auth-y-permisos.md`. Al terminar, el Arquitecto completa la sección §7 "El cómo técnico" de `docs/prd.md` con los links. Sigue sin subagentes (todo en la misma pasada).
- **Razón:** un archivo único mezclaba temas de peso muy distinto; documentos separados permiten linkear con precisión desde specs y constitución, y el PRD como mapa le da al owner un solo punto de entrada. La lista se alineó con la slide 20 de la masterclass (stack · modelo de datos · user flow · marca · IA · autenticación · deploy), sumando marca y auth que faltaban en la lista inicial.
- **Alternativa rechazada:** mantener `decisiones.md` único (mezclaba todo) · un doc por tema con fan-out a subagentes (lento, ya descartado en v2).
- **Constraint:** los condicionales no se rellenan con "N/A" largos — no se crean; el índice de `docs/arquitectura/README.md` y el §7 del PRD son quienes registran el descarte en una línea.

## 2026-08-18: Alinear los comandos al workflow de la masterclass
- **Decisión:** el harness adopta los 8 comandos del mapa público de la masterclass (slide 18): `/icp` · `/solucion` · `/prd` · `/arquitectura` · `/roadmap` · `/specs` · `/implementar` · `/deploy`. Renombres: `/new-prd`→`/prd`, `/new-architecture`→`/arquitectura`, `/deploy-check`→`/deploy` (ahora con GO + confirmación ejecuta el deploy real). `/new-plan` se parte en `/roadmap` (el plan) y `/specs` (las fichas), ambos orquestando la misma skill `planner`. Nuevo `/implementar <ID>` como wrapper del despacho a `builder`. `/empezar` y `/save-point` siguen como transversales.
- **Razón:** un solo vocabulario entre la charla, el curso y el harness — lo que el alumno ve en las slides es exactamente lo que tipea en el repo.
- **Alternativa rechazada:** mantener `/new-plan` fusionado y ajustar las slides — el material público manda; la fusión se conserva a nivel doctrina (una skill), solo se parte la orquestación.
- **Constraint:** la skill `planner` sigue siendo la única dueña de la doctrina de plan+specs; los dos comandos solo ejecutan fases distintas de ella.

## 2026-08-17: Harness v2.1 — optimización 80/20 para founders no técnicos
- **Decisión:** cuatro mejoras a la interfaz con el founder: (1) `/empezar` como puerta de entrada humana (dónde está el proyecto + el único paso siguiente); (2) "probalo vos" al cerrar cada milestone — el agente levanta la app, sirve el link y el milestone no se archiva sin la confirmación del owner (nunca a nivel spec: sería mucho); (3) toda pregunta técnica se sirve digerida — qué significa en llano, pros/contras y una opción sugerida; (4) borrar `QUALITY.md`.
- **Razón:** análisis de primeros principios + Pareto: el núcleo del método ya estaba; los gaps eran de interfaz humana — el founder no puede leer código, solo puede juzgar usando la app y con explicaciones en llano, y no sabe levantar servers.
- **Alternativa rechazada:** ritual de "cuando algo se rompe" (descartado por el owner) · renombrar los comandos a verbos de founder (se mantienen los nombres actuales por el material del curso) · "probalo vos" por spec (demasiada fricción).
- **Constraint:** la regla de trade-offs vive en las skills (doctrina); el "probalo vos" en CLAUDE.md y las plantillas (flujo) — sin duplicar entre capas.

## 2026-08-17: Simplificar el harness (v2 — rápido y en llano)
- **Decisión:** reducir el equipo de 11 a 6 roles (el Programador `builder` implementa Y verifica; el Planificador `planner` hace plan + specs en un comando `/new-plan`; DevOps absorbe el chequeo de costo), dejar 2 rondas de preguntas en todo el flujo, un solo archivo de arquitectura (`decisiones.md`), estado en un solo lugar (la tabla del plan), plantillas cortas cuyas secciones no aplicables se borran, y todos los documentos visibles en lenguaje llano con anexos técnicos para los agentes. Tres capas con responsabilidad única: skill = CÓMO, agente = QUIÉN, command = ejecución — sin duplicar contenido entre capas.
- **Razón:** en la demo en vivo el harness v1 era lentísimo: ~22.500 palabras de instrucciones, ~18 subagentes mayormente en serie, hasta 11 rondas de preguntas y estado por triplicado. Y el usuario final no es técnico.
- **Alternativa rechazada:** mantener el harness completo y agregar un modo `--fast` aparte — doble mantenimiento; y eliminar las skills moviendo la doctrina a los commands — rompía la separación CÓMO/QUIÉN/ejecución.
- **Constraint:** toda doctrina vive en su skill y se referencia (no se copia) desde commands y agentes; el punto de retorno pre-cambio es el commit `42eb607`.

## <AAAA-MM-DD>: <Título corto de la decisión>
- **Decisión:** <qué se decidió>
- **Razón:** <por qué>
- **Alternativa rechazada:** <qué se descartó y por qué>
- **Constraint / consecuencia:** <restricción que esto impone hacia adelante>

## 2026-01-15: Usar Redis para cachear preferencias de usuario
- **Decisión:** cachear las preferencias de usuario en Redis.
- **Razón:** alta frecuencia de lectura (cada llamada de API), datos chicos.
- **Alternativa rechazada:** vista materializada en PostgreSQL — la alta frecuencia de cambio no justifica el costo de mantenimiento.
- **Constraint:** TTL de cache de 5 minutos, invalidación activa al escribir.
