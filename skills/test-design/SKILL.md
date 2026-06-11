---
name: test-design
description: >
  Use SEMPRE que for criar, escrever, projetar, derivar ou aumentar a cobertura de
  testes unitários ou de testes integrados de componente (com ou sem mock), olhando
  para o código em vez de chutar casos. Dispara com "escrever testes", "criar testes",
  "cobrir essa função/componente", "casos de teste", "cobertura de testes", "TDD",
  "test design", ou na fase QA do fluxo dev. Cobre cenários que pedem técnicas como
  boundary value, equivalence class, decision table, state-transition, pairwise,
  domain analysis, control flow, data flow e MC/DC. NÃO use para testes de
  API/contrato, testes de sistema, testes E2E, carga ou performance — esses pertencem
  a outro fluxo. Funciona em PT-BR e inglês.
---

# Skill: Test Design

Projeta **quais casos de teste escrever** olhando para o código, em vez de inventar
asserts ad hoc. Você traduz a estrutura do código e os requisitos da tarefa em casos
de teste usando técnicas consagradas — cada técnica encontra uma classe diferente de
defeito. O objetivo não é "testar tudo", é escolher o conjunto mínimo de casos que
expõe os defeitos mais prováveis para aquele formato de código.

Esta skill **só projeta testes unitários e testes integrados de componente** (integração
entre componentes do código, com ou sem mock). Veja a guarda de escopo abaixo.

## Por que técnicas em vez de intuição

Cada técnica existe porque um tipo de bug se esconde num lugar específico:
comparações erradas vivem nos limites de faixa; lógica booleana complexa esconde
condições que nunca foram exercitadas; sistemas com memória quebram em transições
inválidas de estado. Escolher a técnica pelo formato do código é o que diferencia uma
suíte que "passa" de uma suíte que **encontra defeitos**. As `references/` explicam o
princípio de cada técnica e `examples/` mostram casos concretos — leia a da técnica
escolhida antes de derivar os casos; não confie só na memória.

## Guarda de escopo (leia primeiro)

Esta skill cobre **apenas**:

- **Testes unitários** — uma unidade isolada (função, método, classe, hook, reducer…).
- **Testes integrados de componente** — vários componentes do código integrados entre
  si, com mocks nas bordas externas (rede, banco, FS) ou sem mock quando barato.

Está **fora de escopo** (outro fluxo cuida): testes de API/contrato, de sistema, E2E,
de carga/performance, e testes manuais. Se a tarefa pedir só esses tipos, diga que está
fora do escopo desta skill e **pare** — não improvise. Se a tarefa misturar os dois,
projete a parte unitária/de componente e sinalize o resto como pendente do outro fluxo.

## Input

- A **tarefa** (descrição, critérios de aceite) e o **código alterado/sob teste**.
- **Antes de qualquer coisa**, rode a suíte de testes existente. Isso (1) estabelece um
  baseline verde/vermelho, (2) revela o framework e as convenções de teste do projeto
  (vitest, jest, pytest, go test…), e (3) mostra o que já está coberto, para você não
  duplicar. Reaproveite o framework e o estilo já presentes — nunca introduza um runner
  ou utilitário novo.

## Processo

### Passo 1 — Analisar o formato do código

Leia a unidade sob teste e identifique suas características — elas determinam a técnica.
Procure por:

- Entradas com **faixa** contínua ou discreta (idade, valor, comprimento, quantidade).
- Entradas que se **particionam** em classes válidas/inválidas tratadas igualmente.
- **Regras de negócio** com combinações de condições → ações (vários if/else, flags).
- **Estado interno / memória**: o resultado depende do que aconteceu antes (FSM, fluxo,
  carrinho, sessão, máquina de status).
- **Muitas variáveis independentes** cujo produto explode (config, flags, navegadores).
- **Variáveis com limites interdependentes** (o valor de uma muda o limite aceitável de
  outra).
- **Decisões/ramos** no código que precisam ser exercitados (cobertura de branch).
- **Ciclo de vida de variáveis** (definir/usar/destruir) com risco de uso antes de
  inicializar, ou código morto.
- **Lógica booleana crítica** (expressões com vários `&&`/`||`) em código de alta
  confiabilidade.

Uma unidade costuma ter mais de uma característica → combine técnicas.

### Passo 2 — Selecionar a(s) técnica(s)

Use esta tabela de roteamento. **Comece pelas técnicas de caixa-preta** (derivam do
comportamento/requisito, sobrevivem a refatorações) e use caixa-branca como complemento
para fechar lacunas de cobertura que o comportamento não revelou.

