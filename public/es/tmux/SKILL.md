---
id: TMUX
name: Control de Sesiones tmux
description: Controlar sesiones tmux enviando teclas y leyendo la salida de los paneles. Útil para gestionar procesos interactivos de larga duración, CLIs interactivos y tareas en segundo plano.
icon: 🧵
category: dev
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Control de Sesiones tmux

Controla sesiones tmux enviando pulsaciones de teclas y leyendo la salida. Esencial para gestionar procesos interactivos y tareas en segundo plano.

## Cuándo usar

✅ **USA esta skill cuando:**

- Monitorizar procesos interactivos en tmux
- Enviar entrada a aplicaciones de terminal interactivas
- Leer la salida de procesos de larga duración en tmux
- Navegar paneles/ventanas de tmux programáticamente
- Comprobar el estado de trabajos en segundo plano

## Cuándo NO usar

❌ **NO uses esta skill cuando:**

- Ejecutar comandos de shell puntuales → usa el ejecutor directamente
- Iniciar nuevos procesos en segundo plano → usa el ejecutor con modo background
- Scripts no interactivos → usa el ejecutor de shell
- El proceso no está dentro de tmux

## Instalación

```bash
brew install tmux
```

## Comandos comunes

### Listar sesiones

```bash
tmux list-sessions
tmux ls
```

### Capturar salida

```bash
# Últimas 20 líneas del panel
tmux capture-pane -t mi-sesion -p | tail -20

# Scrollback completo
tmux capture-pane -t mi-sesion -p -S -

# Panel específico en una ventana
tmux capture-pane -t mi-sesion:0.0 -p
```

### Enviar teclas

```bash
# Enviar texto (sin pulsar Enter)
tmux send-keys -t mi-sesion "hola"

# Enviar texto + Enter
tmux send-keys -t mi-sesion "ls -la" Enter

# Teclas especiales
tmux send-keys -t mi-sesion Enter
tmux send-keys -t mi-sesion Escape
tmux send-keys -t mi-sesion C-c          # Ctrl+C
tmux send-keys -t mi-sesion C-d          # Ctrl+D (EOF)
tmux send-keys -t mi-sesion C-z          # Ctrl+Z (suspender)
```

### Navegación de ventanas y paneles

```bash
# Seleccionar ventana
tmux select-window -t mi-sesion:0

# Seleccionar panel
tmux select-pane -t mi-sesion:0.1

# Listar ventanas
tmux list-windows -t mi-sesion
```

### Gestión de sesiones

```bash
# Crear nueva sesión en segundo plano
tmux new-session -d -s nueva-sesion

# Matar sesión
tmux kill-session -t mi-sesion

# Renombrar sesión
tmux rename-session -t vieja nueva
```

## Flujo típico de uso

1. Crear sesión: `tmux new-session -d -s trabajo`
2. Lanzar proceso: `tmux send-keys -t trabajo "python script.py" Enter`
3. Capturar salida: `tmux capture-pane -t trabajo -p | tail -30`
4. Interactuar si es necesario: `tmux send-keys -t trabajo "s" Enter`
5. Limpiar: `tmux kill-session -t trabajo`

## Notas

- Los nombres de sesión distinguen mayúsculas/minúsculas.
- Usa `tmux list-panes -t sesion -F "#{pane_id} #{pane_current_command}"` para ver qué corre en cada panel.
- Para monitorización continua, captura la salida en un bucle con intervalos.
