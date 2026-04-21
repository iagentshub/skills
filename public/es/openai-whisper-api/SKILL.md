---
id: TRANSCRIPCION_API
name: Transcripción de Audio vía API (Whisper)
description: Transcribir audio a través de la API de OpenAI Audio Transcriptions (Whisper). Requiere OPENAI_API_KEY. Ideal cuando no se quiere instalar dependencias locales.
icon: 🌐
category: media
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Transcripción de Audio vía API (Whisper)

Transcribe archivos de audio usando el endpoint `/v1/audio/transcriptions` de OpenAI (modelo `whisper-1`).

## Requisitos

- `curl` en el PATH
- Variable de entorno `OPENAI_API_KEY` configurada

## Uso básico

```bash
curl -X POST https://api.openai.com/v1/audio/transcriptions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F file=@/ruta/audio.m4a \
  -F model=whisper-1
```

## Con parámetros adicionales

```bash
# Especificar idioma (mejora precisión)
curl -X POST https://api.openai.com/v1/audio/transcriptions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F file=@audio.mp3 \
  -F model=whisper-1 \
  -F language=es

# Con prompt para guiar la transcripción
curl -X POST https://api.openai.com/v1/audio/transcriptions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F file=@audio.mp3 \
  -F model=whisper-1 \
  -F prompt="Reunión de ventas con Pedro García y Ana López"

# Respuesta en JSON con timestamps
curl -X POST https://api.openai.com/v1/audio/transcriptions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F file=@audio.mp3 \
  -F model=whisper-1 \
  -F response_format=verbose_json
```

## Guardar resultado en fichero

```bash
curl -X POST https://api.openai.com/v1/audio/transcriptions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F file=@reunión.m4a \
  -F model=whisper-1 \
  -o transcripcion.txt
```

## Formatos de respuesta

- `json` (por defecto): `{"text": "transcripción aquí"}`
- `text`: texto plano
- `srt`: subtítulos SRT
- `vtt`: subtítulos VTT
- `verbose_json`: JSON con segmentos y timestamps

## Formatos de audio soportados

mp3, mp4, mpeg, mpga, m4a, wav, webm (máx. 25 MB por fichero).

## Notas

- Si el fichero supera 25 MB, divídelo con `ffmpeg` antes de enviarlo.
- La variable `OPENAI_BASE_URL` permite apuntar a un proxy o gateway local compatible con la API de OpenAI.
- No compartas ni imprimas el valor de `OPENAI_API_KEY` en logs o salida del agente.
