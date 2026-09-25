# CLAUDE.md — MELONMELONSTORE

Site gerado pelo **SF (Site Factory)** em 15/04/2026. Migrado para o modelo Cloudflare + Supabase em 25/09/2026.

## Contexto do Site

**Nome:** MELONMELONSTORE
**Nicho:** Moda e Beleza
**Keywords:** Ola Eu sou Agatha Amante de qualquer coisa relacionado a moda e
**Paleta de cores:** gold | **Fonte:** outfit

Olá! Eu sou Agatha, Amante de qualquer coisa relacionado a moda e chocólatra assumida. Eu comecei meu blog porque eu precisava descobrir quem era eu de verdade. E a melhor maneira que eu encontrei de fazer isso é falando sobre mim. Eu sempre adorei montar roupas, então decidi que um blog de moda era o melhor caminho e aqui estou eu. Eu ainda estou me acostumando com essa coisa de blog, dando pequenos passos no mundo on-line, uma postagem por semana. E, claro, muitas fotos no instagram.

## Componentes visuais usados

| Seção | Variante |
|-------|----------|
| Header | Header-J |
| Hero | Hero-H |
| Features | Features-C |
| About Section | About-J |
| Posts | Posts-J |
| Footer | Footer-F |
| Página Sobre | Sobre-C |
| Página Contato | Contato-F |

## Estrutura do projeto

```
src/
  sections/        # Layout escolhido pelo SF — Header, Hero, Features, About, Posts, Footer, Sobre, Contato
  data/            # JSONs com todo o conteúdo editável
  lib/             # supabase.ts (cliente) e posts.ts (getPosts/getPostBySlug)
  components/      # Seo.astro (meta tags + JSON-LD)
  pages/           # Rotas Astro (index, sobre, contato, blog, privacidade, termos, [...slug])
  layouts/         # BaseLayout com fonte e cores dinâmicas
  styles/          # global.css com variáveis CSS de cor
public/
  images/          # hero.jpg, about.jpg, sobre.jpg
```

## O que editar

### Textos e conteúdo
- **`src/data/home.json`** — hero (título, subtítulo, botão), features (título, items), about section (título, desc, stats), posts
- **`src/data/sobre.json`** — conteúdo completo da página Sobre (hero, texto, missão)
- **`src/data/contato.json`** — título, subtítulo, email, tempo de resposta
- **`src/data/siteConfig.json`** — nome, slug, email, redes sociais, menu (título/descrição/OG/JSON-LD derivam daqui)

### Imagens
Imagens já estão em `public/images/` (via Pexels). Para substituir, mantenha os mesmos nomes de arquivo:
- `hero.jpg` — imagem de fundo do Hero (e og:image padrão)
- `about.jpg` — imagem da seção About (home)
- `sobre.jpg` — imagem de fundo da página Sobre

### Posts do blog
Os posts NÃO ficam mais em markdown local. São carregados do Supabase (tabela `network_posts`, filtrados por `domain = melonmelonstore.com.br`).
- `src/lib/posts.ts` — `getPosts()` e `getPostBySlug()`; `formatContentToHtml()` converte markdown → HTML.
- Sem painel admin. Novos posts/posts editados entram pela plataforma 8links e publicam automaticamente (via Git/CF).

### Cores
Variáveis em `src/styles/global.css`: `--color-primary`, `--color-accent`, `--color-dark`.

## SEO

- `src/components/Seo.astro` injetado pelo `BaseLayout`: title, description, canonical, OG, Twitter, `name="robots"`, JSON-LD (WebSite nas páginas estáticas, BlogPosting nos artigos).
- `src/pages/robots.txt.ts` e `src/pages/sitemap.xml.ts` gerados dinamicamente (sitemap inclui posts com lastmod).

## Deploy

```bash
bun install
bun run build
# Publicar no Cloudflare: a pasta dist/ é servida como Worker (adaptador @astrojs/cloudflare)
# Envs opcionais no CF: SUPABASE_URL e SUPABASE_ANON_KEY (fallbacks embutidos no código)
```