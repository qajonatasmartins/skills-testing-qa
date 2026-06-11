# Exemplos — Functional Suitability (verificável aqui)

Núcleo do verificador: a suíte prova que cada função/regra produz o resultado certo para
entradas **válidas e inválidas**, cobrindo todas as tarefas especificadas (completeness)
com resultado exato (correctness) e adequado ao objetivo (appropriateness).

## Cenário 1 — Regra de desconto por faixa

**Código sob teste:** `calcularDesconto(valor)` aplica 0% até R$100, 10% de R$100,01 a
R$500, 15% acima de R$500.

**O que a suíte deveria conter:** um teste por faixa (ex.: R$50, R$300, R$800) e nos
limites entre elas (R$100, R$100,01, R$500, R$500,01).

**Veredito:** se só existe teste para R$300 (uma faixa), há regra de negócio não coberta.
`discount.test.ts — faltam casos para as faixas 0% e 15% e os limites — completude
funcional incompleta: adicionar um caso por faixa e por limite.`

## Cenário 2 — Entrada inválida sem cobertura

**Código sob teste:** `criarUsuario(dados)` rejeita e-mail malformado e idade negativa.

**O que a suíte deveria conter:** casos de entrada válida **e** casos inválidos provando
a rejeição (e-mail sem `@`, idade -1).

**Veredito (BLOQUEADO):** se só o caminho feliz é testado,
`user.test.ts — só entradas válidas testadas — correção funcional não verificada para
entradas inválidas: adicionar casos de e-mail malformado e idade negativa.`

## Cenário 3 — OK

Toda regra do critério de aceite tem ao menos um teste de entrada válida e um de inválida,
com saída exata. **Veredito:** Functional Suitability coberta.
