# Las chorradas de Juanjo

Blog personal en [blog.juanjorequena.com](https://blog.juanjorequena.com), hecho con [Jekyll](https://jekyllrb.com) y el tema [Beautiful Jekyll](https://beautifuljekyll.com) (como gema). Se publica en GitHub Pages automáticamente con cada push a `main`.

## Escribir un post

Crea `_posts/AAAA-MM-DD-slug.md`. La URL será `/slug/`.

```markdown
---
title: "Título del post"
subtitle: "Opcional"
tags: [Databases, books]
post-cover: "/assets/img/posts/slug/cover.jpg"     # opcional, imagen al inicio del post
thumbnail-img: "/assets/img/posts/slug/cover.jpg"  # opcional, miniatura en la portada
share-img: "/assets/img/posts/slug/cover.jpg"      # opcional, imagen al compartir en redes
---

Contenido en Markdown...
```

Las imágenes van en `assets/img/posts/<slug>/`. Dentro del post, enlázalas con
`![alt]({{ '/assets/img/posts/<slug>/imagen.png' | relative_url }})` para que funcionen con cualquier `baseurl`.

Para destacar un texto con el estilo de "callout":

```html
<div class="callout"><span class="callout-emoji">💡</span><div class="callout-text">Texto</div></div>
```

## Ver el blog en local

Requiere Ruby >= 3 (`brew install ruby`).

```sh
bundle install
bundle exec jekyll serve --livereload
# http://localhost:4000
```

## Notas

- `_layouts/` y `_includes/` contienen copias de archivos del tema 6.0.1 solo para traducir textos al español. Si actualizas la versión del tema en el `Gemfile`, revisa esos archivos.
- `/rss.xml` se mantiene por compatibilidad con los suscriptores del antiguo blog de Hashnode; `/feed.xml` es el mismo feed.
