# Exemplos — Qualidade ISO 25010 na verificação

Um arquivo por característica da norma, com cenários **escritos** (não código) do que o
verificador deve checar nos testes. Cada cenário segue o formato: **código sob teste →
o que a suíte deveria conter → item de veredito** (OK, bloqueio, ou lacuna delegada).

A definição oficial de cada característica está na reference correspondente em
`references/iso-25010/`. Aqui o foco é *como reconhecer cobertura presente ou ausente*.

## Verificáveis em nível unitário / componente (podem bloquear)

| Característica | O que os exemplos mostram | Arquivo |
|---|---|---|
| **Functional Suitability** | Correção/completude por entrada válida e inválida | [functional-suitability](functional-suitability/readme.md) |
| **Reliability** | Caminhos de erro, tolerância a falha e recuperação | [reliability](reliability/readme.md) |
| **Security** | Authz/authn, validação de entrada, integridade | [security](security/readme.md) |
| **Maintainability** | Testability: testes isolados, legíveis, sem dependência de ordem | [maintainability](maintainability/readme.md) |

## Delegadas a outro fluxo (sinalizar, não bloquear)

| Característica | Como sinalizar a lacuna | Arquivo |
|---|---|---|
| **Performance Efficiency** | Tempo/throughput/recursos → teste de carga/perf | [performance-efficiency](performance-efficiency/readme.md) |
| **Compatibility** | Co-existência/interoperabilidade → integração de sistema | [compatibility](compatibility/readme.md) |
| **Interaction Capability** | Usabilidade/UI → teste manual/E2E | [interaction-capability](interaction-capability/readme.md) |
| **Flexibility** | Adaptabilidade/instalação/escala → outro fluxo | [flexibility](flexibility/readme.md) |
| **Safety** | Perigo de sistema → análise de sistema; só a lógica fail-safe unitária se verifica aqui | [safety](safety/readme.md) |
