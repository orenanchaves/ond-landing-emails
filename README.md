# OND — Landings & E-mails

Repositório das **landing pages** e dos **templates de e-mail** do OND — a plataforma de viagens conversacional com IA (o "vAI das viagens").

Tudo aqui é HTML **estático e autossuficiente** (CSS e SVGs inline, sem build, sem dependências externas). Landings são publicadas em host estático; e-mails são templates HTML prontos para colar no provedor de envio (ou usar como base no backend).

## Estrutura

```
ond-landing-emails/
├── index.html              ← hub: índice visual das landings
├── README.md               ← este arquivo
├── landings/
│   └── assessoria/
│       └── index.html      ← Landing: Assessoria OND
└── emails/                 ← templates de e-mail do OND (ex.: pagamento confirmado)
    └── README.md
```

- **Nova landing** → nova subpasta em `landings/` (ex.: `landings/copa/`) com seu `index.html`, e um card no `index.html` da raiz.
- **Novo e-mail** → novo arquivo/subpasta em `emails/` (ex.: `emails/assessoria-pagamento-confirmado.html`).

---

## Landings

### Assessoria OND — `landings/assessoria/`

Landing personalizada enviada por link (WhatsApp) depois que o lead conversa com o **OND vAI**. Vende a **Assessoria OND** — um especialista humano que planeja e ajusta a viagem junto com o cliente, apoiado pela IA da OND — por **R$ 32,50** (pagamento único).

Duas telas na mesma página: a **oferta** (view inicial) e a **confirmação de pagamento** (exibida ao clicar em "Contratar agora"; hoje é uma demo, o checkout real entra nesse ponto).

#### Personalização por querystring

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

#### Deploy atual

- **Cloudflare Pages:** https://ond-assessoria.pages.dev

---

## E-mails

Templates de e-mail transacionais e de campanha do OND, em `emails/`. Cada template é um HTML autossuficiente pensado para renderizar bem em clientes de e-mail (estilos inline, layout simples).

Primeiro previsto: **e-mail de "pagamento confirmado"** da Assessoria OND — o recibo enviado depois que o cliente contrata (espelha a tela de confirmação da landing). Ainda a produzir.

---

## Como rodar localmente

Por ser HTML estático, qualquer um destes funciona:

```bash
# abrir uma landing direto no navegador
start landings/assessoria/index.html     # Windows
open  landings/assessoria/index.html     # macOS

# ou servir a raiz do repo (permite testar a querystring com URLs limpas)
python -m http.server 8080
# → http://localhost:8080/landings/assessoria/?nome=Renan&destino=Roma&inicio=01/12&fim=07/12
```

## Como fazer deploy

Cada landing pode ir para qualquer host estático (Cloudflare Pages, GitHub Pages, Netlify...). Padrão atual — Cloudflare Pages:

```bash
# publica a pasta da assessoria
wrangler pages deploy landings/assessoria --project-name ond-assessoria
```

> A landing da Assessoria segue viva também em `C:\_tudo\Agama\OND\ond-assessoria\` (fonte original / projeto Wrangler). Este repo é a cópia versionada e o lar das landings e e-mails do OND daqui pra frente.

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
