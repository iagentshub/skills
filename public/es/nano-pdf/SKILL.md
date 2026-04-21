---
id: EDITAR_PDF
name: Edición de PDFs con nano-pdf
description: Editar PDFs con instrucciones en lenguaje natural usando el CLI nano-pdf. Permite cambiar texto, títulos y contenido de páginas específicas.
icon: 📄
category: productivity
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Skill: Edición de PDFs con nano-pdf

`nano-pdf` aplica ediciones a páginas específicas de un PDF usando instrucciones en lenguaje natural.

## Requisitos

- `nano-pdf` instalado (`pip install nano-pdf` o `uv tool install nano-pdf`)

## Instalación

```bash
uv tool install nano-pdf
# o
pip install nano-pdf
```

## Uso básico

```bash
nano-pdf edit documento.pdf 1 "Cambia el título a 'Resultados Q3' y corrige la errata del subtítulo"
```

## Ejemplos

```bash
# Cambiar el título de la portada (página 1)
nano-pdf edit presentacion.pdf 1 "Reemplaza el título por 'Informe Anual 2026'"

# Actualizar una fecha en la portada
nano-pdf edit informe.pdf 1 "Cambia la fecha a 'Abril 2026'"

# Corregir texto en una página específica
nano-pdf edit contrato.pdf 3 "Corrige 'Artículo 2.1' por 'Artículo 2.2' en el tercer párrafo"

# Añadir información en una página
nano-pdf edit folleto.pdf 2 "Añade 'Versión revisada' debajo del subtítulo"
```

## Notas

- Los números de página pueden ser 0-based o 1-based según la versión; si el resultado parece desplazado una página, prueba con el otro.
- Verifica siempre el PDF resultante antes de distribuirlo.
- La herramienta usa IA para interpretar la instrucción; sé específico para mejores resultados.
- Para ediciones complejas o múltiples páginas, es preferible editar el documento fuente (Word, InDesign, etc.) y regenerar el PDF.
