---
id: WHATSAPP
name: WhatsApp CLI (wacli)
description: Enviar mensajes de WhatsApp a terceras personas y buscar/sincronizar historial de WhatsApp usando el CLI wacli. SOLO para mensajes a terceros, no para el canal de chat del agente con el usuario.
icon: 📱
category: messaging
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: WhatsApp CLI (wacli)

Usa `wacli` **únicamente** cuando el usuario pida explícitamente enviar un mensaje a otra persona por WhatsApp, o cuando pida sincronizar/buscar el historial de WhatsApp.

> ⚠️ **Importante:** Esta skill es para mensajes a terceros. Si el usuario está chateando contigo por WhatsApp, respóndele por el canal normal; no uses esta herramienta a menos que pida contactar a otra persona.

## Requisitos

- `wacli` instalado
- Autenticación QR completada (`wacli auth`)

## Instalación

```bash
brew install steipete/tap/wacli
# o
go install github.com/steipete/wacli/cmd/wacli@latest
```

## Seguridad

- Requiere destinatario explícito y texto del mensaje.
- Confirma siempre destinatario y mensaje antes de enviar.
- Ante cualquier ambigüedad, pregunta antes de actuar.

## Autenticación y sincronización

```bash
# Login inicial (escanear QR con el teléfono)
wacli auth

# Sincronización continua
wacli sync --follow

# Diagnóstico
wacli doctor
```

## Buscar chats y mensajes

```bash
# Listar chats
wacli chats list --limit 20 --query "nombre o número"

# Buscar mensajes
wacli messages search "consulta" --limit 20 --chat <jid>

# Buscar por rango de fechas
wacli messages search "factura" --after 2025-01-01 --before 2025-12-31
```

## Rellenar historial

```bash
wacli history backfill --chat <jid> --requests 2 --count 50
```

## Enviar mensajes

```bash
# Texto a número de teléfono
wacli send text --to "+34666123456" --message "Hola, ¿puedes hablar a las 15:00?"

# Mensaje a grupo
wacli send text --to "1234567890-123456789@g.us" --message "Llego 5 minutos tarde."

# Enviar fichero con pie de foto
wacli send file --to "+34666123456" --file /ruta/agenda.pdf --caption "Agenda"
```

## Notas

- Directorio de almacenamiento: `~/.wacli` (override con `--store`).
- Usa `--json` para salida legible por máquina.
- JIDs de chats directos: `<número>@s.whatsapp.net`; grupos: `<id>@g.us`.
- Usa `wacli chats list` para obtener los JIDs de los grupos.
- El relleno de historial requiere que el teléfono esté en línea; los resultados son best-effort.
