---
id: GOOGLE_WORKSPACE
name: Google Workspace CLI (gog)
description: CLI para Gmail, Calendar, Drive, Contactos, Sheets y Docs de Google. Usar cuando el usuario pida gestionar correo Gmail, eventos de Google Calendar, archivos de Drive, hojas de cálculo de Sheets o documentos de Docs.
icon: 📬
category: productivity
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Google Workspace CLI (gog)

`gog` es un CLI moderno para interactuar con Gmail, Calendar, Drive, Contacts, Sheets y Docs. Requiere autenticación OAuth.

## Requisitos

- `gog` instalado (`brew install steipete/tap/gogcli`)
- Credenciales OAuth de Google configuradas

## Configuración inicial (una vez)

```bash
gog auth credentials /ruta/client_secret.json
gog auth add tu@gmail.com --services gmail,calendar,drive,contacts,docs,sheets
gog auth list
```

## Gmail

```bash
# Buscar correos (últimos 7 días)
gog gmail search 'newer_than:7d' --max 10

# Buscar por mensaje individual (sin agrupación por hilo)
gog gmail messages search "in:inbox from:ryanair.com" --max 20

# Enviar (texto plano)
gog gmail send --to destinatario@ejemplo.com --subject "Hola" --body "Mensaje aquí"

# Enviar desde fichero (permite múltiples párrafos)
gog gmail send --to destinatario@ejemplo.com --subject "Hola" --body-file ./mensaje.txt

# Enviar desde stdin
gog gmail send --to destinatario@ejemplo.com --subject "Hola" --body-file - <<'EOF'
Hola,

Este es el cuerpo del mensaje.
Puede tener varias líneas.

Saludos
EOF

# Enviar HTML
gog gmail send --to destinatario@ejemplo.com --subject "Hola" --body-html "<p>Hola</p>"

# Crear borrador
gog gmail drafts create --to dest@ejemplo.com --subject "Borrador" --body-file ./bdr.txt

# Enviar borrador
gog gmail drafts send <draftId>

# Responder a un mensaje
gog gmail send --to dest@ejemplo.com --subject "Re: Asunto" --body "Respuesta" \
  --reply-to-message-id <msgId>
```

## Calendar

```bash
# Listar eventos
gog calendar events <calendarId> --from 2026-04-18T00:00:00Z --to 2026-04-25T23:59:59Z

# Crear evento
gog calendar create <calendarId> --summary "Reunión" \
  --from 2026-04-20T10:00:00Z --to 2026-04-20T11:00:00Z

# Crear evento con color (1-11)
gog calendar create <calendarId> --summary "Reunión" \
  --from 2026-04-20T10:00:00Z --to 2026-04-20T11:00:00Z --event-color 7

# Ver colores disponibles
gog calendar colors

# Actualizar evento
gog calendar update <calendarId> <eventId> --summary "Nuevo título" --event-color 4
```

**Colores de eventos (IDs 1–11):**
`1`=#a4bdfc · `2`=#7ae7bf · `3`=#dbadff · `4`=#ff887c · `5`=#fbd75b · `6`=#ffb878 · `7`=#46d6db · `8`=#e1e1e1 · `9`=#5484ed · `10`=#51b749 · `11`=#dc2127

## Drive

```bash
gog drive search "informe" --max 10
gog drive list
```

## Contactos

```bash
gog contacts list --max 20
```

## Sheets

```bash
# Leer rango
gog sheets get <sheetId> "Hoja1!A1:D10" --json

# Actualizar celdas
gog sheets update <sheetId> "Hoja1!A1:B2" \
  --values-json '[["A","B"],["1","2"]]' --input USER_ENTERED

# Añadir filas
gog sheets append <sheetId> "Hoja1!A:C" \
  --values-json '[["x","y","z"]]' --insert INSERT_ROWS

# Limpiar rango
gog sheets clear <sheetId> "Hoja1!A2:Z"

# Ver metadatos
gog sheets metadata <sheetId> --json
```

## Docs

```bash
# Exportar a TXT
gog docs export <docId> --format txt --out /tmp/doc.txt

# Leer contenido
gog docs cat <docId>
```

## Notas

- Usa `GOG_ACCOUNT=tu@gmail.com` para evitar repetir `--account` en cada comando.
- Para scripts, añade `--json --no-input`.
- `gog gmail search` devuelve hilos; `gog gmail messages search` devuelve mensajes individuales.
- Confirma siempre antes de enviar correos o crear eventos.
- Los docs de Google no admiten edición en-línea con `gog`; para eso usa la API de Docs directamente.
