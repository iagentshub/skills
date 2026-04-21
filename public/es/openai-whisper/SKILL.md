---
id: TRANSCRIPCION_LOCAL
name: Transcripción de Audio Local (Whisper)
description: Transcribir audio localmente con el CLI de Whisper (sin API key). Soporta múltiples formatos de audio y modelos de distinto tamaño para equilibrar velocidad y precisión.
icon: 🎤
category: media
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Transcripción de Audio Local (Whisper)

Usa `whisper` para transcribir audio **localmente** sin necesidad de API key ni conexión a internet (después de la primera descarga del modelo).

## Requisitos

- `whisper` instalado (`pip install openai-whisper` o `brew install openai-whisper`)
- `ffmpeg` en el PATH para convertir formatos de audio

## Instalación

```bash
brew install openai-whisper
# o
pip install openai-whisper
```

## Uso básico

```bash
# Transcripción simple a texto
whisper /ruta/audio.mp3 --model medium --output_format txt --output_dir .

# Traducir a inglés
whisper /ruta/audio.m4a --task translate --output_format srt

# Especificar idioma para mejor precisión
whisper /ruta/audio.mp3 --language es --model medium
```

## Modelos disponibles

| Modelo    | Tamaño | Velocidad | Precisión |
|-----------|--------|-----------|-----------|
| `tiny`    | 75 MB  | Muy rápido | Básica |
| `base`    | 142 MB | Rápido | Aceptable |
| `small`   | 466 MB | Normal | Buena |
| `medium`  | 1.5 GB | Lento | Muy buena |
| `large`   | 2.9 GB | Muy lento | Excelente |
| `turbo`   | ~800 MB | Rápido | Muy buena (por defecto) |

## Formatos de salida

```bash
# Texto plano (.txt)
whisper audio.mp3 --output_format txt

# Subtítulos SRT
whisper audio.mp3 --output_format srt

# Subtítulos VTT
whisper audio.mp3 --output_format vtt

# JSON completo con timestamps
whisper audio.mp3 --output_format json

# Todos los formatos a la vez
whisper audio.mp3 --output_format all
```

## Notas

- Los modelos se descargan automáticamente en `~/.cache/whisper` la primera vez.
- El modelo por defecto es `turbo`; usa `--model medium` para mejor equilibrio.
- Para audio largo (> 30 min), usa `--model large` para mayor precisión.
- Formatos soportados: mp3, mp4, m4a, wav, ogg, flac, webm y más (vía ffmpeg).
