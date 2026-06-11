# Test Design: [unidade / componente sob teste]

> Preenchido pela skill `test-design`. Escopo: testes unitários e integrados de
> componente. API/sistema/E2E ficam fora — listar em "Lacunas".

## Baseline da suíte existente

- Framework: [vitest / jest / pytest / go test / …]
- Estado antes: [N passando / M falhando]
- Já coberto (não duplicar): [resumo]

## Análise do código (formato)

- [ ] Faixa de valores com limites
- [ ] Entradas particionáveis em classes
- [ ] Regras de negócio (condições → ações)
- [ ] Estado interno / memória (ordem importa)
- [ ] Muitas variáveis independentes
- [ ] Variáveis com limites interdependentes
- [ ] Ramos/decisões a exercitar
- [ ] Ciclo de vida de variáveis
- [ ] Lógica booleana crítica

## Técnicas escolhidas

| Técnica | Por que (qual característica do código pediu) | Reference |
|---|---|---|
| [Boundary Value] | [faixa de idade 0–17] | `references/black-box-testing-techniques/boundary-value-testing/readme.md` |

## Casos derivados

| # | Entrada / cenário | Esperado | Técnica |
|---|---|---|---|
| 1 | [valor logo abaixo do limite] | [rejeitado] | Boundary Value |
| 2 | [valor sobre o limite] | [aceito] | Boundary Value |

## Testes escritos

| Arquivo | Casos | Comportamento coberto |
|---|---|---|
| `path/to/x.test.ts` | N | [...] |

## Resultado da suíte

[pass/fail — contagem após rodar]

## Lacunas

[O que ficou de fora e por quê — incluindo o que pertence a API/sistema/E2E (outro fluxo)]
