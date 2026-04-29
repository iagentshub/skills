<div align="center">
  <a href="es/index.md">← Índice</a> &nbsp;·&nbsp;
  <a href="en.md">🇬🇧 Read in English</a>
</div>

<br>

<h1 align="center">Catálogo de Skills — Español</h1>

<p align="center">
  <img src="https://img.shields.io/badge/skills-33-6366f1?style=flat-square" alt="Skills">
  <img src="https://img.shields.io/badge/idioma-Español-DC2626?style=flat-square" alt="Idioma">
</p>

---

## Categorías

![ai](https://img.shields.io/badge/ai-7C3AED?style=for-the-badge&logoColor=white)
![data](https://img.shields.io/badge/data-2563EB?style=for-the-badge&logoColor=white)
![dev](https://img.shields.io/badge/dev-16A34A?style=for-the-badge&logoColor=white)
![media](https://img.shields.io/badge/media-EA580C?style=for-the-badge&logoColor=white)
![messaging](https://img.shields.io/badge/messaging-0891B2?style=for-the-badge&logoColor=white)
![notes](https://img.shields.io/badge/notes-CA8A04?style=for-the-badge&logoColor=white)
![productivity](https://img.shields.io/badge/productivity-DC2626?style=for-the-badge&logoColor=white)
![security](https://img.shields.io/badge/security-BE123C?style=for-the-badge&logoColor=white)

---

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

| Skill | Categoría | Descripción |
|-------|-----------|-------------|
| [1password](../public/es/1password/SKILL.md) | ![productivity](https://img.shields.io/badge/productivity-DC2626?style=flat-square&logoColor=white) | Configura y usa el CLI de 1Password (op) para instalar, iniciar sesión y leer/inyectar secretos. |
| [apple-notes](../public/es/apple-notes/SKILL.md) | ![notes](https://img.shields.io/badge/notes-CA8A04?style=flat-square&logoColor=white) | Gestiona notas de Apple Notes mediante el CLI memo en macOS. |
| [apple-reminders](../public/es/apple-reminders/SKILL.md) | ![productivity](https://img.shields.io/badge/productivity-DC2626?style=flat-square&logoColor=white) | Gestionar recordatorios de Apple Reminders desde el terminal con remindctl. |
| [bear-notes](../public/es/bear-notes/SKILL.md) | ![notes](https://img.shields.io/badge/notes-CA8A04?style=flat-square&logoColor=white) | Crear, buscar y gestionar notas en Bear en macOS mediante el CLI grizzly. |
| [blogwatcher](../public/es/blogwatcher/SKILL.md) | ![productivity](https://img.shields.io/badge/productivity-DC2626?style=flat-square&logoColor=white) | Monitorizar blogs y feeds RSS/Atom para detectar nuevas publicaciones usando el CLI blogwatcher. |
| [camsnap](../public/es/camsnap/SKILL.md) | ![media](https://img.shields.io/badge/media-EA580C?style=flat-square&logoColor=white) | Capturar fotogramas y clips de cámaras IP RTSP/ONVIF con el CLI camsnap. |
| [coding-agent](../public/es/coding-agent/SKILL.md) | ![ai](https://img.shields.io/badge/ai-7C3AED?style=flat-square&logoColor=white) | Delega tareas de código a Claude Code o Codex vía proceso en segundo plano. |
| [discord](../public/es/discord/SKILL.md) | ![messaging](https://img.shields.io/badge/messaging-0891B2?style=flat-square&logoColor=white) | Operaciones de Discord: enviar mensajes, reaccionar, leer canales, gestionar roles y moderar. |
| [gemini](../public/es/gemini/SKILL.md) | ![ai](https://img.shields.io/badge/ai-7C3AED?style=flat-square&logoColor=white) | CLI de Gemini para consultas, resúmenes y generación de contenido en modo one-shot. |
| [gh-issues](../public/es/gh-issues/SKILL.md) | ![dev](https://img.shields.io/badge/dev-16A34A?style=flat-square&logoColor=white) | Obtiene issues de GitHub, lanza sub-agentes para implementar fixes y abrir PRs. |
| [github](../public/es/github/SKILL.md) | ![dev](https://img.shields.io/badge/dev-16A34A?style=flat-square&logoColor=white) | Operaciones en GitHub con el CLI gh: issues, PRs, CI, revisiones de código. |
| [gog](../public/es/gog/SKILL.md) | ![productivity](https://img.shields.io/badge/productivity-DC2626?style=flat-square&logoColor=white) | CLI para Gmail, Calendar, Drive, Contactos, Sheets y Docs de Google. |
| [goplaces](../public/es/goplaces/SKILL.md) | ![data](https://img.shields.io/badge/data-2563EB?style=flat-square&logoColor=white) | Consultar la API de Google Places mediante el CLI goplaces para buscar lugares y reseñas. |
| [healthcheck](../public/es/healthcheck/SKILL.md) | ![security](https://img.shields.io/badge/security-BE123C?style=flat-square&logoColor=white) | Auditoría y endurecimiento de seguridad del host (firewall, SSH, actualizaciones, exposición de red). |
| [himalaya](../public/es/himalaya/SKILL.md) | ![messaging](https://img.shields.io/badge/messaging-0891B2?style=flat-square&logoColor=white) | Gestionar correos electrónicos vía IMAP/SMTP con el CLI himalaya. |
| [imsg](../public/es/imsg/SKILL.md) | ![messaging](https://img.shields.io/badge/messaging-0891B2?style=flat-square&logoColor=white) | Leer y enviar mensajes de iMessage/SMS en macOS usando el CLI imsg. |
| [nano-pdf](../public/es/nano-pdf/SKILL.md) | ![productivity](https://img.shields.io/badge/productivity-DC2626?style=flat-square&logoColor=white) | Editar PDFs con instrucciones en lenguaje natural usando el CLI nano-pdf. |
| [notion](../public/es/notion/SKILL.md) | ![notes](https://img.shields.io/badge/notes-CA8A04?style=flat-square&logoColor=white) | API de Notion para crear y gestionar páginas, bases de datos y bloques. |
| [obsidian](../public/es/obsidian/SKILL.md) | ![notes](https://img.shields.io/badge/notes-CA8A04?style=flat-square&logoColor=white) | Trabaja con vaults de Obsidian y automatiza operaciones con obsidian-cli. |
| [openai-whisper](../public/es/openai-whisper/SKILL.md) | ![ai](https://img.shields.io/badge/ai-7C3AED?style=flat-square&logoColor=white) | Transcribir audio localmente con el CLI de Whisper, sin API key. |
| [openai-whisper-api](../public/es/openai-whisper-api/SKILL.md) | ![ai](https://img.shields.io/badge/ai-7C3AED?style=flat-square&logoColor=white) | Transcribir audio vía la API de OpenAI Audio Transcriptions (Whisper). Requiere OPENAI_API_KEY. |
| [oracle](../public/es/oracle/SKILL.md) | ![ai](https://img.shields.io/badge/ai-7C3AED?style=flat-square&logoColor=white) | Bundlear un prompt con ficheros para hacer consultas one-shot a un LLM con contexto real del repo. |
| [peekaboo](../public/es/peekaboo/SKILL.md) | ![dev](https://img.shields.io/badge/dev-16A34A?style=flat-square&logoColor=white) | Capturar pantalla y automatizar la interfaz de usuario de macOS con el CLI Peekaboo. |
| [sag](../public/es/sag/SKILL.md) | ![ai](https://img.shields.io/badge/ai-7C3AED?style=flat-square&logoColor=white) | Convertir texto a voz con ElevenLabs usando el CLI sag. Requiere ELEVENLABS_API_KEY. |
| [slack](../public/es/slack/SKILL.md) | ![messaging](https://img.shields.io/badge/messaging-0891B2?style=flat-square&logoColor=white) | Operaciones de Slack: enviar/editar/eliminar mensajes, reaccionar, fijar mensajes y leer conversaciones. |
| [summarize](../public/es/summarize/SKILL.md) | ![ai](https://img.shields.io/badge/ai-7C3AED?style=flat-square&logoColor=white) | Resumir o extraer texto y transcripciones de URLs, podcasts, YouTube y ficheros locales. |
| [things-mac](../public/es/things-mac/SKILL.md) | ![productivity](https://img.shields.io/badge/productivity-DC2626?style=flat-square&logoColor=white) | Gestionar Things 3 en macOS mediante el CLI things. |
| [tmux](../public/es/tmux/SKILL.md) | ![dev](https://img.shields.io/badge/dev-16A34A?style=flat-square&logoColor=white) | Controlar sesiones tmux enviando teclas y leyendo la salida de los paneles. |
| [trello](../public/es/trello/SKILL.md) | ![productivity](https://img.shields.io/badge/productivity-DC2626?style=flat-square&logoColor=white) | Gestión de tableros, listas y tarjetas de Trello mediante la API REST. |
| [video-frames](../public/es/video-frames/SKILL.md) | ![media](https://img.shields.io/badge/media-EA580C?style=flat-square&logoColor=white) | Extraer fotogramas o clips cortos de videos usando ffmpeg. |
| [wacli](../public/es/wacli/SKILL.md) | ![messaging](https://img.shields.io/badge/messaging-0891B2?style=flat-square&logoColor=white) | Enviar mensajes de WhatsApp y buscar/sincronizar historial usando el CLI wacli. |
| [weather](../public/es/weather/SKILL.md) | ![data](https://img.shields.io/badge/data-2563EB?style=flat-square&logoColor=white) | Obtener el tiempo actual y previsiones mediante wttr.in. No requiere API key. |
| [xurl](../public/es/xurl/SKILL.md) | ![messaging](https://img.shields.io/badge/messaging-0891B2?style=flat-square&logoColor=white) | CLI para la API de X (Twitter) v2: publicar, responder, buscar, DMs y subir media. |
