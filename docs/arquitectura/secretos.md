# Secretos y claves

> **Dueño de:** qué claves necesita el proyecto, de dónde salen, y cómo nunca tocan el código
> (regla **#2**).

## Regla #2, en criollo

Ninguna clave (token, contraseña, credencial) se escribe nunca dentro de un archivo de código ni
se sube al repositorio de GitHub. Todas viven como **variables de entorno**: en la nube
(configuradas en el panel de Vercel) y, en la máquina de quien programa, en un archivo `.env.local`
que Git ignora explícitamente (nunca se comitea).

## Claves que necesita este proyecto

| Variable | Para qué | De dónde sale |
|---|---|---|
| `WHATSAPP_ACCESS_TOKEN` | Autenticarse contra la API de WhatsApp Cloud (Meta) para mandar mensajes | Se genera en el panel de Meta for Developers, una vez verificada la cuenta de negocio (ver `stack.md` §Abierto) |
| `WHATSAPP_PHONE_NUMBER_ID` | Identifica el número de WhatsApp del negocio dentro de la API de Meta | Panel de Meta for Developers |
| `WHATSAPP_WEBHOOK_VERIFY_TOKEN` | Confirma que las notificaciones de mensajes nuevos vienen realmente de Meta | Se define al configurar el webhook, no lo entrega Meta |
| `ANTHROPIC_API_KEY` | Autenticarse contra la API de Claude, el modelo que redacta las respuestas | Panel de Anthropic (console.anthropic.com) |
| `DATABASE_URL` | Conectarse a la base de datos (Supabase) | Panel de Supabase, al crear el proyecto |
| `VENDEDOR_WHATSAPP_NUMBER` (u otro medio de notificación, ver `integraciones.md`) | A qué número/canal se le avisa cuando llega un pedido a medida (PRD, pregunta abierta #3) | Lo define el dueño de la zinguería |

Ninguna de estas claves pertenece a un usuario final (cliente que escribe por WhatsApp) — todas
son credenciales del negocio/proyecto, así que no hay conflicto con la regla #1 (aislamiento de
datos entre usuarios).

## Cómo se cargan

- **En desarrollo:** archivo `.env.local` en la raíz del proyecto, listado en `.gitignore`.
- **En producción:** panel de "Environment Variables" de Vercel — se configuran ahí directamente,
  nunca en un archivo que viaje con el código.

## Abierto

- El token y el ID de número de WhatsApp dependen de que el dueño complete la verificación de su
  Cuenta de Negocio en Meta (ver `stack.md` §Abierto) — hasta entonces, se puede desarrollar
  contra un número de prueba que Meta provee sin necesitar la verificación final.
