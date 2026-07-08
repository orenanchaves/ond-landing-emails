# OND — Landing Pages

Repositório das **landing pages do OND** — a plataforma de viagens conversacional com IA (o "vAI das viagens").

Cada landing é um HTML **estático e autossuficiente** (CSS e SVGs inline, sem build, sem dependências externas). Basta abrir o arquivo no navegador ou publicar a pasta em qualquer host estático.

## Estrutura

```
ond-landing/
├── index.html            ← hub: índice visual apontando para as landings
├── README.md             ← este arquivo
└── assessoria/
    └── index.html        ← Landing: Assessoria OND
```

Novas landings entram como uma nova subpasta (`ex.: /copa/`, `/b2b/`) com seu próprio `index.html`, e ganham um card no `index.html` da raiz.

---

## Landing: Assessoria OND

Landing personalizada enviada por link (WhatsApp) depois que o lead conversa com o **OND vAI**. Vende a **Assessoria OND** — um especialista humano que planeja e ajusta a viagem junto com o cliente, apoiado pela IA da OND — por **R$ 32,50** (pagamento único).

Duas telas na mesma página: a **oferta** (view inicial) e a **confirmação de pagamento** (exibida ao clicar em "Contratar agora"; hoje é uma demo, o checkout real entra nesse ponto).

### Personalização por querystring

A página lê os dados do lead da URL. Enquanto é estática, os parâmetros vêm pela querystring; em produção, a rota `/advisory/<id>` resolve esses campos no backend e injeta na página.

| Parâmetro  | Aliases                 | Exemplo   | Padrão (fallback) |
|------------|-------------------------|-----------|-------------------|
| `nome`     | `name`                  | `Renan`   | `Renan`           |
| `destino`  | `dest`                  | `Paris`   | `Paris`           |
| `inicio`   | `ini`, `dataIni`        | `10/09`   | `10/09`           |
| `fim`      | `dataFim`               | `15/09`   | `15/09`           |
| `dias`     | —                       | `6`       | calculado de `inicio`→`fim` (senão `6`) |
| `preco`    | —                       | `32,50`   | `32,50`           |

**Exemplo:**

```
https://ond-assessoria.pages.dev/?nome=Renan&destino=Paris&inicio=10/09&fim=15/09
```

Sem parâmetros, a página cai nos valores de demonstração.

### Deploy atual

- **Cloudflare Pages:** https://ond-assessoria.pages.dev

---

## Como rodar localmente

Por ser HTML estático, qualquer um destes funciona:

```bash
# abrir direto no navegador
start assessoria/index.html          # Windows
open  assessoria/index.html          # macOS

# ou servir a pasta (permite testar a querystring com URLs limpas)
python -m http.server 8080
# → http://localhost:8080/assessoria/?nome=Renan&destino=Roma&inicio=01/12&fim=07/12
```

## Como fazer deploy

Cada landing pode ir para qualquer host estático (Cloudflare Pages, GitHub Pages, Netlify...). Padrão atual — Cloudflare Pages:

```bash
# publica a pasta da assessoria
wrangler pages deploy assessoria --project-name ond-assessoria
```

> A landing da Assessoria segue viva também em `C:\_tudo\Agama\OND\ond-assessoria\` (fonte original / projeto Wrangler). Este repo é a cópia versionada e o lar das landings do OND daqui pra frente.

---

## Design System OND

| Token          | Cor       |
|----------------|-----------|
| OND Purple     | `#7F11F4` |
| Purple Night   | `#290756` |
| Gold           | `#F59811` |
| Neon           | `#11F558` |
| Ink            | `#1E1E1E` |

Fonte: **Bio Sans** (com fallbacks Onest / Hanken Grotesk / system-ui).
