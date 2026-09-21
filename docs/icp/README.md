# ICP — para quién construimos

> **Dueño de:** el perfil de cliente que el proyecto sirve — **un documento por perfil**, escritos
> por `/icp`. **No cubre:** el producto (→ [`../solucion.md`](../solucion.md), si existe) ni el
> alcance (→ [`../prd.md`](../prd.md)).

Hay **dos capas de cliente** en este proyecto, y no hay que confundirlas:
- **Quien le escribe al local** por WhatsApp (los clientes de la zinguería) — ICP-01 e ICP-02.
- **Quien decide adoptar el agente** (el dueño de la PyME) — ICP-03. Es el ICP que manda para
  `docs/solucion.md`, porque es quien compra el producto.

Los tres perfiles se documentan por separado porque su evento disparador, su JTBD priorizado y
su lenguaje difieren.

## Mapa perfil → documento

| ICP | Documento | Estado | Se corresponde con |
|---|---|---|---|
| ICP-01 — Particular / compra estándar | `icp-01-particular.md` | Hipótesis | M1 del PRD |
| ICP-02 — Contratista / compra a medida | `icp-02-contratista.md` | Hipótesis | M2 del PRD |
| ICP-03 — Dueño de zinguería/corralón (comprador del agente) | `icp-03-dueno-pyme.md` | Hipótesis | `docs/solucion.md` |

Los tres arrancan como **v1-hipótesis**: no hay todavía evidencia directa (conversaciones,
entrevistas) de clientes reales de esta zinguería — se construyeron a partir del conocimiento
del negocio. Antes de tener usuarios reales conviene validar al menos las hipótesis marcadas
🔴 en la sección "Supuestos abiertos" de cada documento.

Reglas de esta carpeta:
- **Un ICP por documento.** Si aparece un tercer perfil, se agrega como `icp-03-<nombre>.md` y se
  suma a esta tabla.
- El porqué de decisiones durables (priorización de nicho, fusión/separación de perfiles) va a
  [`../../DECISIONS.md`](../../DECISIONS.md).
