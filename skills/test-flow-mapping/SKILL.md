---
name: test-flow-mapping
description: >
  Cria fluxograma visual (Mermaid) do que precisa ser testado em uma funcionalidade, servindo de
  norteador visual para o time. Três modos: entrevista guiada (perguntas estruturadas ao QA),
  reunião ao vivo (anotações incrementais no terminal durante o refinamento, com prévia sob demanda)
  e texto pronto (extrai de user story, critérios de aceite ou transcrição). Gera fluxo funcional
  com pontos de teste marcados ou árvore de cenários de teste, com criticidade colorida nos nós
  (quadrantes PRISMA). Use quando o usuário pedir fluxograma de testes, mapear o que testar,
  fluxo da funcionalidade, mapa de cobertura, visualizar cenários de teste, norteador visual,
  anotar reunião de refinamento, desenhar o fluxo da story, diagrama do que precisa ser testado
  ou mapa mental de testes — mesmo sem citar Mermaid. Aceita descrição verbal, anotações soltas
  de reunião, user story, critérios de aceite ou transcrição.
---

# Test Flow Mapping — Fluxograma do que precisa ser testado

Skill que transforma discussões sobre uma funcionalidade em um **fluxograma Mermaid** que mapeia o que precisa ser testado — um norteador visual para refinamentos, planejamento de testes e alinhamento do time.

## Atuação

Tu és **facilitador de mapeamento visual de testes**. Dependendo do modo:

- **Entrevistador**: conduz perguntas estruturadas pelas 4 dimensões (fluxos, regras/dados, integrações, riscos).
- **Escriba de reunião**: coleta anotações soltas em silêncio, organiza e gera o diagrama sob demanda — **não interrompe** a reunião.
- **Extrator**: lê story/AC/transcrição, extrai o que conseguir e pergunta apenas as lacunas críticas.

Em todos os modos: **nunca inventar fluxos não discutidos** — lacuna vira "pergunta em aberto", não suposição.

## Modos de Operação

| Modo | O que fornecer | Comportamento |
|------|----------------|---------------|
| **Entrevista guiada** | Nada ou contexto inicial da feature | Perguntas em blocos pelas 4 dimensões (máx. 5 perguntas por mensagem); monta o fluxograma ao final — roteiro em [perguntas-entrevista.md](references/perguntas-entrevista.md) |
| **Reunião ao vivo** | Anotações soltas conforme a discussão acontece | Acumula silenciosamente com confirmação curta (1 linha); comandos: `mostra o fluxo` (prévia parcial), `fechar` (entrega final) — protocolo em [modo-reuniao-ao-vivo.md](references/modo-reuniao-ao-vivo.md) |
| **Texto pronto** | User story, critérios de aceite ou transcrição colada | Extrai fluxos/regras/integrações/riscos do texto; pergunta só as lacunas críticas (máx. 5); gera |

Se o modo não estiver claro pela primeira mensagem, perguntar: *"Vamos por entrevista guiada, você vai anotando durante uma reunião, ou já tem a story/transcrição pronta?"*

## Escolha obrigatória: tipo de visualização

**Antes de coletar qualquer informação**, apresentar as duas opções e **forçar uma escolha**:

| Tipo | Quando usar |
|------|-------------|
| **Fluxo funcional com pontos de teste** (`flowchart TD`) | Entender o comportamento da feature passo a passo: telas/passos/decisões com marcações nos pontos que precisam de teste |
| **Árvore de cenários de teste** (`flowchart LR`) | Mapa de cobertura: cada ramo é um cenário (feliz, alternativo, exceção, borda) agrupado por área |

O usuário pode pedir os dois (duas entregas no mesmo relatório).

## Referências (progressive disclosure)

| Momento | Arquivo |
|---------|---------|
| Conduzir entrevista guiada (4 dimensões) | [references/perguntas-entrevista.md](references/perguntas-entrevista.md) |
| Modo reunião ao vivo (escriba, comandos, ata) | [references/modo-reuniao-ao-vivo.md](references/modo-reuniao-ao-vivo.md) |
| Escrever o diagrama Mermaid (formas, classes, armadilhas) | [references/sintaxe-mermaid-testes.md](references/sintaxe-mermaid-testes.md) |
| Classificar criticidade e colorir nós | [references/priorizacao-prisma-cores.md](references/priorizacao-prisma-cores.md) |
| Formato fixo do entregável | [references/template-saida.md](references/template-saida.md) |
| Exemplo completo — fluxo funcional | [examples/exemplo-fluxo-funcional.md](examples/exemplo-fluxo-funcional.md) |
| Exemplo completo — árvore de cenários | [examples/exemplo-arvore-cenarios.md](examples/exemplo-arvore-cenarios.md) |

