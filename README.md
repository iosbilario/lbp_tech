# LBP Tecnologia — site institucional

Site institucional de página única da **LBP Tecnologia** (empresa exclusivamente PJ).
Tese da marca: **"Prova, não promessa."**

O herói traz um painel de indicadores que busca **Selic (série 432)**, **IPCA 12m (13522)**
e **dólar (série 1)** direto da API pública do Banco Central (`api.bcb.gov.br`) via `fetch`
no navegador. Quando a API responde, o selo mostra **"AO VIVO"**; quando não responde,
mantém a **"ÚLTIMA LEITURA"** conhecida (fallback embutido no HTML) — nunca finge tempo real.

Case citado: [observatoriodetaxas.tec.br](https://observatoriodetaxas.tec.br).

## Como funciona (sem build)

O site é 100% estático e sem dependências de build. É um único `index.html` com HTML, CSS
e JS embutidos. Para publicar, basta **copiar os arquivos** para qualquer hospedagem estática
(GitHub Pages, S3, Netlify, um servidor Nginx, etc.). Nada de framework, bundler ou
minificação — manutenção fácil vem antes de otimização.

## Estrutura

```
.
├── index.html      # a landing page (fonte da verdade — não alterar design/textos/links)
├── 404.html        # página de erro mínima, reusando topo/rodapé e tokens da marca
├── robots.txt      # libera indexação e aponta o sitemap
├── sitemap.xml     # sitemap simples (1 URL: o domínio final)
├── .nojekyll       # desliga o processamento Jekyll no GitHub Pages
├── assets/
│   ├── favicon.svg              # marca em quadrado arredondado (idêntica ao favicon do index)
│   ├── lbp-logo-mark.svg        # marca (tinta nanquim) para fundo claro
│   └── lbp-logo-mark-branco.svg # marca (branca) para fundo escuro
└── README.md
```

> **Nota sobre os assets:** o `index.html` desenha o logo e o favicon como SVG embutido —
> ele **não depende** de nenhum arquivo em `assets/`. Os SVGs acima estão aqui para uso avulso
> (apresentações, e-mail, etc.). Se quiser guardar também os PNGs do LinkedIn
> (`lbplogohorizontal1200.png`, `linkedinlogo300x300.png`, `linkedinbanner1128x191.png`),
> é só copiá-los para dentro de `assets/`.

## Como publicar no GitHub Pages

Substitua `<usuario>` pela conta GitHub dona do repositório.

### Opção A — com o GitHub CLI (`gh`)

```bash
# na raiz do projeto, com o gh já autenticado (gh auth login)
git add -A
git commit -m "Publica site institucional da LBP Tecnologia"

# cria o repositório remoto público e faz o push (se ainda não existir)
gh repo create <usuario>/lbptecnologia.com.br --public --source=. --remote=origin --push

# ativa o GitHub Pages na branch main, pasta raiz (/)
gh api -X POST repos/<usuario>/lbptecnologia.com.br/pages \
  -f "source[branch]=main" -f "source[path]=/"
```

A URL sai como `https://<usuario>.github.io/lbptecnologia.com.br/`.
Se o repositório se chamar exatamente `<usuario>.github.io`, o site fica no apex
`https://<usuario>.github.io/`.

### Opção B — pelo site do GitHub

1. Crie um repositório público e envie estes arquivos (`git push`).
2. Em **Settings → Pages**, selecione **Deploy from a branch**, branch `main`, pasta `/ (root)`.
3. Aguarde o deploy e acesse a URL que o GitHub mostrar.

## Trocar o e-mail de contato

Hoje o site usa **`iosbilario@gmail.com`** em **dois pontos do `index.html`**, na seção de
contato:

1. O botão **"Escrever para a LBP"**:
   `href="mailto:iosbilario@gmail.com?subject=Contato%20via%20lbptecnologia.com.br"`
2. O cartão de contato (linha do e-mail):
   `href="mailto:iosbilario@gmail.com"` / texto `iosbilario@gmail.com`

Quando existir um e-mail no domínio (ex.: `contato@lbptecnologia.com.br`), troque as **três
ocorrências** (dois `href` + o texto visível). Um find-and-replace de `iosbilario@gmail.com`
resolve tudo.

## Domínio próprio (`lbptecnologia.com.br`) — passo a passo para depois

O site funciona primeiro na URL do GitHub Pages. Quando o domínio estiver **registrado no
Registro.br** e você quiser apontá-lo, siga esta ordem:

1. **No Registro.br (DNS da zona `lbptecnologia.com.br`):**
   - 4 registros **A** para o apex (`@`), apontando para o GitHub Pages:
     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```
   - 1 registro **CNAME** para `www` apontando para `<usuario>.github.io.`
2. **No repositório:** crie um arquivo `CNAME` (na raiz) com uma única linha:
   ```
   lbptecnologia.com.br
   ```
   (ou configure em **Settings → Pages → Custom domain**, que cria o `CNAME` para você).
3. Aguarde a checagem de DNS do GitHub e marque **"Enforce HTTPS"** em Settings → Pages.

> O arquivo `CNAME` **ainda não foi criado** neste repositório porque o domínio pode não
> estar registrado/apontado. Crie-o apenas quando o DNS acima estiver no ar.

## Tokens da marca (identidade visual — não alterar)

| Token       | Hex       | Uso                                         |
|-------------|-----------|---------------------------------------------|
| papel       | `#F5F7F8` | fundo                                       |
| nanquim     | `#122B3F` | tinta principal / texto                     |
| meridiano   | `#1B5FAB` | azul de confiança / links                   |
| sinal       | `#E08A1E` | âmbar — **só para dado vivo**               |
| grafite     | `#51636F` | texto secundário                            |
| traço       | `#D9E2E8` | linhas / bordas                             |

**Tipografia:** Archivo (display), IBM Plex Sans (corpo), IBM Plex Mono (dados).

---

© 2026 LBP Tecnologia. Conteúdo do painel: Banco Central do Brasil (SGS) — informativo,
sem recomendação financeira.
