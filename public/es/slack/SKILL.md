---
id: SLACK
name: Slack
description: "Operaciones de Slack: enviar/editar/eliminar mensajes, reaccionar, fijar mensajes, leer conversaciones y obtener info de miembros. Requiere un bot token de Slack configurado como canal."
icon: 💬
category: messaging
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Slack

Usa las acciones de Slack para reaccionar, gestionar mensajes fijados, enviar/editar/eliminar mensajes y obtener información de miembros.

## Datos a recopilar

- `channelId` y `messageId` (timestamp del mensaje de Slack, ej. `1712023032.1234`).
- Para reacciones, un `emoji` (Unicode o `:nombre:`).
- Para envíos de mensajes, un destino `to` (`channel:<id>` o `user:<id>`) y `content`.

## Grupos de acciones

| Grupo | Por defecto | Notas |
|-------|-------------|-------|
| reactions | activo | Reaccionar + listar reacciones |
| messages | activo | Leer/enviar/editar/eliminar |
| pins | activo | Fijar/desfijar/listar |
| memberInfo | activo | Info de miembro |
| emojiList | activo | Lista de emojis personalizados |

## Acciones comunes

### Reaccionar a un mensaje

```json
{
  "action": "react",
  "channelId": "C123",
  "messageId": "1712023032.1234",
  "emoji": "✅"
}
```

### Listar reacciones

```json
{
  "action": "reactions",
  "channelId": "C123",
  "messageId": "1712023032.1234"
}
```

### Enviar un mensaje

```json
{
  "action": "sendMessage",
  "to": "channel:C123",
  "content": "Hola desde Gaia Control"
}
```

### Editar un mensaje

```json
{
  "action": "editMessage",
  "channelId": "C123",
  "messageId": "1712023032.1234",
  "content": "Texto actualizado"
}
```

### Eliminar un mensaje

```json
{
  "action": "deleteMessage",
  "channelId": "C123",
  "messageId": "1712023032.1234"
}
```

### Leer mensajes recientes

```json
{
  "action": "readMessages",
  "channelId": "C123",
  "limit": 20
}
```

### Fijar un mensaje

```json
{
  "action": "pinMessage",
  "channelId": "C123",
  "messageId": "1712023032.1234"
}
```

### Desfijar un mensaje

```json
{
  "action": "unpinMessage",
  "channelId": "C123",
  "messageId": "1712023032.1234"
}
```

### Listar mensajes fijados

```json
{
  "action": "listPins",
  "channelId": "C123"
}
```

### Info de un miembro

```json
{
  "action": "memberInfo",
  "userId": "U123"
}
```

### Lista de emojis personalizados

```json
{
  "action": "emojiList"
}
```

## Ideas de uso

- Reacciona con ✅ para marcar tareas completadas en un canal de tareas.
- Fija decisiones clave o actualizaciones semanales de estado.
- Envía resúmenes automáticos de workflows al completarse.
