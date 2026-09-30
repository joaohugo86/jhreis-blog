---
title: "Modelo de post: entendendo o frontmatter"
description: "Referência dos campos de metadados disponíveis no frontmatter de cada post. Use este arquivo como modelo ao criar uma nota nova."
pubDate: 2026-09-29
tags: ["modelo", "frontmatter", "astro"]
draft: false
---

Todo artigo publicado aqui começa com um bloco de metadados no topo do arquivo, chamado **frontmatter YAML**.

Delimitado por duas linhas de três hifens (`---`), o frontmatter informa ao Astro os atributos essenciais do post — título, data de publicação, tags e imagem de preview para redes sociais — sem poluir o texto do artigo.

---

## O modelo padrão

Ao criar uma nota nova em `src/content/blog/<slug>/index.md`, comece com este esquema:

```yaml
---
title: "O título do seu artigo"
description: "Um resumo de 1 ou 2 frases para buscadores, cards e RSS."
pubDate: 2026-09-29
updatedDate: 2026-09-30
tags: ["operações", "processos", "astro"]
draft: false
image: "/images/capa.jpg"
audio: true
---
```

---

## Campo por campo

O que cada propriedade faz, seu tipo e por que ela importa:

| Propriedade | Tipo | Obrigatório? | Para que serve |
| :--- | :--- | :--- | :--- |
| **`title`** | `string` | **Sim** | Título principal do artigo. Usado na tag `<title>`, no H1, nos cards e nos dados estruturados JSON-LD. |
| **`description`** | `string` | **Sim** | Essencial para SEO e compartilhamento. Aparece nos previews do LinkedIn e do X, nas descrições do RSS e na busca interna. |
| **`pubDate`** | `date` | **Sim** | Data de publicação (`AAAA-MM-DD`). A home e a página `/blog` ordenam os artigos por ela. |
| **`updatedDate`** | `date` | Opcional | Exibe um aviso discreto de *"atualizado em [data]"* quando você revisa um texto antigo. |
| **`tags`** | `array` | Opcional | Tags de categorização (`["astro", "processos"]`). Alimentam a nuvem de tags em `/tags` e o filtro por tema. |
| **`draft`** | `boolean` | Opcional (padrão `false`) | Com `draft: true`, o post aparece no desenvolvimento local (`npm run dev`), mas fica fora do build de produção (`npm run build`). |
| **`image`** | `string` | Opcional | Caminho da imagem de capa (por exemplo `/images/capa.png` ou um arquivo dentro da própria pasta do post). |
| **`audio`** | `boolean` | Opcional (padrão `true`) | Controla se o leitor de áudio aparece abaixo do título do artigo. |

---

## Exemplos práticos

### 1. Nota rápida

Quando você só quer registrar uma ideia:

```markdown
---
title: "Padrões de cache sem latência"
description: "Notas rápidas sobre implementações de cache LRU em memória."
pubDate: 2026-09-29
tags: ["sistemas", "performance"]
---

Minhas anotações sobre limites de memória...
```

### 2. Rascunho em andamento

Quando você está escrevendo algo longo e quer revisar localmente sem publicar por acidente:

```markdown
---
title: "Um mergulho em consenso distribuído"
description: "Rascunho sobre timeouts de eleição e replicação de log no Raft."
pubDate: 2026-09-29
tags: ["sistemas-distribuidos"]
draft: true
---
```

> [!TIP]
> Com `draft: true`, o build de produção ignora o arquivo — nenhum texto inacabado vaza para o site publicado.

---

## Frontmatter no Obsidian

A pasta de conteúdo é um vault nativo do Obsidian, então vale usar as ferramentas visuais:

1. **Plugin Properties (nativo):** ative a visualização *"Properties"* nas configurações para editar o frontmatter em campos de formulário, com seletor de tags e calendário, em vez de YAML cru.
2. **Slug automático:** o nome da pasta em `src/content/blog/<slug>/index.md` se torna a URL definitiva (`/blog/<slug>`), o que mantém o nome do arquivo independente do título.

---

## Validação com Astro e Zod

O esquema de conteúdo fica definido com **Zod** em `src/content.config.ts`:

```typescript
const blog = defineCollection({
  loader: glob({ pattern: '**/[^_]*.{md,mdx}', base: './src/content/blog' }),
  schema: z.object({
    title: z.string(),
    description: z.string(),
    pubDate: z.coerce.date(),
    updatedDate: z.coerce.date().optional(),
    tags: z.array(z.string()).default([]),
    draft: z.boolean().default(false),
    image: z.string().optional(),
    audio: z.boolean().default(true),
  })
});
```

Se uma data estiver mal formatada ou um campo obrigatório faltar, o Astro interrompe o build e aponta o arquivo e a linha exatos do problema. É o que garante que o site publicado nunca tenha página quebrada.
