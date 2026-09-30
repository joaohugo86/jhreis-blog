# jhreis.com

Blog pessoal do **JH Reis** — um espaço onde escrevo sobre operações, processos,
tecnologia no dia a dia e outras coisas que acho interessantes.

Site estático feito com [Astro](https://astro.build), conteúdo em Markdown,
sem banco de dados e sem rastreadores.

## Rodando localmente

```bash
npm install
npm run dev      # servidor de desenvolvimento em http://localhost:4321
npm run build    # gera o site estático em ./dist
npm run preview  # pré-visualiza o build
```

## Como escrever um post

1. Crie a pasta `src/content/blog/<slug>/` e dentro dela um `index.md`.
2. Copie o frontmatter de `src/content/blog/modelo-de-post/index.md` e ajuste os campos.
3. O nome da pasta vira a URL do post: `/blog/<slug>`.
4. Use `draft: true` enquanto o texto não estiver pronto — rascunhos aparecem em
   `npm run dev`, mas ficam fora do `npm run build`.

## Estrutura

| Caminho | O que é |
| :--- | :--- |
| `src/config/site.ts` | Título, tagline, menu, redes sociais e feature flags do site |
| `src/content/blog/` | Os posts (um `index.md` por pasta) |
| `src/content/projects/` | Os projetos exibidos em `/projects` |
| `src/pages/` | Páginas (home, sobre, blog, tags, 404) |
| `src/components/` | Componentes de interface |
| `src/styles/global.css` | Design system e tipografia |

## Créditos

Construído sobre o tema [Minrock](https://github.com/rnt-rez/minrock),
de Renato Rezende, licenciado sob MIT.
