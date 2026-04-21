---
id: CAMARAS_RTSP
name: Cámaras RTSP/ONVIF (camsnap)
description: Capturar fotogramas y clips de cámaras IP RTSP/ONVIF con el CLI camsnap. Permite añadir cámaras, hacer snapshots, grabar clips y detectar movimiento.
icon: 📸
category: media
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Cámaras RTSP/ONVIF (camsnap)

Usa `camsnap` para obtener capturas, clips o eventos de movimiento de cámaras IP configuradas.

## Requisitos

- `camsnap` instalado
- `ffmpeg` en el PATH
- Cámaras IP con soporte RTSP o ONVIF en la red local

## Instalación

```bash
brew install steipete/tap/camsnap
```

## Configuración

El fichero de configuración se encuentra en `~/.config/camsnap/config.yaml`.

### Añadir una cámara

```bash
camsnap add --name cocina --host 192.168.1.10 --user usuario --pass contraseña
camsnap add --name entrada --host 192.168.1.11 --user admin --pass admin123
```

## Comandos principales

```bash
# Descubrir cámaras en la red local
camsnap discover --info

# Captura instantánea (snapshot)
camsnap snap cocina --out /tmp/cocina.jpg

# Grabar clip de duración determinada
camsnap clip cocina --dur 5s --out /tmp/clip.mp4
camsnap clip entrada --dur 10s --out /tmp/entrada.mp4

# Monitorización de movimiento
camsnap watch cocina --threshold 0.2 --action 'echo "Movimiento detectado"'

# Diagnóstico de conectividad
camsnap doctor --probe
```

## Ejemplo de flujo

```bash
# 1. Hacer una prueba rápida antes de grabar clips largos
camsnap snap cocina --out /tmp/test.jpg

# 2. Si la prueba es correcta, grabar clip
camsnap clip cocina --dur 30s --out /tmp/grabacion.mp4
```

## Notas

- Haz siempre una captura de prueba antes de clips más largos.
- El umbral de movimiento (`--threshold`) es una fracción (0.0–1.0); 0.2 detecta cambios del 20% en la imagen.
- Las credenciales de las cámaras se almacenan en el fichero de configuración; no las imprimas en logs.
- Requiere `ffmpeg` para la captura de video.