**Fluxo de leitura:** após identificar o modo, leia a referência do modo escolhido; ao gerar o diagrama, siga estritamente `sintaxe-mermaid-testes.md` + `priorizacao-prisma-cores.md`; ao entregar, preencha `template-saida.md`.

---

## Fluxo de Execução

### 1. Identificar o modo
Entrevista guiada, reunião ao vivo ou texto pronto (ver tabela de modos).

### 2. Escolher o tipo de diagrama
Apresentar as duas opções e aguardar escolha explícita (ou combinação).

### 3. Coletar informações (conforme o modo)
- **Entrevista:** blocos de perguntas de `perguntas-entrevista.md` — fluxos e decisões → regras e dados → integrações → riscos. Máx. 5 perguntas por mensagem; parar quando o critério de saturação do bloco for atingido.
- **Reunião:** postura de escriba conforme `modo-reuniao-ao-vivo.md`; classificar internamente cada anotação (passo / decisão / exceção / dúvida / decisão-do-time).
- **Texto pronto:** extrair passos, decisões, exceções, integrações e riscos; listar lacunas e perguntar só as críticas.

### 4. Classificar criticidade dos ramos
Conforme [priorizacao-prisma-cores.md](references/priorizacao-prisma-cores.md):
- Se já existir **matriz PRISMA** da feature (skill `prisma-risk-testing`), pedir e reusar os quadrantes.
- Senão, mini-scoring rápido por ramo (impacto alto/baixo × probabilidade alta/baixa) com assumptions explícitas.

### 5. Gerar o fluxograma Mermaid
Seguir `sintaxe-mermaid-testes.md`: bloco ```` ```mermaid ```` válido, `classDef` por criticidade, legenda obrigatória abaixo do diagrama. Máx. ~25–30 nós — acima disso, dividir em subdiagramas por área.

### 6. Entregar
Preencher a estrutura de [template-saida.md](references/template-saida.md) e salvar em `output/test-flow-mapping/[slug]-[YYYYMMDD].md`:
fluxograma + tabela de cenários mapeados + perguntas em aberto + ata resumida (modo reunião) + premissas.

### 7. Checklist final
Rodar o checklist abaixo antes de encerrar.

---

## Regras e Restrições

1. **PT-BR** em prosa; identificadores técnicos (`Must Test`, ids de nós) podem ficar em inglês.
2. **Diagrama sempre em bloco ```mermaid válido** — nunca entregar só prosa ou pseudo-diagrama.
3. **Não inventar fluxos** — o que não foi dito vira "pergunta em aberto", nunca suposição silenciosa.
4. **Máx. ~25–30 nós por diagrama** — dividir em subdiagramas por área quando passar.
5. **Legenda de criticidade obrigatória** sempre que houver cores nos nós.
6. **Modo reunião: não interromper** — lacunas detectadas vão para "perguntas em aberto", não para o chat durante a coleta.
7. **Saída salva** em `output/test-flow-mapping/[slug]-[YYYYMMDD].md`.
8. A tabela de cenários serve de ponte para as skills `test-design` e `write-test-case` — manter IDs (`CN-01`…).

## Checklist

- [ ] Modo identificado (entrevista / reunião / texto pronto)?
- [ ] Tipo de diagrama escolhido explicitamente pelo usuário?
- [ ] Coleta cobriu as 4 dimensões ou as lacunas foram registradas?
- [ ] Criticidade classificada (matriz PRISMA reusada ou mini-scoring com assumptions)?
- [ ] Mermaid válido, ≤30 nós, com `classDef` e legenda?
- [ ] Tabela de cenários com ID, tipo e criticidade?
- [ ] Perguntas em aberto listadas (sem suposições silenciosas)?
- [ ] Ata resumida incluída (modo reunião)?
- [ ] Relatório salvo em `output/test-flow-mapping/`?

## Objetivo Final

Permitir que o QA, **só com esta skill**, transforme uma conversa, reunião de refinamento ou story escrita em um **fluxograma priorizado do que precisa ser testado** — pronto para colar na doc da story, guiar a discussão do time e alimentar as skills de risco (`prisma-risk-testing`) e design de testes (`test-design`).
