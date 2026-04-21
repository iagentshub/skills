---
id: EMAIL
name: Email con Himalaya CLI
description: Gestionar correos electrónicos vía IMAP/SMTP con el CLI himalaya. Listar, leer, escribir, responder, reenviar, buscar y organizar emails desde el terminal. Soporta múltiples cuentas.
icon: 📧
category: messaging
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Email con Himalaya CLI

`himalaya` es un cliente de correo CLI que permite gestionar emails desde el terminal usando backends IMAP, SMTP, Notmuch o Sendmail.

## Requisitos

1. `himalaya` instalado (`himalaya --version` para verificar)
2. Fichero de configuración en `~/.config/himalaya/config.toml`
3. Credenciales IMAP/SMTP configuradas

## Instalación

```bash
brew install himalaya
```

## Configuración

Ejecuta el asistente interactivo para configurar una cuenta:

```bash
himalaya account configure
```

O crea `~/.config/himalaya/config.toml` manualmente:

```toml
[accounts.personal]
email = "tu@ejemplo.com"
display-name = "Tu Nombre"
default = true

backend.type = "imap"
backend.host = "imap.ejemplo.com"
backend.port = 993
backend.encryption.type = "tls"
backend.login = "tu@ejemplo.com"
backend.auth.type = "password"
backend.auth.cmd = "pass show email/imap"

message.send.backend.type = "smtp"
message.send.backend.host = "smtp.ejemplo.com"
message.send.backend.port = 587
message.send.backend.encryption.type = "start-tls"
message.send.backend.login = "tu@ejemplo.com"
message.send.backend.auth.type = "password"
message.send.backend.auth.cmd = "pass show email/smtp"
```

## Operaciones comunes

### Listar carpetas

```bash
himalaya folder list
```

### Listar emails

```bash
# Bandeja de entrada (por defecto)
himalaya envelope list

# Carpeta específica
himalaya envelope list --folder "Sent"

# Con paginación
himalaya envelope list --page 1 --page-size 20
```

### Buscar emails

```bash
himalaya envelope list from juan@ejemplo.com subject reunión
```

### Leer un email

```bash
# Leer por ID
himalaya message read 42

# Exportar MIME completo
himalaya message export 42 --full
```

### Responder a un email

```bash
# Respuesta interactiva (abre $EDITOR)
himalaya message reply 42

# Responder a todos
himalaya message reply 42 --all
```

### Reenviar un email

```bash
himalaya message forward 42
```

### Escribir un nuevo email

```bash
# Composición interactiva (abre $EDITOR)
himalaya message write
```

### Envío directo con plantilla

```bash
himalaya message send <<EOF
From: tu@ejemplo.com
To: destinatario@ejemplo.com
Subject: Asunto del mensaje

Cuerpo del mensaje aquí.
EOF
```

## Múltiples cuentas

```bash
# Listar cuentas configuradas
himalaya account list

# Usar cuenta específica
himalaya --account trabajo envelope list
```

## Notas

- Las contraseñas se almacenan de forma segura (no en texto plano) usando `pass`, `keyring` u otro gestor.
- Usa `--output json` para salida legible por máquina.
- Referencia completa: `himalaya --help` o `himalaya <subcomando> --help`.
