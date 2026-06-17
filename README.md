# Site da disciplina Agentes Inteligentes

Site estático que apresenta o material da disciplina **Agentes Inteligentes**
(PPGI · IC / UFAL · 2026.1), organizado na linha do tempo **Passado → Presente → Futuro**.

É um site **sem build**: o navegador lê um arquivo `structure.json` que descreve a
hierarquia de páginas e renderiza o conteúdo Markdown em tempo real. Não há servidor,
framework ou etapa de compilação, apenas HTML, CSS e um arquivo JavaScript. Por isso
roda diretamente no **GitHub Pages**.

## Como funciona

- **`content/structure.json`** define a árvore de seções (sidebar, páginas e navegação).
- Cada nó da árvore é uma **página**:
  - um nó **com `markdown`** renderiza esse arquivo Markdown (página de conteúdo);
  - um nó **com `children`** vira uma página de seção que mostra a descrição e
    *cards* clicáveis para as subpáginas.
- A navegação é por **hash** (`#/presente/best-practices/subagents`), então o
  recarregar de página e os links diretos funcionam sem configuração de servidor.

O Markdown é renderizado com formatação rica: títulos, listas, tabelas (com rolagem
horizontal no mobile), blocos de código com realce de sintaxe, diagramas **Mermaid**,
imagens e um **visualizador de PDF embutido** (todo link `.pdf` vira um card que abre
o PDF inline).

## Design

Tema *obsidian glass*: fundo escuro com elementos de vidro fosco (*frosty glass*),
aurora de fundo, tipografia Fraunces / Hanken Grotesk / JetBrains Mono, transições
suaves e itens recolhíveis. Layout responsivo com *drawer* lateral no mobile.

## Estrutura do repositório

```
githubpages/
├── index.html              # casca da página (sidebar, topo, libs via CDN)
├── .nojekyll               # desativa o Jekyll no GitHub Pages (serve pastas/_ como estão)
├── assets/
│   ├── css/styles.css      # tema obsidian glass (design tokens em :root)
│   ├── js/app.js           # roteador + renderer (fetch → marked → realce)
│   └── images/             # favicons, logo
└── content/
    ├── structure.json      # << a árvore de páginas do curso
    ├── passado/            # Markdown + assets por seção
    ├── presente/
    └── futuro/             # papers (pdfs/) + páginas de world models
```

Cada seção guarda seu próprio Markdown e os *assets* (imagens, PDFs) ao lado dos
arquivos que os referenciam, usando caminhos relativos.

## Editar o conteúdo

1. Escreva/edite o Markdown dentro de `content/<seção>/…`.
2. Coloque imagens e PDFs perto do `.md` (ou numa subpasta) e referencie por caminho
   relativo, por exemplo `![](figuras/diagrama.png)`, `[Paper](pdfs/artigo.pdf)`.
3. Registre a página em `content/structure.json` com `name`, `slug`, `description`
   e `markdown` (página de conteúdo) **ou** `children` (página de seção).
4. Use *slugs* em `kebab-case`, únicos entre os irmãos.

## Rodar localmente

O navegador bloqueia `fetch` via `file://`, então sirva a pasta por HTTP:

```bash
cd githubpages
python3 -m http.server 8000
# abra http://localhost:8000
```

## Publicar

É um site estático puro, basta servir o conteúdo de `githubpages/` no GitHub Pages
(o `.nojekyll` garante que pastas e arquivos sejam servidos sem processamento). Não há
nada a compilar.

> Detalhes de arquitetura e convenções para manutenção (inclusive por agentes de IA)
> estão em [CLAUDE.md](CLAUDE.md).
