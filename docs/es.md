# Catálogo de Skills — Español

> 33 skills disponibles · [English](en.md)

## Cómo contribuir

- Cada carpeta de skill debe contener un `SKILL.md` con al menos `name` y `description` en el frontmatter.
- El slug de la skill (nombre de la carpeta) debe ser **idéntico en todos los idiomas** — usa el slug canónico en inglés.
- Añadir una skill en un idioma sin añadirla en los demás fallará el check de paridad en CI.
- Nuevos idiomas: crea una carpeta en `public/` (p.ej. `public/fr/`) y traduce todas las skills existentes.

---

## Formato de una skill

Cada `SKILL.md` debe incluir un bloque de frontmatter YAML al inicio:

````yaml
---
name: mi-skill
description: Descripción en una línea de lo que hace esta skill.
icon: 🔧
category: dev
---

Aquí van las instrucciones de la skill...
````

Campos obligatorios: `name`, `description`. Opcionales: `icon`, `category`, `homepage`.

---

## Skills

| Skill | Descripción |
|-------|-------------|
| [1password](../public/es/1password/SKILL.md) | Configura y usa el CLI de 1Password (op) para instalar, iniciar sesión y leer/inyectar secretos. |
| [apple-notes](../public/es/apple-notes/SKILL.md) | Gestiona notas de Apple Notes mediante el CLI memo en macOS. |
| [apple-reminders](../public/es/apple-reminders/SKILL.md) | Gestionar recordatorios de Apple Reminders desde el terminal con remindctl. |
| [bear-notes](../public/es/bear-notes/SKILL.md) | Crear, buscar y gestionar notas en Bear en macOS mediante el CLI grizzly. |
| [blogwatcher](../public/es/blogwatcher/SKILL.md) | Monitorizar blogs y feeds RSS/Atom para detectar nuevas publicaciones usando el CLI blogwatcher. |
| [camsnap](../public/es/camsnap/SKILL.md) | Capturar fotogramas y clips de cámaras IP RTSP/ONVIF con el CLI camsnap. |
| [coding-agent](../public/es/coding-agent/SKILL.md) | Delega tareas de código a Claude Code o Codex vía proceso en segundo plano. |
| [discord](../public/es/discord/SKILL.md) | Operaciones de Discord: enviar mensajes, reaccionar, leer canales, gestionar roles y moderar. |
| [gemini](../public/es/gemini/SKILL.md) | CLI de Gemini para consultas, resúmenes y generación de contenido en modo one-shot. |
| [gh-issues](../public/es/gh-issues/SKILL.md) | Obtiene issues de GitHub, lanza sub-agentes para implementar fixes y abrir PRs. |
| [github](../public/es/github/SKILL.md) | Operaciones en GitHub con el CLI gh: issues, PRs, CI, revisiones de código. |
| [gog](../public/es/gog/SKILL.md) | CLI para Gmail, Calendar, Drive, Contactos, Sheets y Docs de Google. |
| [goplaces](../public/es/goplaces/SKILL.md) | Consultar la API de Google Places mediante el CLI goplaces para buscar lugares y reseñas. |
| [healthcheck](../public/es/healthcheck/SKILL.md) | Auditoría y endurecimiento de seguridad del host (firewall, SSH, actualizaciones, exposición de red). |
| [himalaya](../public/es/himalaya/SKILL.md) | Gestionar correos electrónicos vía IMAP/SMTP con el CLI himalaya. |
| [imsg](../public/es/imsg/SKILL.md) | Leer y enviar mensajes de iMessage/SMS en macOS usando el CLI imsg. |
| [nano-pdf](../public/es/nano-pdf/SKILL.md) | Editar PDFs con instrucciones en lenguaje natural usando el CLI nano-pdf. |
| [notion](../public/es/notion/SKILL.md) | API de Notion para crear y gestionar páginas, bases de datos y bloques. |
| [obsidian](../public/es/obsidian/SKILL.md) | Trabaja con vaults de Obsidian y automatiza operaciones con obsidian-cli. |
| [openai-whisper](../public/es/openai-whisper/SKILL.md) | Transcribir audio localmente con el CLI de Whisper, sin API key. |
| [openai-whisper-api](../public/es/openai-whisper-api/SKILL.md) | Transcribir audio vía la API de OpenAI Audio Transcriptions (Whisper). Requiere OPENAI_API_KEY. |
| [oracle](../public/es/oracle/SKILL.md) | Bundlear un prompt con ficheros para hacer consultas one-shot a un LLM con contexto real del repo. |
| [peekaboo](../public/es/peekaboo/SKILL.md) | Capturar pantalla y automatizar la interfaz de usuario de macOS con el CLI Peekaboo. |
| [sag](../public/es/sag/SKILL.md) | Convertir texto a voz con ElevenLabs usando el CLI sag. Requiere ELEVENLABS_API_KEY. |
| [slack](../public/es/slack/SKILL.md) | Operaciones de Slack: enviar/editar/eliminar mensajes, reaccionar, fijar mensajes y leer conversaciones. |
| [summarize](../public/es/summarize/SKILL.md) | Resumir o extraer texto y transcripciones de URLs, podcasts, YouTube y ficheros locales. |
| [things-mac](../public/es/things-mac/SKILL.md) | Gestionar Things 3 en macOS mediante el CLI things. |
| [tmux](../public/es/tmux/SKILL.md) | Controlar sesiones tmux enviando teclas y leyendo la salida de los paneles. |
| [trello](../public/es/trello/SKILL.md) | Gestión de tableros, listas y tarjetas de Trello mediante la API REST. |
| [video-frames](../public/es/video-frames/SKILL.md) | Extraer fotogramas o clips cortos de videos usando ffmpeg. |
| [wacli](../public/es/wacli/SKILL.md) | Enviar mensajes de WhatsApp y buscar/sincronizar historial usando el CLI wacli. |
| [weather](../public/es/weather/SKILL.md) | Obtener el tiempo actual y previsiones mediante wttr.in. No requiere API key. |
| [xurl](../public/es/xurl/SKILL.md) | CLI para la API de X (Twitter) v2: publicar, responder, buscar, DMs y subir media. |
