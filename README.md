# Allia Studio Blog

Blog de tecnología generado automáticamente por el sistema de content marketing de Allia Studio.

## Arquitectura

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     RESEARCH-PROJECT                                     │
│                     (Content Pipeline)                                   │
├─────────────────────────────────────────────────────────────────────────┤
│  RSS Sources → Scout → Analyst → Writer → Distribution Hub              │
│                                                  │                       │
│                                                  ▼                       │
│                                           GitHub API                     │
│                                           (commit .md)                   │
└──────────────────────────────────────────────────┬──────────────────────┘
                                                   │
                          ┌────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     ESTE REPOSITORIO                                     │
│                     allia-blog                                           │
├─────────────────────────────────────────────────────────────────────────┤
│  content/posts/*.md  →  Hugo Build  →  GitHub Pages                     │
│                              │                                           │
│                              ▼                                           │
│                     blog.allia.dev (o github.io)                        │
│                              │                                           │
│                              ▼                                           │
│                     RSS Feed (/index.xml)                               │
└──────────────────────────────────────────────────┬──────────────────────┘
                                                   │
                          ┌────────────────────────┘
                          ▼ (futuro: fetch RSS en build)
┌─────────────────────────────────────────────────────────────────────────┐
│                     ALLIA-LANDINGPAGE                                    │
│                     allia.dev                                            │
├─────────────────────────────────────────────────────────────────────────┤
│  BlogSection.astro  ←  Muestra últimos posts del RSS                    │
│  "Ver más" button   →  Redirige a blog.allia.dev                        │
└─────────────────────────────────────────────────────────────────────────┘
```

## Workflow

1. **Recolección**: research-project recolecta noticias de RSS feeds (HN, TechCrunch, etc.)
2. **Procesamiento**: Agentes de IA analizan y generan blog posts en español
3. **Adaptación**: Distribution Hub adapta el contenido para Hugo (frontmatter, SEO)
4. **Publicación**: GitHub API hace commit a `content/posts/`
5. **Build**: GitHub Actions ejecuta Hugo y despliega a GitHub Pages
6. **Distribución**: RSS feed disponible para la landing page

## Estructura

```
allia-blog/
├── hugo.toml              # Configuración Hugo
├── content/
│   └── posts/             # Blog posts (generados automáticamente)
│       └── YYYY-MM-DD-slug.md
├── layouts/               # Templates HTML
├── static/                # Assets estáticos
├── themes/
│   └── allia-theme/       # Tema personalizado
└── .github/
    └── workflows/
        └── hugo.yml       # GitHub Actions para build
```

## Desarrollo Local

```bash
# Instalar Hugo (macOS)
brew install hugo

# Instalar Hugo (Linux)
sudo apt install hugo

# Servidor de desarrollo
hugo server -D

# Build producción
hugo --minify
```

## Formato de Posts

Los posts son generados por el Distribution Hub con este formato:

```yaml
---
title: "Título del Post"
slug: "titulo-del-post"
date: 2024-01-23T10:00:00-05:00
draft: false
author: "Allia Studio"
description: "Meta description para SEO..."
categories: ["Tech", "AI"]
tags: ["machine-learning", "startups"]
---

Contenido del post en Markdown...
```

## Outputs

- **HTML**: Sitio estático en GitHub Pages
- **RSS**: `/index.xml` - Feed para la landing page
- **JSON**: `/index.json` - API de posts (opcional)

## Conexión con Landing Page

La landing page (allia.dev) consume el RSS feed en build time:

```typescript
// En Allia-landingpage/src/lib/blog.ts
const RSS_URL = 'https://allia-studio.github.io/allia-blog/index.xml';
const posts = await fetchAndParseRSS(RSS_URL);
```

## URLs

- **Blog**: https://allia-studio.github.io/allia-blog/
- **RSS**: https://allia-studio.github.io/allia-blog/index.xml
- **Posts**: https://allia-studio.github.io/allia-blog/posts/

## Configuración GitHub Pages

1. Settings → Pages
2. Source: GitHub Actions
3. El workflow `hugo.yml` se encarga del deploy

---

*Generado y mantenido por el sistema de content marketing de [Allia Studio](https://allia.dev)*
