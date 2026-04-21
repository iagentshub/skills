---
id: IMESSAGE
name: iMessage / SMS (imsg)
description: Leer y enviar mensajes de iMessage/SMS en macOS usando el CLI imsg a través de Messages.app. Solo para macOS con Messages firmado en cuenta de Apple ID.
icon: 📨
category: messaging
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: iMessage / SMS (imsg)

> ⚠️ **Solo macOS.** Requiere Messages.app iniciado sesión con un Apple ID.

Usa `imsg` para leer y enviar iMessage/SMS a través de la app Messages de macOS.

## Cuándo usar

✅ **USA esta skill cuando:**

- El usuario pide explícitamente enviar un iMessage o SMS
- Leer el historial de conversaciones de iMessage
- Comprobar chats recientes en Messages.app
- Enviar a números de teléfono o Apple IDs

## Cuándo NO usar

❌ **NO uses esta skill cuando:**

- Mensajes de Telegram, Signal, WhatsApp o Discord → usa sus respectivos canales
- Gestión de grupos (añadir/eliminar miembros) → no soportado
- Mensajes masivos → siempre confirma con el usuario antes
- El usuario está chateando contigo por iMessage → respóndele normalmente

## Requisitos

- macOS con Messages.app iniciado sesión
- Acceso Completo al Disco para el terminal
- Permiso de Automatización para Messages.app (para envío)

## Instalación

```bash
brew install steipete/tap/imsg
```

## Comandos

### Listar chats

```bash
imsg chats --limit 10 --json
```

### Ver historial de conversación

```bash
# Por ID de chat
imsg history --chat-id 1 --limit 20 --json

# Con información de adjuntos
imsg history --chat-id 1 --limit 20 --attachments --json
```

### Enviar mensaje

```bash
# A número de teléfono
imsg send --to "+34666123456" --message "Hola, ¿estás disponible?"

# A Apple ID (email)
imsg send --to "persona@icloud.com" --message "Te mando los documentos ahora"
```

### Monitorizar mensajes nuevos

```bash
imsg watch --chat-id 1 --attachments
```

## Notas

- El ID de chat se obtiene con `imsg chats --limit 20`.
- Los mensajes se envían a través de Messages.app (no directamente a la red de Apple).
- Confirma siempre el destinatario y el mensaje antes de enviar.
- Solo macOS; no funciona en Linux ni Windows.
