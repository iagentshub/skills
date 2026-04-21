---
id: AGENTE_CODIGO
name: Agente de Código
description: "Delega tareas de código a Claude Code o Codex vía proceso en segundo plano. Úsalo cuando: (1) construyas nuevas funcionalidades, (2) revises PRs en directorio temporal, (3) refactorices bases de código grandes, (4) necesites exploración iterativa de ficheros. NO para: correcciones de una línea (edita directamente), leer código (usa la herramienta de lectura), trabajo en el propio workspace del agente."
icon: 🧩
category: dev
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Agente de Código

Usa **bash** (con modo background opcional) para todo el trabajo de agentes de código.

## PTY: Codex sí, Claude Code no

Para **Codex**, PTY sigue siendo necesario (aplicaciones de terminal interactiva):

```bash
# ✅ Correcto para Codex
bash pty:true command:"codex exec 'Tu tarea aquí'"
```

Para **Claude Code** (`claude` CLI), usa `--print --permission-mode bypassPermissions`:

```bash
# ✅ Correcto para Claude Code (sin PTY)
cd /ruta/proyecto && claude --permission-mode bypassPermissions --print 'Tu tarea'

# Para ejecución en background:
bash workdir:~/proyecto background:true command:"claude --permission-mode bypassPermissions --print 'Tu tarea'"
```

## Inicio rápido: tareas en un solo paso

```bash
# Chat rápido (¡Codex necesita un repo git!)
SCRATCH=$(mktemp -d) && cd $SCRATCH && git init && codex exec "Tu tarea aquí"

# En un proyecto real — ¡con PTY!
bash pty:true workdir:~/Projects/mi-proyecto command:"codex exec 'Añade gestión de errores a las llamadas API'"
```

**¿Por qué git init?** Codex se niega a ejecutarse fuera de un directorio git de confianza.

## El patrón: workdir + background + pty

Para tareas largas, usa el modo background con PTY:

```bash
# Lanzar agente en el directorio objetivo (¡con PTY!)
bash pty:true workdir:~/proyecto background:true command:"codex exec --full-auto 'Construye un juego de serpiente'"
# Devuelve sessionId para seguimiento

# Monitorizar progreso
process action:log sessionId:XXX

# Comprobar si ha terminado
process action:poll sessionId:XXX

# Enviar input (si el agente hace una pregunta)
process action:submit sessionId:XXX data:"sí"

# Matar si es necesario
process action:kill sessionId:XXX
```

## Codex CLI

### Flags principales

| Flag | Efecto |
|------|--------|
| `exec "tarea"` | Ejecución directa, termina al acabar |
| `--full-auto` | Con sandbox, aprueba automáticamente en el workspace |
| `--yolo` | SIN sandbox, SIN aprobaciones (más rápido, más peligroso) |

### Construir/Crear

```bash
# Directo (aprueba automáticamente) — ¡recuerda PTY!
bash pty:true workdir:~/proyecto command:"codex exec --full-auto 'Construye el toggle de modo oscuro'"

# Background para trabajo más largo
bash pty:true workdir:~/proyecto background:true command:"codex --yolo 'Refactoriza el módulo de autenticación'"
```

### Revisar PRs

**⚠️ CRÍTICO: Nunca revises PRs en la propia carpeta del proyecto que estás editando.**
Clona a carpeta temporal o usa git worktree.

```bash
# Clonar a temporal para revisión segura
REVIEW_DIR=$(mktemp -d)
git clone https://github.com/usuario/repo.git $REVIEW_DIR
cd $REVIEW_DIR && gh pr checkout 130
bash pty:true workdir:$REVIEW_DIR command:"codex review --base origin/main"
# Limpiar después: rm -rf $REVIEW_DIR

# O usar git worktree (mantiene main intacto)
git worktree add /tmp/pr-130-review rama-pr-130
bash pty:true workdir:/tmp/pr-130-review command:"codex review --base main"
```

## Claude Code

```bash
# Primer plano
bash workdir:~/proyecto command:"claude --permission-mode bypassPermissions --print 'Tu tarea'"

# Background
bash workdir:~/proyecto background:true command:"claude --permission-mode bypassPermissions --print 'Tu tarea'"
```

## Acciones del proceso (para sesiones en background)

| Acción | Descripción |
|--------|-------------|
| `list` | Listar todas las sesiones en ejecución/recientes |
| `poll` | Comprobar si la sesión sigue activa |
| `log` | Obtener salida de la sesión |
| `write` | Enviar datos a stdin |
| `submit` | Enviar datos + intro (como escribir y pulsar Enter) |
| `kill` | Terminar la sesión |
