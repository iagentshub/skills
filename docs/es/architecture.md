<div align="center">
  <a href="index.md">← Índice</a> &nbsp;·&nbsp;
  <a href="../en/architecture.md">🇬🇧 Read in English</a>
</div>

<br>

# Arquitectura de las skills

---

## Qué es una skill

Una skill es un conjunto de instrucciones que amplía las capacidades de un agente. Cuando un agente tiene una skill activada, las instrucciones de esa skill se incorporan automáticamente a su comportamiento. El agente sabe qué puede hacer y cómo hacerlo, sin necesidad de configuración adicional.

---

## Tipos de skills

**Skills públicas** — skills del catálogo oficial, disponibles para todos los agentes. Se sincronizan automáticamente desde este repositorio en cada arranque de la plataforma.

**Skills privadas** — skills específicas de una instalación, no incluidas en el catálogo público. Se almacenan localmente y no se distribuyen.

---

## Idiomas

El catálogo está disponible en varios idiomas. Cada skill existe en todas las versiones de idioma disponibles, con el mismo identificador en todas ellas. La plataforma sirve el catálogo en el idioma configurado por el administrador.

---

## Categorías

Las skills están organizadas por categoría según su función principal:

| Categoría | Qué incluye |
|---|---|
| **ai** | Integración con otros modelos de IA y herramientas de generación |
| **data** | Consulta de datos externos (tiempo, lugares, APIs) |
| **dev** | Herramientas de desarrollo (GitHub, control de versiones, terminal) |
| **media** | Captura y procesamiento de audio, vídeo e imagen |
| **messaging** | Comunicación (correo, mensajería, redes sociales) |
| **notes** | Gestión de notas y documentos |
| **productivity** | Productividad y gestión de tareas |
| **security** | Auditoría y seguridad del sistema |

---

## Cómo se distribuyen

Este repositorio es la fuente de verdad del catálogo público. La plataforma descarga automáticamente las skills en cada arranque, por lo que cualquier actualización en el repositorio se refleja en la próxima puesta en marcha sin intervención manual.

---

## Cómo contribuir

Cada skill es un fichero de texto con una cabecera de metadatos (nombre, descripción, icono, categoría) seguida de las instrucciones. Para añadir una skill al catálogo, debe existir en todos los idiomas disponibles con el mismo identificador. El sistema de integración continua verifica esta consistencia automáticamente.
