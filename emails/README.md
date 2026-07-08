# E-mails do OND

Templates de e-mail (HTML autossuficiente, estilos inline) do OND.

Convenção: um arquivo por template, nomeado por produto + evento — ex.:

```
emails/
└── assessoria-pagamento-confirmado.html
```

Cada template deve renderizar bem em clientes de e-mail: estilos inline, layout
em tabela quando necessário, sem dependências externas. Seguem o Design System
OND (Purple #7F11F4 · Purple Night #290756 · Gold #F59811 · Neon #11F558).

## A produzir

- **Assessoria OND — pagamento confirmado**: recibo enviado após a contratação,
  espelhando a tela de confirmação da landing (`landings/assessoria/`).
