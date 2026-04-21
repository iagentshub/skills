---
id: TWITTER_X
name: Twitter / X API
description: CLI para interactuar con la API de X (Twitter) v2. Publicar tweets, responder, citar, buscar, gestionar seguidores, enviar DMs y subir media usando xurl.
icon: 🐦
category: messaging
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Twitter / X API (xurl)

`xurl` es un CLI para la API de X. Soporta comandos de acceso directo (shortcuts) amigables para agentes y acceso directo a cualquier endpoint v2. Todas las respuestas son JSON.

## Requisitos

- `xurl` instalado
- Autenticación OAuth configurada (`xurl auth status` para verificar)

## Instalación

```bash
brew install --cask xdevplatform/tap/xurl
# o
npm install -g @xdevplatform/xurl
```

## Seguridad (obligatorio)

- **Nunca** leer, imprimir ni enviar el fichero `~/.xurl` al contexto del agente.
- **Nunca** pedir al usuario que pegue credenciales/tokens en el chat.
- **Nunca** usar `--verbose`/`-v` en sesiones de agente (puede exponer tokens en la salida).
- Flags que **nunca** deben usarse en comandos del agente: `--bearer-token`, `--consumer-key`, `--consumer-secret`, `--access-token`, `--token-secret`, `--client-id`, `--client-secret`.
- El registro de apps y la autenticación inicial debe hacerlos el usuario manualmente, fuera de la sesión del agente.

## Autenticación

```bash
# Verificar si está autenticado
xurl auth status

# Autenticar (el usuario lo hace manualmente)
xurl auth oauth2
```

## Referencia rápida

| Acción | Comando |
|--------|---------|
| Publicar tweet | `xurl post "Hola mundo!"` |
| Responder | `xurl reply POST_ID "Respuesta"` |
| Citar | `xurl quote POST_ID "Mi opinión"` |
| Eliminar tweet | `xurl delete POST_ID` |
| Leer tweet | `xurl read POST_ID` |
| Buscar tweets | `xurl search "CONSULTA" -n 10` |
| ¿Quién soy? | `xurl whoami` |
| Ver usuario | `xurl user @nombre` |
| Timeline propio | `xurl timeline -n 20` |
| Menciones | `xurl mentions -n 10` |
| Dar like | `xurl like POST_ID` |
| Quitar like | `xurl unlike POST_ID` |
| Repostear | `xurl repost POST_ID` |
| Guardar | `xurl bookmark POST_ID` |
| Ver guardados | `xurl bookmarks -n 10` |
| Seguir | `xurl follow @nombre` |
| Dejar de seguir | `xurl unfollow @nombre` |
| Ver seguidores | `xurl followers -n 20` |
| Ver seguidos | `xurl following -n 20` |
| Bloquear | `xurl block @nombre` |
| Silenciar | `xurl mute @nombre` |
| Enviar DM | `xurl dm @nombre "mensaje"` |
| Ver DMs | `xurl dms -n 10` |
| Subir media | `xurl media upload /ruta/fichero.mp4` |
| Estado de media | `xurl media status MEDIA_ID` |

## Acceso raw a la API

```bash
# Cualquier endpoint v2
xurl GET /2/users/me
xurl GET /2/tweets?ids=1234567890
```

## Múltiples apps

```bash
# Ver apps registradas
xurl auth apps list

# Establecer app por defecto
xurl auth default mi-app

# Usar app específica para un comando
xurl --app mi-app /2/users/me
```

## Notas

- Todos los comandos devuelven JSON a stdout.
- Usar `-n` para limitar resultados y evitar requests innecesarios.
- Confirma siempre antes de publicar tweets o enviar DMs.