| Formato do código / entrada                                    | Técnica              | Referência                                                                   |
|----------------------------------------------------------------|----------------------|------------------------------------------------------------------------------|
| Faixa contínua ou discreta com limites                         | **Boundary Value**   | `references/black-box-testing-techniques/boundary-value-testing/readme.md`   |
| Entradas particionáveis em classes (válidas/inválidas)         | **Equivalence Class**| `references/black-box-testing-techniques/equivalence-class-testing/readme.md`|
| Combinações de condições → ações (regras de negócio)           | **Decision Table**   | `references/black-box-testing-techniques/decision-table-testing/readme.md`   |
| Estado/memória: ordem de eventos importa (FSM)                 | **State-Transition** | `references/black-box-testing-techniques/state-transition-testing/readme.md` |
| Muitas variáveis independentes (explosão combinatória)         | **Pairwise**         | `references/black-box-testing-techniques/pairwise-testing/readme.md`         |
| Múltiplas variáveis com limites interdependentes               | **Domain Analysis**  | `references/black-box-testing-techniques/domain-analysis-testing/readme.md`  |
| Garantir que cada ramo/decisão é exercitado                    | **Control Flow**     | `references/white-box-testing-techniques/control-flow-testing/readme.md`     |
| Ciclo de vida de variáveis; uso antes de definir, código morto | **Data Flow**        | `references/white-box-testing-techniques/data-flow-testing/readme.md`        |
| Lógica booleana crítica (validar testes de requisito)          | **MC/DC**            | `references/white-box-testing-techniques/mc-dc/readme.md`                    |

Combinações comuns: Decision Table **+** Boundary Value (quando uma condição é faixa
numérica); Equivalence Class **+** Boundary Value (testa o representante da classe e os
limites entre classes); qualquer caixa-preta **+** Control Flow para confirmar que os
casos derivados cobrem todos os ramos.

### Passo 3 — Ler a reference (e o example) da técnica

Leia o `readme.md` da reference da técnica escolhida — ele traz princípio, quando usar,
quando **não** usar, e o passo a passo. Cada reference termina com um link para o
`examples/.../readme.md` correspondente: abra-o para ver casos concretos antes de
derivar os seus. Não pule este passo: o valor da técnica está nos detalhes (ex.: usar
sempre a tabela de decisão **não-colapsada**, ou os três pontos do boundary).

### Passo 4 — Derivar os casos de teste

Aplique a mecânica da técnica para listar os casos **antes** de codar:

- **Boundary Value:** para cada limite, 3 pontos — sobre, logo abaixo, logo acima (na
  unidade do valor: idade 15/16/17; dinheiro $4.99/$5.00/$5.01).
- **Equivalence Class:** um representante por classe válida e por classe inválida.
- **Decision Table:** uma coluna (= um caso) por regra da tabela **não-colapsada**;
  para condições de faixa, escolha valores reais que satisfaçam cada faixa.
- **State-Transition:** transições válidas **e** eventos inválidos em cada estado.
- **Pairwise:** cobrir todos os pares de valores das variáveis (não o produto completo).
- **Domain Analysis:** pontos on/off/in/out para cada fronteira interdependente.
- **Control Flow / Data Flow / MC/DC:** derive caminhos/condições conforme a reference e
  use-os para **conferir** que os casos de caixa-preta cobrem o que falta.

Liste cada caso com: entrada → saída/efeito esperado → técnica que o originou. Esse é o
"plano de testes" — pode usar `assets/test-plan-template.md` para estruturá-lo.

### Passo 5 — Escrever os testes em AAA estrito e rodar

Escreva cada caso seguindo **Arrange-Act-Assert em ordem estrita**

- **ARRANGE:** declare variáveis, monte entradas e mocks.
- **ACT:** chame a unidade sob teste — exatamente **uma vez**.
- **ASSERT:** todas as expectativas. **Nenhuma** declaração, atribuição ou chamada
  depois da primeira assertion.
- **Um comportamento por teste.** Nomeie o teste pelo comportamento, não pela função.
- Use só APIs padrão do framework do projeto; mock apenas as bordas externas.

Depois **rode os testes** e ajuste até passarem. Um teste que não passa não é um teste.
Antes de concluir, faça a verificação AAA: nada após o primeiro `expect`, sem múltiplos
comportamentos no mesmo bloco, ACT chamado uma única vez — reescreva o que violar.

## Formato de saída

```markdown
# Test Design: [unidade/componente]

## Técnicas aplicadas
- [Técnica] — [por que esse formato de código pediu essa técnica]

## Casos derivados
| # | Entrada / cenário | Esperado | Técnica |
|---|---|---|---|
| 1 | ... | ... | Boundary Value |

## Testes escritos
| Arquivo | Casos | Comportamento coberto |
|---|---|---|
| `path/to/x.test.ts` | N | ... |

## Resultado da suíte
[pass/fail — contagem]

## Lacunas
[O que ficou de fora e por quê — incl. o que pertence a API/sistema/E2E (outro fluxo)]
```

## Degradação graciosa

- **Sem framework de teste:** se o projeto não tem runner/teste algum, faça o design dos
  casos (passos 1–4) mesmo assim e sinalize que a execução automatizada não está
  disponível — não invente um runner.
- **Reference ilegível:** se um `readme.md` de reference não puder ser lido, prossiga com
  o princípio geral da técnica e avise qual reference faltou.
