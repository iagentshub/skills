---
id: TEXTO_A_VOZ
name: Texto a Voz con ElevenLabs (sag)
description: Convertir texto a voz con ElevenLabs usando el CLI sag, con UX similar al comando say de macOS. Múltiples voces, modelos y tags de audio expresivo. Requiere ELEVENLABS_API_KEY.
icon: 🔊
category: media
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Texto a Voz con ElevenLabs (sag)

Usa `sag` para síntesis de voz con ElevenLabs y reproducción local.

## Requisitos

- `sag` instalado
- `ELEVENLABS_API_KEY` o `SAG_API_KEY` configurado

## Instalación

```bash
brew install steipete/tap/sag
```

## Uso básico

```bash
# Texto simple (voz por defecto)
sag "Hola, esto es una prueba"

# Con voz específica
sag speak -v "Roger" "Hola, soy Roger"

# Listar voces disponibles
sag voices

# Ver consejos de pronunciación por modelo
sag prompting
```

## Modelos disponibles

| Modelo | Descripción |
|--------|-------------|
| `eleven_v3` | Expresivo (por defecto) |
| `eleven_multilingual_v2` | Estable y multilingüe |
| `eleven_flash_v2_5` | Rápido y económico |

```bash
# Especificar modelo
sag speak --model eleven_multilingual_v2 "Texto en español"
```

## Tags de audio expresivo (solo v3)

Colócalos al inicio de una línea:

```bash
sag "[susurra] esto es un secreto. [pausa corta] ¿de acuerdo?"
sag "[emocionado] ¡Ganamos el campeonato!"
sag "[sarcástico] claro, qué buena idea..."
```

Tags disponibles: `[susurra]`, `[grita]`, `[canta]`, `[ríe]`, `[empieza a reír]`, `[suspira]`, `[exhala]`, `[sarcástico]`, `[curioso]`, `[emocionado]`, `[llora]`, `[con malicia]`

Pausas (v3): `[pausa]`, `[pausa corta]`, `[pausa larga]`
Pausas (v2/v2.5): SSML `<break time="1.5s" />`

## Pronunciación y ajustes

```bash
# Normalización automática de números/unidades
sag --normalize auto "Son 3,5 km o 3500 metros"

# Fijar idioma para normalización
sag --lang es "Son €1.500 el metro cuadrado"
```

## Guardar en fichero

```bash
sag -o /tmp/respuesta.mp3 "Texto que quiero guardar"
sag speak -v "Rachel" -o /tmp/audio.mp3 "Hola desde ElevenLabs"
```

## Variables de entorno

```bash
export ELEVENLABS_API_KEY="tu-api-key"
export ELEVENLABS_VOICE_ID="id-voz-preferida"  # opcional
```

## Notas

- Confirma siempre la voz y el modelo antes de generar audios largos.
- No imprimas ni compartas el valor de `ELEVENLABS_API_KEY` en logs del agente.
- `sag voices` muestra los IDs de las voces disponibles en tu cuenta.
