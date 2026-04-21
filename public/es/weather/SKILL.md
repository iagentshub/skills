---
id: TIEMPO
name: Tiempo y Previsión Meteorológica
description: Obtener el tiempo actual y previsiones mediante wttr.in. Usar cuando el usuario pregunte por el tiempo, temperatura o previsiones para cualquier lugar. No requiere API key.
icon: ☔
category: data
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Tiempo y Previsión Meteorológica

Obtén las condiciones meteorológicas actuales y las previsiones usando `wttr.in` a través de `curl`. No se necesita API key.

## Cuándo usar

✅ **USA esta skill cuando:**

- "¿Qué tiempo hace?"
- "¿Va a llover hoy/mañana?"
- "Temperatura en [ciudad]"
- "Previsión del tiempo para la semana"
- Planificación de viajes

## Cuándo NO usar

❌ **NO uses esta skill cuando:**

- Datos meteorológicos históricos → usa archivos/APIs de clima
- Análisis climático o tendencias → usa fuentes de datos especializadas
- Alertas de tiempo severo → consulta fuentes oficiales (AEMET, etc.)
- Meteorología para aviación/náutica → usa servicios especializados (METAR, etc.)

## Requisitos

- `curl` en el PATH

## Comandos

### Tiempo actual

```bash
# Resumen en una línea
curl "wttr.in/Madrid?format=3"

# Condiciones actuales detalladas
curl "wttr.in/Madrid?0"

# Ciudad específica
curl "wttr.in/Barcelona?format=3"
```

### Previsiones

```bash
# Previsión 3 días
curl "wttr.in/Madrid"

# Formato extendido
curl "wttr.in/Madrid?format=v2"

# Día concreto (0=hoy, 1=mañana, 2=pasado)
curl "wttr.in/Madrid?1"
```

### Opciones de formato

```bash
# Una línea personalizada
curl "wttr.in/Madrid?format=%l:+%c+%t+%w"

# Salida JSON
curl "wttr.in/Madrid?format=j1"

# Imagen PNG
curl "wttr.in/Madrid.png"
```

### Códigos de formato

- `%c` — Emoji de condición
- `%t` — Temperatura
- `%f` — Sensación térmica
- `%w` — Viento
- `%h` — Humedad
- `%p` — Precipitación
- `%l` — Localización

## Ejemplos rápidos

**"¿Qué tiempo hace?"**

```bash
curl -s "wttr.in/Madrid?format=%l:+%c+%t+(sensación+%f),+viento+%w,+humedad+%h"
```

**"¿Va a llover?"**

```bash
curl -s "wttr.in/Madrid?format=%l:+%c+%p"
```

**Previsión de fin de semana**

```bash
curl -s "wttr.in/Madrid?2"
```

## Notas

- Siempre incluye una ciudad, región o código de aeropuerto en las consultas.
- Puedes usar nombres en español: `wttr.in/Sevilla`, `wttr.in/Bilbao`.
- Para idioma en la respuesta: `curl "wttr.in/Madrid?lang=es"`.
