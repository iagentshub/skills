---
id: GITHUB_COPILOT_SKILLS
name: GitHub Copilot Skills
description: "Crear, instalar y gestionar skills de GitHub Copilot Cloud Agent. Usar cuando: se construyan archivos SKILL.md para Copilot, se use el CLI gh skill, o se configuren skills de proyecto/personales. NO para: operaciones GitHub habituales (usar el skill github), ni sistemas de agentes no-Copilot."
icon: 🧑‍✈️
category: dev
created_at: "2026-06-05"
updated_at: "2026-06-05"
homepage: https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/create-skills
---

# GitHub Copilot Skills

Crea y gestiona skills para GitHub Copilot Cloud Agent. Las skills extienden el comportamiento de Copilot con instrucciones, scripts y recursos personalizados — cargados automáticamente cuando son relevantes.

## Qué es una skill de Copilot

Una skill es una carpeta con un archivo `SKILL.md` (y scripts/recursos opcionales) que Copilot carga cuando el prompt del usuario coincide con la descripción de la skill. Funciona en:
- Copilot Cloud Agent
- Code review
- GitHub CLI
- Modo agent en VS Code

## Ubicaciones de skills

| Alcance | Ubicación |
|---|---|
| Proyecto | `.github/skills/` o `.claude/skills/` o `.agents/skills/` |
| Personal | `~/.copilot/skills/` o `~/.agents/skills/` |

## Estructura del SKILL.md

```markdown
---
name: mi-skill
description: "Qué hace esta skill y cuándo debe usarla Copilot."
allowed-tools:
  - shell
---

# Título de la Skill

Instrucciones para Copilot en Markdown.

## Cuándo usar

Describe los desencadenantes y situaciones relevantes.

## Cómo usar

Instrucciones paso a paso o ejemplos.
```

### Campos obligatorios del frontmatter

| Campo | Descripción |
|---|---|
| `name` | Identificador único (minúsculas, con guiones) |
| `description` | Indica a Copilot qué hace la skill y cuándo activarla |

### Campos opcionales

| Campo | Descripción |
|---|---|
| `allowed-tools` | Preaprueba herramientas (ej: `shell`) sin preguntar al usuario cada vez |
| `license` | Identificador de licencia (ej: `MIT`) |

## Crear una skill desde cero

```bash
# Skill a nivel de proyecto
mkdir -p .github/skills/mi-skill
cat > .github/skills/mi-skill/SKILL.md << 'EOF'
---
name: mi-skill
description: "Descripción breve de qué hace la skill y cuándo usarla."
---

# Mi Skill

Instrucciones aquí...
EOF
```

## Skill con un script

```bash
mkdir -p .github/skills/ejecutar-tests

# El script
cat > .github/skills/ejecutar-tests/run.sh << 'EOF'
#!/bin/bash
npm test -- --reporter=json
EOF
chmod +x .github/skills/ejecutar-tests/run.sh

# El SKILL.md que referencia el script
cat > .github/skills/ejecutar-tests/SKILL.md << 'EOF'
---
name: ejecutar-tests
description: "Ejecuta el conjunto de tests y reporta resultados. Usar cuando se pida ejecutar tests o verificar su estado."
allowed-tools:
  - shell
---

# Ejecutar Tests

Ejecuta la suite de tests con el script proporcionado.

## Uso

```bash
bash .github/skills/ejecutar-tests/run.sh
```

Interpreta la salida JSON y resume los fallos.
EOF
```

## CLI gh skill

```bash
# Buscar skills por tema
gh skill search testing

# Instalar una skill de un repo de GitHub
gh skill install owner/repositorio nombre-skill

# Listar skills instaladas
gh skill list

# Actualizar todas las skills instaladas
gh skill update

# Validar antes de publicar (simulacro)
gh skill publish --dry-run

# Publicar una skill
gh skill publish
```

## Consejos para buenas descripciones

El campo `description` es lo que usa Copilot para decidir cuándo cargar la skill. Escríbelo como "Qué hacer y cuándo usarlo":

```yaml
# Bien — específico, orientado a la acción
description: "Despliega en Vercel y verifica el estado del despliegue. Usar cuando el usuario pida desplegar, previsualizar una rama o revisar los logs de despliegue."

# Mal — vago
description: "Ayudante de despliegue"
```

## Notas

- Copilot carga las skills automáticamente — no se necesita invocación explícita.
- Mantén las instrucciones concisas; Copilot carga el `SKILL.md` completo en contexto.
- `allowed-tools: [shell]` elimina las confirmaciones repetidas para comandos shell — usar con cuidado.
- Las skills en `.github/skills/` están versionadas y compartidas con el equipo; `~/.copilot/skills/` son solo personales.
- Referencia completa: `https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/create-skills`
