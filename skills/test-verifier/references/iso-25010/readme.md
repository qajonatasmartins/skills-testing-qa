# ISO 25010 — Características de Qualidade de Produto

As oito características de qualidade da ISO/IEC 25010. Para o **test-verifier**, o que
importa é: *quais delas dá para verificar com testes unitários e integrados de
componente* — e quais pertencem a outros níveis de teste (carga, sistema, E2E) e devem
ser **delegadas ao outro fluxo** em vez de bloquear a tarefa aqui.

## Tabela de aplicabilidade (nível unitário / componente)

| Característica | Verificável aqui? | O que checar nos testes | Detalhe |
|---|---|---|---|
| **Functional Suitability** | ✅ Sim (núcleo) | Correção e completude: cada função/regra produz o resultado certo para entradas válidas e inválidas | [functional-suitability](functional-suitability/readme.md) |
| **Reliability** | ✅ Parcial | Caminhos de erro, tolerância a falha e recuperação testados (faultlessness, fault tolerance, recoverability) | [reliability](reliability/readme.md) |
| **Security** | ✅ Parcial | Lógica de authz/authn, validação de entrada, integridade — quando o código a contém (confidentiality, integrity, authenticity) | [security](security/readme.md) |
| **Maintainability** | ✅ Parcial (Testability) | Os próprios testes são modulares, legíveis e isolados (testability, modularity) | [maintainability](maintainability/readme.md) |
| **Performance Efficiency** | ⚠️ Delegar | Tempo/throughput/recursos → testes de carga/perf, outro fluxo | [performance-efficiency](performance-efficiency/readme.md) |
| **Compatibility** | ⚠️ Delegar | Co-existência/interoperabilidade → integração de sistema, outro fluxo | [compatibility](compatibility/readme.md) |
| **Interaction Capability** | ⚠️ Delegar | Usabilidade/UI → testes manuais/E2E, outro fluxo | [interaction-capability](interaction-capability/readme.md) |
| **Flexibility** | ⚠️ Delegar | Adaptabilidade/escalabilidade/instalação → outro fluxo | [flexibility](flexibility/readme.md) |
| **Safety** | ⚠️ Delegar (em geral) | Operação segura/fail-safe → análise de sistema; só checar aqui a lógica fail-safe unitária quando existir | [safety](safety/readme.md) |

## Como usar no veredito

1. Identifique o tipo de código sob teste.
2. Para cada característica marcada ✅, verifique se há testes cobrindo o aspecto
   correspondente. Ausência relevante → item de bloqueio (ex.: rota protegida sem teste
   de autorização → Security não coberta).
3. Para cada característica ⚠️ relevante ao código, **sinalize como lacuna do outro
   fluxo** na saída — não bloqueie a tarefa por ela.

Cada arquivo linkado traz a definição oficial da característica e suas
sub-características.

## Exemplos

Cenários escritos do que checar em cada característica (cobertura presente = OK, ausente =
bloqueio ou lacuna delegada): `examples/iso-25010/readme.md`.
