---
id: 1PASSWORD
name: 1Password
description: "Configura y usa el CLI de 1Password (op). Úsalo para instalar el CLI, habilitar la integración con la app de escritorio, iniciar sesión (cuenta única o multi-cuenta) y leer/inyectar secretos con op. Solo si usas 1Password como gestor de contraseñas."
icon: 🔐
category: security
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# 1Password CLI

Sigue los pasos oficiales de inicio. No adivines los comandos de instalación.

Homepage: https://developer.1password.com/docs/cli/get-started/

## Flujo de trabajo

1. Comprueba el SO y el shell.
2. Verifica que el CLI está instalado: `op --version`.
3. Confirma que la integración con la app de escritorio está habilitada y la app está desbloqueada.
4. **OBLIGATORIO:** crea una sesión tmux para todos los comandos `op` (nunca llames a `op` fuera de tmux).
5. Inicia sesión/autoriza dentro de tmux: `op signin` (espera la confirmación en la app).
6. Verifica el acceso dentro de tmux: `op whoami` (debe tener éxito antes de leer secretos).
7. Si hay varias cuentas: usa `--account` o `OP_ACCOUNT`.

## Instalación

```bash
brew install 1password-cli
```

## Sesión tmux obligatoria

La herramienta de shell usa un TTY fresco por cada comando. Para evitar reautenticaciones y fallos, ejecuta siempre `op` dentro de una sesión tmux dedicada:

```bash
SESSION="op-auth-$(date +%Y%m%d-%H%M%S)"
tmux new-session -d -s "$SESSION"
tmux send-keys -t "$SESSION" "op signin" Enter
tmux send-keys -t "$SESSION" "op whoami" Enter
tmux send-keys -t "$SESSION" "op vault list" Enter
tmux capture-pane -p -J -t "$SESSION" -S -200
tmux kill-session -t "$SESSION"
```

## Comandos comunes

```bash
# Verificar sesión activa
op whoami

# Listar vaults
op vault list

# Leer un secreto
op read "op://vault/item/field"

# Inyectar secretos en un comando
op run -- tu-comando-con-env-vars

# Inyectar secretos en un fichero de plantilla
op inject -i template.env -o .env
```

## Guardianes de seguridad

- Nunca pegues secretos en logs, chat o código.
- Prefiere `op run` / `op inject` sobre escribir secretos a disco.
- Si se necesita inicio de sesión sin integración con la app, usa `op account add`.
- Si un comando devuelve "account is not signed in", re-ejecuta `op signin` dentro de tmux y autoriza en la app.
- No ejecutes `op` fuera de tmux.
