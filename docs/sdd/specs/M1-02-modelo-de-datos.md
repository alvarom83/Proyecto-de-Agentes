# M1-02 · Base de datos: negocio, catálogo y FAQs

**Depende de:** nada

## 1. Qué queremos lograr

La base de datos existe con las tablas que necesita M1 (`docs/arquitectura/modelo-de-datos.md`),
y alguien del equipo puede cargar o actualizar el catálogo de productos y la lista de FAQs con un
comando simple — sin escribir SQL a mano y sin necesitar un panel visual (fuera de alcance,
`docs/prd.md` §5).

## 2. Qué hay hoy

Nada — no hay base de datos todavía. Se crea sobre Supabase, según lo decidido en
`docs/arquitectura/stack.md` y `docs/arquitectura/modelo-de-datos.md`.

## 3. Qué tiene que hacer (y cómo comprobamos cada cosa)

| # | Qué hace | Cómo se comprueba |
|---|---|---|
| 1 | Crea las tablas `negocio`, `productos_estandar`, `faqs` y `conversaciones` con los campos descriptos en `docs/arquitectura/modelo-de-datos.md` | Las tablas existen en la base con esas columnas (verificable con una migración aplicada y su test) |
| 2 | Permite cargar/actualizar el catálogo de `productos_estandar` (nombre, precio, stock) desde un archivo estructurado (CSV/Excel) sin escribir SQL a mano | Correr el script de carga con un archivo de ejemplo de 3-4 productos y verificar que quedan en la tabla con los valores correctos |
| 3 | Permite cargar/actualizar la lista de `faqs` (pregunta + respuesta) de la misma forma | Correr el script de carga con una lista de ejemplo de 3-4 preguntas y verificar que quedan en la tabla |
| 4 | El script de carga de FAQs descarta cualquier columna que parezca un dato personal (nombre, teléfono, email) — nunca la guarda en `faqs` | Intentar cargar un archivo de ejemplo que incluya una columna "teléfono" y verificar que esa columna no llega a la tabla (regla #8) |
| 5 | `conversaciones` guarda mensajes asociados a un número de cliente puntual, sin mezclarse con los de otro número | Insertar mensajes de dos números de prueba distintos y verificar que una consulta por número trae solo los suyos, nunca los del otro (regla #1) |

## 5. Qué queda afuera

- El research de qué preguntas van en el archivo de FAQs — es un trabajo de datos del equipo
  (PRD, pregunta abierta #5), no de este código.
- Cualquier panel visual para cargar o editar estos datos — la carga es por script, sin pantalla
  (`docs/prd.md` §5).
- La lógica que decide qué responder con estos datos — eso es M1-03.

## 6. Reglas del proyecto que toca

- **#5** — terminada = las 3 verificaciones en verde (ver anexo).
- **#1** — cada conversación queda aislada por número de cliente; la tabla `conversaciones` nunca
  mezcla los mensajes de dos clientes distintos.
- **#8** — el script de carga de FAQs nunca guarda datos personales (nombres, teléfonos) del
  historial de origen — solo la pregunta y la respuesta genéricas.

---

## Anexo técnico (para los agentes)

- **Las 3 verificaciones, en orden (regla #5):** 1) tests unitarios + linter (incluida la
  validación del script de carga con archivos de ejemplo válidos e inválidos) → 2) integración
  (la migración se aplica limpio contra una base de prueba; la prueba de aislamiento del punto 5
  de la tabla) → 3) el recorrido completo: cargar un catálogo y una lista de FAQs de ejemplo de
  punta a punta y leerlos de vuelta. No se avanza a una capa con la anterior en rojo.
- **Test primero:** el test se escribe antes de la implementación y se lo ve fallar por la razón
  correcta antes de darlo por bueno.
- **Entregables:** las migraciones de Supabase para las 4 tablas, el script de carga (ej.
  `scripts/cargar-catalogo.ts` y `scripts/cargar-faqs.ts` o equivalente), y datos de ejemplo para
  probarlo (no datos reales de la zinguería, que todavía no están disponibles).
- **Diseño y alcance no se deciden acá:** el esquema de datos ya está en
  `docs/arquitectura/modelo-de-datos.md`; el si entra o no, en `docs/prd.md`.
