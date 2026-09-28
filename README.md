# my-portfolio

Portfólio pessoal e blog feito com [Hugo](https://gohugo.io/), com visual inspirado em terminal, sem dependências de Node.js, frameworks CSS ou JavaScript. Todos os templates, estilos e conteúdo ficam neste repositório, então dá para clonar, editar e publicar.

## Requisitos

- [Hugo](https://gohugo.io/installation/) **0.162 ou mais recente** (versão testada: `0.162.1`). A edição *extended* não é necessária.
- [Git](https://git-scm.com/) (opcional, para clonar e publicar)

Para conferir a versão instalada:

```bash
hugo version
```

## Instalação

### 1. Clone o repositório

```bash
git clone https://github.com/<seu-usuario>/my-portfolio.git
cd my-portfolio
```

> Se preferir, faça um *fork* primeiro e clone o seu fork.

### 2. Inicie o servidor de desenvolvimento

```bash
hugo server -D
```

O comando acima carrega o projeto no endereço [http://localhost:1313](http://localhost:1313). A flag `-D` inclui rascunhos (`draft = true`). Se a porta estiver ocupada, use `--port 1314` ou outra de sua preferência.

## Configuração

### `hugo.toml`

Edite os dados principais do site:

```toml
baseURL = "https://seudominio.com/"
title = "Seu Nome"
defaultContentLanguage = "pt-br"

[params]
  author = "Seu Nome"
  prompt = "guest@portfolio"          # texto da barra do terminal na home
  role = "Desenvolvedor(a) Full Stack"
  location = "Brasil"
  description = "Portfólio, projetos e blog de Seu Nome."  # meta description padrão
  bio = "Um parágrafo curto sobre você."
  avatar = "images/avatar.svg"        # caminho relativo a static/
  favicon = "favicon.ico"             # opcional; sem ele, usa o avatar
```

| Parâmetro     | Descrição                                               |
| ------------- | ------------------------------------------------------- |
| `author`      | Nome exibido na home e na meta tag `author`             |
| `prompt`      | Texto da barra superior do card de terminal             |
| `role`        | Cargo/função                                            |
| `location`    | Localização                                             |
| `description` | Descrição padrão usada nas meta tags                    |
| `bio`         | Texto de apresentação na home                           |
| `avatar`      | Foto de perfil (também usada como favicon padrão)       |
| `favicon`     | Ícone da aba do navegador (opcional)                    |

### Redes sociais

Adicione quantos blocos `[[params.social]]` quiser. Os ícones disponíveis são `github`, `linkedin`, `x`, `instagram`, `email` e `rss`; qualquer outro nome usa um ícone genérico de link.

```toml
[[params.social]]
  name = "github"
  url = "https://github.com/uruser"
[[params.social]]
  name = "email"
  url = "mailto:voce@example.org"
  label = "Fale comigo"   # opcional; por padrão exibe a URL sem o protocolo
```

### Menu

Os itens de navegação ficam em `[menus]`:

```toml
[[menus.main]]
  name = "projetos"
  pageRef = "/projects"
  weight = 10
```

### Avatar

Substitua `static/images/avatar.svg` pela sua foto (SVG, PNG ou JPG) e ajuste `params.avatar` se mudar o nome do arquivo.

### Favicon

Assim como no Next.js (`public/`), Vite (`public/`) ou Astro (`public/`), os ícones ficam na pasta de arquivos estáticos, que no Hugo é `static/`:

```
static/
├── favicon.ico            # ou favicon.svg / favicon.png
└── apple-touch-icon.png   # opcional, 180×180, usado no iOS
```

Depois, aponte o parâmetro no `hugo.toml`:

```toml
[params]
  favicon = "favicon.ico"
```

Sem `params.favicon`, o avatar é usado como ícone. O `apple-touch-icon.png` é detectado automaticamente se existir. Para gerar os arquivos a partir de uma imagem, use um serviço como o [RealFaviconGenerator](https://realfavicongenerator.net/).

### Competências (`data/skills.yaml`)

As competências são agrupadas por categoria. A home mostra as 12 primeiras; a lista completa é inserida em qualquer página com o shortcode `{{< skills >}}` (usado em `content/about.md`).

```yaml
- category: Linguagens
  items: [Go, Python, TypeScript]
- category: DevOps
  items: [Docker, Kubernetes, GitHub Actions]
```

## Conteúdo

```
content/
├── about.md          # página "Sobre" (layout = "about")
├── posts/            # artigos do blog
│   └── _index.md
└── projects/         # projetos do portfólio
    └── _index.md
```

### Página "Sobre"

Edite `content/about.md`. O texto antes de `<!--more-->` aparece como resumo na home; o resto aparece apenas na página completa.

### Criando um post

```bash
hugo new content posts/meu-primeiro-post.md
```

Front matter gerado (arquétipo em `archetypes/posts.md`):

```toml
+++
title = "Meu Primeiro Post"
date = 2026-01-01T10:00:00-03:00
description = "Resumo exibido na listagem e nas meta tags."
tags = ["hugo", "web"]
draft = true
+++
```

### Criando um projeto

```bash
hugo new content projects/meu-projeto.md
```

Front matter gerado (arquétipo em `archetypes/projects.md`):

```toml
+++
title = "meu-projeto"
date = 2026-01-01T10:00:00-03:00
description = "Descrição curta exibida no card."
stack = ["Go", "Docker"]
repo = "https://github.com/uruser/meu-projeto"   # opcional
demo = "https://meu-projeto.example.org"         # opcional
weight = 1                                        # opcional; menor aparece primeiro
draft = true
+++
```

A home mostra os 4 primeiros projetos e os 5 posts mais recentes.

> Lembre-se de trocar `draft = true` por `draft = false` (ou remover a linha) para publicar.

### Shortcodes

Para inserir elementos ricos no Markdown, use [shortcodes](https://gohugo.io/content-management/shortcodes/) em vez de HTML puro.

**Personalizados** (em `layouts/_shortcodes/`):

| Shortcode        | Descrição                                          |
| ---------------- | -------------------------------------------------- |
| `{{< skills >}}` | Lista de competências de `data/skills.yaml`        |

**Embutidos no Hugo** (exemplos úteis):

```markdown
{{< figure src="/images/print.png" alt="Tela do projeto" caption="Dashboard" >}}

{{< details summary="Ver detalhes" >}}
Conteúdo **recolhível**.
{{< /details >}}

[Veja o projeto]({{< relref "projects/cli-tasks" >}})

{{< youtube ID_DO_VIDEO >}}
```

> Prefira `relref` para links internos: o Hugo valida o destino no build e falha se a página não existir.

## Personalização

- **Estilos:** todo o CSS está em `assets/css/main.css`.
- **Templates:** ficam em `layouts/`; os componentes reutilizáveis estão em `layouts/_partials/` e os shortcodes em `layouts/_shortcodes/`.
- **Realce de código:** altere `markup.highlight.style` no `hugo.toml` (veja os [estilos disponíveis](https://gohugo.io/quick-reference/syntax-highlighting-styles/)).

## Estrutura do projeto

```
.
├── archetypes/   # modelos de front matter para `hugo new`
├── assets/css/   # estilos processados pelo Hugo Pipes
├── content/      # páginas, posts e projetos (Markdown)
├── data/         # dados estruturados (skills.yaml)
├── layouts/      # templates HTML e partials
├── static/       # arquivos copiados como estão (imagens, favicon)
└── hugo.toml     # configuração do site
```

## Build e deploy

Gere a versão de produção:

```bash
hugo --minify
```

O site é gerado em `public/`, que pode ser publicado em qualquer hospedagem estática (GitHub Pages, Netlify, Cloudflare Pages, Vercel etc.). Antes, confira se o `baseURL` no `hugo.toml` aponta para o domínio final.

Exemplo de workflow para **GitHub Pages** (`.github/workflows/hugo.yml`):

```yaml
name: Deploy Hugo
on:
  push:
    branches: [main]
permissions:
  contents: read
  pages: write
  id-token: write
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: "0.162.1"
      - run: hugo --minify
      - uses: actions/upload-pages-artifact@v3
        with:
          path: ./public
  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
    steps:
      - uses: actions/deploy-pages@v4
```

Depois, em **Settings → Pages**, selecione **GitHub Actions** como fonte.
