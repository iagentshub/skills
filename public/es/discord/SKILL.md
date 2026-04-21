---
id: DISCORD
name: Discord
description: "Operaciones de Discord: enviar mensajes (con o sin adjuntos), reaccionar, leer canales, gestionar roles y moderar. Requiere un bot token de Discord configurado como canal."
icon: 🎮
category: messaging
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Discord

Operaciones de Discord mediante la herramienta de mensajería con `channel: "discord"`.

## Reglas principales

- Siempre usa `channel: "discord"`.
- Prefiere IDs explícitos: `guildId`, `channelId`, `messageId`, `userId`.
- Evita tablas Markdown en mensajes salientes de Discord.
- Menciona usuarios como `<@USER_ID>`.
- Prefiere componentes Discord v2 (`components`) para UI enriquecida; usa `embeds` (legado) solo cuando sea necesario.
- Multi-cuenta: usa `accountId` opcional.

## Destinos

- Acciones de tipo envío: `to: "channel:<id>"` o `to: "user:<id>"`.
- Acciones sobre mensajes: `channelId: "<id>"` + `messageId: "<id>"`.

## Acciones comunes

### Enviar un mensaje

```json
{
  "action": "send",
  "channel": "discord",
  "to": "channel:123",
  "message": "Hola desde Gaia Control",
  "silent": true
}
```

### Enviar con adjunto

```json
{
  "action": "send",
  "channel": "discord",
  "to": "channel:123",
  "message": "Ver adjunto",
  "media": "file:///tmp/ejemplo.png"
}
```

### Enviar con componentes v2 (recomendado para UI enriquecida)

```json
{
  "action": "send",
  "channel": "discord",
  "to": "channel:123",
  "message": "Actualización de estado",
  "components": "[componentes Carbon v2]"
}
```

> No combines `components` con `embeds` (Discord los rechaza juntos).

### Embeds legado (no recomendado)

```json
{
  "action": "send",
  "channel": "discord",
  "to": "channel:123",
  "message": "Actualización de estado",
  "embeds": [{"title": "Título", "description": "Descripción"}]
}
```

### Reaccionar a un mensaje

```json
{
  "action": "react",
  "channel": "discord",
  "channelId": "123",
  "messageId": "456",
  "emoji": "✅"
}
```

### Leer mensajes recientes

```json
{
  "action": "read",
  "channel": "discord",
  "channelId": "123",
  "limit": 20
}
```

### Editar un mensaje

```json
{
  "action": "edit",
  "channel": "discord",
  "channelId": "123",
  "messageId": "456",
  "message": "Mensaje actualizado"
}
```

### Eliminar un mensaje

```json
{
  "action": "delete",
  "channel": "discord",
  "channelId": "123",
  "messageId": "456"
}
```

## Ideas de uso

- Envía resúmenes automáticos de workflows al canal de notificaciones.
- Reacciona con ✅ o ❌ cuando un proceso termine (con éxito o con error).
- Envía alertas con embeds enriquecidos a canales de monitorización.
