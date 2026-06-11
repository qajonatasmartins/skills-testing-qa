---
name: test-verifier
description: >
  Use SEMPRE que precisar VERIFICAR / VALIDAR a qualidade de uma suíte de testes já
  escrita — antes de declarar uma tarefa pronta, na fase QA do fluxo dev, ou quando
  alguém pergunta "os testes estão bons?", "esses testes seguem o padrão?", "essa
  suíte pode liberar?". Dispara com "verificar testes", "validar testes", "revisar a
  suíte", "gate de testes". NÃO use para PROJETAR ou escrever os casos de teste — isso
  é a skill `test-design`. Escopo: testes unitários e integrados de componente.
  Funciona em PT-BR e inglês.
---

# Skill: Test Verifier

Verifica testes **já escritos** e decide se a suíte pode ser considerada pronta. Não
projeta casos (isso é `test-design`) nem escreve testes — recebe a tarefa e os testes
implementados e responde uma pergunta: **essa suíte é boa o suficiente para liberar?**
A resposta é um veredito — **APROVADO** ou **BLOQUEADO** — com itens acionáveis quando
bloqueia.

Escopo: testes **unitários** e **integrados de componente**. Características de qualidade que só se verificam em outros níveis (carga, sistema, E2E) são sinalizadas como lacuna do outro fluxo, não bloqueiam aqui.

## As duas frentes de verificação

Cada frente tem uma reference própria. Leia a reference da frente antes de aplicá-la.
Cada reference termina com um link para o `examples/` correspondente — abra-o para ver
casos concretos do que checar antes de aplicar.

| Frente | O que verifica | Reference | Exemplos |
|---|---|---|---|
| **1. Padrão AAA** | Cada teste segue Arrange-Act-Assert em ordem estrita, um comportamento por teste | `references/aaa-pattern/readme.md` | `examples/aaa-pattern/readme.md` |
| **2. Qualidade ISO 25010** | Os testes cobrem as características de qualidade relevantes (correção, confiabilidade, segurança…) que são verificáveis em nível unitário/componente | `references/iso-25010/readme.md` | `examples/iso-25010/readme.md` |

## Processo

### Passo 1 — Reunir o material

- A **tarefa/demanda** (descrição, critérios de aceite, definição de cobertura do projeto).
- O **diff** (o que mudou) e os **testes** correspondentes.
- Rode a suíte de testes. Uma suíte que não passa já é BLOQUEADO — registre e siga.

### Passo 2 — Frente 1: Padrão AAA

Leia `references/aaa-pattern/readme.md` e cheque cada teste novo/alterado:

- ARRANGE → ACT (uma única chamada à unidade) → ASSERT, nesta ordem.
- Nenhuma declaração, atribuição ou chamada **depois** da primeira assertion.
- Um único comportamento por bloco `it`/`test`.

Cada violação vira um item de bloqueio com arquivo e linha.

### Passo 3 — Frente 2: Qualidade ISO 25010

Leia o índice `references/iso-25010/readme.md` e, para o tipo de código sob teste,
verifique se os testes cobrem as características **verificáveis neste nível**:

- **Functional Suitability** (correção/completude) — quase sempre aplicável.
- **Reliability** (tolerância a falha, recuperação) — há testes de caminho de erro?
- **Security** (confidencialidade, integridade, autorização) — há testes da lógica de
  authz/validação de entrada quando o código a contém?
- **Maintainability → Testability** — os próprios testes são legíveis e isolados?

As demais (Performance, Safety, Compatibility, Interaction Capability, Flexibility)
raramente se verificam em unitário/componente — quando relevantes ao código, **sinalize
como lacuna do outro fluxo**, não bloqueie por elas. O índice diz quais cobrir aqui e
quais delegar.

### Passo 4 — Veredito

Consolide as duas frentes num veredito único. **BLOQUEADO** se qualquer item crítico
falhar (suíte vermelha, comportamento alterado sem teste, característica de qualidade
verificável sem cobertura, violação de AAA). Caso contrário **APROVADO**, possivelmente
com observações menores.

## Formato de saída

```markdown
# Test Verification: [tarefa/componente]

## Veredito: APROVADO | BLOQUEADO

## Frente 1 — AAA
[OK | itens: arquivo:linha — violação]

## Frente 2 — Qualidade ISO 25010
- Verificadas: [Functional Suitability: ok, Reliability: …]
- Delegadas ao outro fluxo: [Performance, Safety… quando aplicável]

## Itens de bloqueio (se BLOQUEADO)
1. [ação concreta para destravar]
```

## Degradação graciosa

- **Suíte não executável:** se o projeto não tem runner ou os testes não rodam,
  verifique AAA e a presença de testes para o que mudou por leitura do código, e
  registre que a execução automatizada não pôde ser confirmada — não invente resultado.
- **Reference ilegível:** prossiga com o princípio geral da frente e avise qual
  reference faltou.
