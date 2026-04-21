---
id: GOOGLE_PLACES
name: Google Places API
description: Consultar la API de Google Places (New) mediante el CLI goplaces para buscar lugares, obtener detalles, resolver nombres y leer reseñas. Requiere GOOGLE_PLACES_API_KEY.
icon: 📍
category: data
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Google Places API (goplaces)

CLI moderno para la API de Google Places. Salida amigable por defecto, `--json` para scripts.

## Requisitos

- `goplaces` instalado
- Variable de entorno `GOOGLE_PLACES_API_KEY` configurada

## Instalación

```bash
brew install steipete/tap/goplaces
```

## Configuración

```bash
export GOOGLE_PLACES_API_KEY="tu-api-key-aqui"
# Opcional: proxy/testing
export GOOGLE_PLACES_BASE_URL="https://tu-proxy/maps"
```

## Comandos

### Buscar lugares

```bash
# Búsqueda básica
goplaces search "café Madrid"

# Con filtros
goplaces search "restaurante italiano" --open-now --min-rating 4 --limit 5

# Con ubicación y radio
goplaces search "pizza" --lat 40.416 --lng -3.703 --radius-m 2000

# Por tipo
goplaces search "hospital" --type hospital

# Paginación
goplaces search "bar" --page-token "SIGUIENTE_PAGINA_TOKEN"
```

### Resolver nombre/dirección a place_id

```bash
goplaces resolve "Gran Vía, Madrid" --limit 5
goplaces resolve "Torre Picasso" --limit 3
```

### Detalles de un lugar

```bash
goplaces details <place_id>
goplaces details <place_id> --reviews
```

### Salida JSON para scripts

```bash
goplaces search "sushi" --json
goplaces details <place_id> --json
```

## Notas de uso

- `--no-color` o `NO_COLOR=1` desactiva el color ANSI.
- Los niveles de precio van de 0 (gratis) a 4 (muy caro).
- El filtro `--type` solo acepta un tipo (limitación de la API de Google).
- Los `place_id` son estables y puedes usarlos para obtener detalles en el futuro.
- No imprimas ni compartas el valor de `GOOGLE_PLACES_API_KEY` en logs del agente.
