---
id: FOTOGRAMAS_VIDEO
name: Extracción de Fotogramas de Video
description: Extraer fotogramas o clips cortos de videos usando ffmpeg. Útil para obtener capturas en timestamps específicos o crear thumbnails para inspección visual.
icon: 🎬
category: media
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Extracción de Fotogramas de Video (ffmpeg)

Usa `ffmpeg` para extraer un fotograma específico o crear thumbnails de un video para inspección visual.

## Requisitos

- `ffmpeg` instalado en el PATH

## Instalación

```bash
brew install ffmpeg
```

## Comandos

### Extraer un fotograma

```bash
# Primer fotograma
ffmpeg -i video.mp4 -frames:v 1 fotograma.jpg

# En un timestamp específico (HH:MM:SS)
ffmpeg -i video.mp4 -ss 00:00:10 -frames:v 1 fotograma_10s.jpg

# Con alta calidad
ffmpeg -i video.mp4 -ss 00:01:30 -frames:v 1 -q:v 2 fotograma.jpg

# Como PNG (sin pérdida)
ffmpeg -i video.mp4 -ss 00:00:05 -frames:v 1 fotograma.png
```

### Crear thumbnails a intervalos

```bash
# Un thumbnail cada 30 segundos
ffmpeg -i video.mp4 -vf fps=1/30 /tmp/thumb%03d.jpg

# Un thumbnail cada 10 segundos
ffmpeg -i video.mp4 -vf fps=1/10 /tmp/thumb%03d.jpg

# Un thumbnail por segundo
ffmpeg -i video.mp4 -vf fps=1 /tmp/thumb%03d.jpg
```

### Extraer clip corto

```bash
# Clip de 5 segundos desde el segundo 30
ffmpeg -i video.mp4 -ss 00:00:30 -t 5 -c copy clip.mp4

# Clip de 10 segundos desde el minuto 2
ffmpeg -i video.mp4 -ss 00:02:00 -t 10 -c copy clip.mp4
```

### Obtener información del video

```bash
ffmpeg -i video.mp4 2>&1 | grep -E "Duration|Stream"
```

## Notas

- Usa `-ss` antes de `-i` para búsqueda rápida (menos precisa) o después de `-i` para búsqueda cuadro a cuadro (más lenta pero exacta).
- `.jpg` para compartir rápido; `.png` para capturas de UI con texto (sin artefactos).
- Para videos muy largos, usa búsqueda rápida (`-ss` antes de `-i`) para mayor velocidad.
- `-q:v 2` da la máxima calidad JPEG (rango 1-31, menor = mejor).
