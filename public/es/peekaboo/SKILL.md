---
id: AUTOMATIZACION_UI
name: Automatización de UI en macOS (Peekaboo)
description: Capturar pantalla y automatizar la interfaz de usuario de macOS con el CLI Peekaboo. Permite hacer clic, escribir, navegar menús y controlar apps. Solo para macOS.
icon: 👀
category: dev
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Automatización de UI en macOS (Peekaboo)

> ⚠️ **Solo macOS.** Requiere permisos de Grabación de Pantalla y Accesibilidad.

Peekaboo es un CLI de automatización completa de UI para macOS: captura/inspecciona pantallas, dirige elementos de UI, controla el input y gestiona apps, ventanas y menús.

## Requisitos

- macOS
- `peekaboo` instalado (`brew install steipete/tap/peekaboo`)
- Permisos de **Grabación de Pantalla** y **Accesibilidad** concedidos a la app desde la que se ejecuta

## Instalación

```bash
brew install steipete/tap/peekaboo
```

## Verificar permisos

```bash
peekaboo permissions
```

## Comandos principales

### Captura e inspección

```bash
# Ver apps disponibles
peekaboo list apps --json

# Capturar pantalla y anotar elementos UI
peekaboo see --annotate --path /tmp/pantalla.png

# Capturar ventana específica
peekaboo image --mode window --app Safari --path /tmp/safari.png
```

### Interacción

```bash
# Hacer clic en elemento (usa ID del snapshot de `see`)
peekaboo click --on B1

# Escribir texto
peekaboo type "Hola mundo" --return

# Pegar texto desde portapapeles
peekaboo paste "Texto a pegar"

# Tecla de acceso rápido
peekaboo hotkey cmd,shift,t

# Scroll
peekaboo scroll --direction down --amount 3
```

### Gestión de apps

```bash
# Lanzar app
peekaboo app --action launch --app "Safari"

# Cambiar a app
peekaboo app --action switch --app "Terminal"

# Listar apps abiertas
peekaboo list apps
```

### Gestión de ventanas

```bash
# Listar ventanas
peekaboo list windows

# Enfocar ventana
peekaboo window --action focus --window-title "GitHub"

# Maximizar ventana
peekaboo window --action maximize --app "Finder"
```

### Menús

```bash
# Hacer clic en menú
peekaboo menu --app Safari --path "Archivo > Nueva ventana"

# Listar items del menú
peekaboo menu --app Safari --list
```

### Portapapeles

```bash
# Leer portapapeles
peekaboo clipboard --read

# Escribir en portapapeles
peekaboo clipboard --write "Texto copiado"
```

## Flujo típico (ver → hacer clic → escribir)

```bash
# 1. Capturar y anotar la pantalla
peekaboo see --annotate --path /tmp/estado.png

# 2. Hacer clic en el elemento deseado (por ID anotado)
peekaboo click --on B3

# 3. Escribir texto en el campo activo
peekaboo type "mi búsqueda" --return
```

## Notas

- Los IDs de elementos (B1, B2...) vienen del snapshot generado por `peekaboo see`.
- Usa `--json`/`-j` para salida legible por máquina.
- `--verbose`/`-v` para depuración.
- Los snapshots se cachean; usa `peekaboo clean` para limpiar la caché.
- Referencia completa: `peekaboo --help` o `peekaboo <cmd> --help`.
