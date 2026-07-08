# E-mails do OND

Templates de e-mail (HTML **email-safe** + fallback texto) do OND.

Regras de construção (e-mail não é web):
- Layout 100% em `<table>`, largura máx. ~600px, centralizado.
- **CSS inline** — nada de `flex`, `grid`, `var()` ou classes externas.
- Fontes web-safe (Arial/Helvetica); pode citar `'Bio Sans'` antes, mas o cliente
  quase sempre cai no fallback — não conte com custom font.
- Logo em **texto estilizado** ("OND") — e-mail costuma bloquear SVG/imagem.
- Cores do Design System OND: Purple `#7F11F4` · Purple Night `#290756`
  · Gold `#F59811` · Neon `#11F558`. Fundo externo claro `#f4f2fb`, card branco.

Convenção de nomes: um arquivo por template, `produto-evento` — ex.
`pagamento-confirmado.html`. Sempre um `.txt` irmão como fallback de texto.

## Templates

### `pagamento-confirmado.html` (+ `.txt`)

Recibo enviado após a contratação da **Assessoria OND**, espelhando a tela de
confirmação da landing (`landings/assessoria/`).

- **Assunto:** `Pagamento confirmado — sua Assessoria OND está garantida ✦`
- **Preheader:** `Um especialista OND vai planejar sua viagem com você.`
- Header roxo com selo de confirmação, corpo, mini-recibo, botão CTA
  ("bulletproof", `border-radius` na `<td>` + no `<a>`), assinatura e rodapé.

#### Placeholders (o backend substitui antes de enviar)

| Placeholder      | Descrição                          | Exemplo                         |
|------------------|------------------------------------|---------------------------------|
| `{{nome}}`       | Primeiro nome do cliente           | `Renan`                         |
| `{{destino}}`    | Destino da viagem                  | `Paris`                         |
| `{{data_inicio}}`| Data de início                     | `10/09`                         |
| `{{data_fim}}`   | Data de fim                        | `15/09`                         |
| `{{preco}}`      | Valor **sem** o "R$" (já no template) | `32,50`                      |
| `{{cta_url}}`    | Destino do botão/CTA               | `https://wa.me/55XXXXXXXXXXX` ou `mailto:contato@ond...` |

#### Como o backend deve preencher

1. Carregar `pagamento-confirmado.html` (e `.txt` para a parte `text/plain`).
2. Fazer replace literal de cada `{{campo}}` pelos dados do lead
   (os mesmos campos que a rota `/advisory/<id>` já resolve para a landing:
   `nome`, `destino`, `inicio`, `fim`, `preco`).
3. Escapar os valores para HTML (evita quebra se um destino tiver `&`, `<`, etc.).
4. Enviar como e-mail **multipart/alternative**: `.txt` como `text/plain` e o
   HTML como `text/html`. O `subject` está no comentário do topo do `.html` e na
   1ª linha do `.txt`.

> Dica: o `{{cta_url}}` pode apontar para o WhatsApp do time (com mensagem
> pré-preenchida) ou para um `mailto:` — o corpo já pede para o cliente
> responder o e-mail com horários, então `mailto:`/responder funciona bem.
