---
id: APPLE_REMINDERS
name: Apple Reminders CLI
description: Gestionar recordatorios de Apple Reminders desde el terminal con remindctl. Listar, crear, completar y eliminar recordatorios con soporte de fechas y listas. Se sincroniza con iOS/iPadOS.
icon: ⏰
category: productivity
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Apple Reminders CLI (remindctl)

> ⚠️ **Solo macOS.** Los recordatorios se sincronizan con iCloud (iPhone/iPad).

Usa `remindctl` para gestionar Apple Reminders directamente desde el terminal.

## Cuándo usar

✅ **USA esta skill cuando:**

- El usuario menciona explícitamente "recordatorio" o la app "Recordatorios"
- Crear tareas personales con fecha de vencimiento que se sincronicen con iOS
- Gestionar listas de Apple Reminders
- El usuario quiere que las tareas aparezcan en su iPhone/iPad

## Cuándo NO usar

❌ **NO uses esta skill cuando:**

- Programar alertas del agente → usa cron o el planificador de tareas
- Eventos o citas de calendario → usa Apple Calendar
- Gestión de proyectos/trabajo → usa Notion, GitHub Issues u otras herramientas
- El usuario dice "recuérdame" pero se refiere a una alerta del agente → aclara primero

## Instalación

```bash
brew install steipete/tap/remindctl
```

Concede acceso a Recordatorios cuando se solicite.

## Ver recordatorios

```bash
remindctl               # Recordatorios de hoy
remindctl today         # Hoy
remindctl tomorrow      # Mañana
remindctl week          # Esta semana
remindctl overdue       # Vencidos
remindctl all           # Todos
remindctl 2026-05-01    # Fecha específica
```

## Gestionar listas

```bash
remindctl list                           # Ver todas las listas
remindctl list Trabajo                   # Ver lista específica
remindctl list "Compras" --create        # Crear lista
remindctl list "Compras" --delete        # Eliminar lista
```

## Crear recordatorios

```bash
remindctl add "Comprar pan"
remindctl add --title "Llamar a mamá" --list Personal --due tomorrow
remindctl add --title "Reunión de equipo" --due "2026-05-10 09:00"
remindctl add --title "Revisión médica" --list Salud --due "2026-06-01"
```

## Completar y eliminar

```bash
remindctl complete 1 2 3       # Completar por ID
remindctl delete 4A83 --force  # Eliminar por ID
```

## Formatos de salida

```bash
remindctl today --json         # JSON para scripts
remindctl today --plain        # Formato TSV
remindctl today --quiet        # Solo contadores
```

## Diagnóstico

```bash
remindctl status               # Verificar estado de permisos
remindctl authorize            # Solicitar acceso si se denegó
```

## Notas

- Solo macOS; los recordatorios se sincronizan con iCloud automáticamente.
- Usa `remindctl --help` o `remindctl <subcomando> --help` para ver todas las opciones.
