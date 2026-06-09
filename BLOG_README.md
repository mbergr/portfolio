# Blog — quick reference

## Crear un post nuevo

1. Crea un archivo en `_posts/` con el nombre `YYYY-MM-DD-titulo-con-guiones.md`.
2. Agrega este front matter al inicio del archivo:

```yaml
---
layout: post
title: "Tu título aquí"
date: YYYY-MM-DD
description: "Una línea para SEO y el excerpt en el listado."
tags: [dbt, sql, python]   # opcional
---
```

3. Escribe el contenido en Markdown debajo del segundo `---`. Usa ` ```sql `, ` ```python ` o ` ```yaml ` para bloques de código con syntax highlighting.

## Publicar

```bash
git add _posts/tu-post.md
git commit -m "New post: título"
git push
```

GitHub Pages hace el deploy automáticamente. Tarda **1–3 minutos** en reflejarse en `mbergr.github.io/portfolio/blog/`.

## Preview local (opcional)

Requiere Ruby + la gem `github-pages` instalada:

```bash
bundle exec jekyll serve --baseurl "/portfolio"
# Abre http://localhost:4000/portfolio/blog/
```

Si no tienes Ruby configurado, simplemente haz push y revisa el resultado en vivo — es igual de rápido.
