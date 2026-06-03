---
name: prisma-risk-testing
description: Conduz análise de risco de produto com o método PRISMA (Product Risk Management) de van Veenendaal, gerando matriz de risco (Impacto x Likelihood), classificação em quadrantes e abordagem diferenciada de teste por prioridade. Use quando o usuário mencionar PRISMA, matriz de risco, teste baseado em risco, risk based testing, product risk, priorizar testes, impacto e probabilidade, quadrante must test, abordagem diferenciada, o que testar primeiro com pouco tempo, good enough testing ou squeeze no teste — mesmo sem citar PRISMA explicitamente. Aceita os seguintes contextos: épico, épico + tarefa, repositório, função específica ou combinação deles.
---

# PRISMA — Teste Baseado em Risco de Produto

Skill que aplica o método **PRISMA** (*Product RISk MAnagement*, Erik van Veenendaal) para priorizar testes com recursos limitados: testar primeiro o que **mais importa** e onde há **maior chance de defeito**.

> **Não confundir** com PRISMA de revisões sistemáticas em saúde (Preferred Reporting Items for Systematic Reviews). Esta skill trata de **gestão de risco de produto** e *Practical Risk-Based Testing*.

**Foco:** risco de **produto** (impacto no negócio/usuário × probabilidade de defeito), não de projeto (prazo/orçamento — podem complementar, mas não substituem).

## Atuação

Você é **facilitador de análise de risco de produto** e **advisor de test manager**. Conduz o processo em seis fases, coleta evidências e produz a matriz com abordagem diferenciada.

**Papel do tester:** testemunha especialista — informa riscos e evidências; **não** decide sozinho se o produto está "bom o suficiente" para release. Stakeholders decidem com base na matriz, no resumo e nos critérios de *good enough testing*.

## Contextos de Entrada

Aceita os seguintes contextos: épico, épico + tarefa, repositório, função específica ou combinação deles.

| Modo | O que fornecer | Comportamento |
|------|----------------|---------------|
| **Planejamento completo** | Requisitos, épico ClickUp, arquitetura, histórico de defeitos | 6 fases com facilitação; solicitar stakeholders antes do scoring |
| **Rápido / solo QA** | Descrição de feature, MR diff, lista de mudanças | QA simula papéis negócio + técnico com fatores default; matriz + abordagem com ressalvas documentadas |
| **Revisão pós-defeitos** | Matriz anterior + defeitos encontrados | Recalcula Likelihood; ajusta quadrantes; versiona matriz |
| **Integração upstream** | Story ClickUp + critérios de aceite | Risk items agrupados por AC; pode cruzar com `upstream-refinement-strategy` |

### Quando perguntar vs inferir

| Situação | Ação |
|----------|------|
| Stakeholders não informados (modo completo) | Perguntar nomes/papéis para Impacto e Likelihood |
| Escala de scoring não definida | Sugerir 1–3; confirmar se release crítica (0,1,3,5,9) |
| Fatores customizados da empresa | Perguntar; senão usar fatores default das referências |
| Modo rápido / squeeze | Inferir scores com **assumptions** explícitas; marcar issue list como "ressalvas" |
| Documentos de entrada ausentes | Pedir épico, MR ou lista mínima de funcionalidades — não inventar escopo |

## Referências (progressive disclosure)

| Momento | Arquivo |
|---------|---------|
| Definir fatores, escalas, scoring | [references/fatores-impacto-likelihood.md](references/fatores-impacto-likelihood.md) |
| Posicionar na matriz, quadrantes | [references/matriz-risco-quadrantes.md](references/matriz-risco-quadrantes.md) |
| Formato do relatório e CSV | [references/template-matriz-risco.md](references/template-matriz-risco.md) |
| Plano de teste por quadrante | [references/abordagem-diferenciada-teste.md](references/abordagem-diferenciada-teste.md) |

**Fluxo de leitura:** após identificar o modo, leia `fatores-impacto-likelihood.md` na Planning; ao calcular posições, `matriz-risco-quadrantes.md`; ao fechar, `abordagem-diferenciada-teste.md` + preencha `template-matriz-risco.md`.

---

## Fluxo de Execução (6 fases)

### Fase 1 — Planning

1. **Coletar documentos de entrada** conforme contexto:
   - Épico/task ClickUp (MCP se disponível): critérios de aceite, descrição.
   - MR GitLab: arquivos alterados, módulos tocados.
   - Histórico de defeitos, arquitetura, release notes.
2. **Identificar risk items** (máx. **30–35**):
   - Agrupar requisitos, funcionalidades ou atributos de qualidade em unidades testáveis.
   - ID único (`RI-01`…), descrição one-liner, link à fonte.
3. **Definir fatores** de Impacto e Likelihood — ver [fatores-impacto-likelihood.md](references/fatores-impacto-likelihood.md).
4. **Definir escala** (1–3 ou 0,1,3,5,9) e pesos opcionais.
5. **Mapear stakeholders** — mínimo 2 pessoas por fator; negócio → Impacto; técnico → Likelihood.

**Saída parcial:** lista de risk items + fatores + escala + lista de stakeholders.

### Fase 2 — Kick-off (recomendado no modo completo)

- Explicar processo, matriz, papéis, prazos e ferramenta (markdown/planilha).
- Alinhar expectativa: **não** é testar tudo igual; Q IV pode ficar de fora com aceite.
- Pular ou resumir em 2–3 linhas no modo rápido/squeeze.

### Fase 3 — Individual Preparation

- Cada stakeholder pontua seus fatores por risk item (valores relativos entre itens).
- **Documentar assumptions** por pessoa — essenciais para o consenso.
- Modo rápido: QA preenche duas colunas lógicas (negócio / técnico) com premissas explícitas.

### Fase 4 — Gather Individual Scores

1. **Entry check:** regras cumpridas, ≥2 scores por fator (ou modo rápido documentado), sem blanks.
2. Calcular **médias por fator**; somar fatores de Impacto e de Likelihood separadamente.
3. Posicionar cada risk item na **matriz 2D** — ver [matriz-risco-quadrantes.md](references/matriz-risco-quadrantes.md).
4. Classificar quadrantes: I Must Test, II Should, III Could, IV Won't Test.

**Saída parcial:** tabela de scoring + matriz por quadrante.

### Fase 5 — Consensus Meeting

- Montar **issue list**: outliers (min vs max), itens no centro, violações de regras, requisitos ambíguos.
- Ajustar scores finais; validar: "A matriz faz sentido para vocês?"
- Common sense prevalece sobre mecânica rígida.
- Ambiguidade de requisito → registrar como change request.
- Modo rápido: seção **Ressalvas** em vez de reunião.

### Fase 6 — Define Differentiated Test Approach

1. **Priorizar** ordem de execução (Q I primeiro, por soma de scores).
2. Por quadrante, definir profundidade — ver [abordagem-diferenciada-teste.md](references/abordagem-diferenciada-teste.md):
   - Técnicas de design de teste.
   - Revisões / static testing.
   - Independência (autor ≠ executor).
   - Re-test e regressão.
   - Exit criteria (cobertura, % casos, defeitos pendentes).
3. Montar **resumo para stakeholders** e critérios de *good enough testing*.

**Saída final:** relatório completo salvo em `output/prisma/[slug]-[YYYYMMDD].md` (e CSV opcional).

---

## Entregáveis Obrigatórios

A skill produz **todos** os itens abaixo (estrutura em [template-matriz-risco.md](references/template-matriz-risco.md)):

1. **Lista de risk items** (tabela markdown).
2. **Matriz de risco** (por quadrante + scores).
3. **Issue list** ou ressalvas (modo rápido).
4. **Plano de abordagem diferenciada** (por item/quadrante).
5. **Resumo para stakeholders** (linguagem de negócio).
6. **Opcional:** CSV em `output/prisma/[slug]-[YYYYMMDD].csv`.

---

## Template de Saída (resumo)

Use a estrutura completa de [references/template-matriz-risco.md](references/template-matriz-risco.md). Seções mínimas no relatório:

```markdown
# Análise PRISMA — [título]

## Risk items
| ID | Descrição | Fonte |

## Scoring e assumptions
[tabela + bullets de premissas]

## Matriz por quadrante
### Q I — Must Test
### Q II — Should Test
...

## Issue list / Ressalvas

## Abordagem diferenciada
| ID | Quadrante | Ordem | Técnicas | Exit criteria |

## Resumo para stakeholders
[top 3 riscos, itens adiados, recomendação testemunha]

## Good enough testing
[checklist Bach]
```

---

## Integração com Outras Skills

| Skill | Quando cruzar |
|-------|----------------|
| [upstream-refinement-strategy](../upstream-refinement-strategy/SKILL.md) | Risk items = grupos de AC; PRISMA prioriza *o que* testar; upstream detalha *como* por camada |
| [upstream-pre-refinement-brainstorm](../upstream-pre-refinement-brainstorm/SKILL.md) | Riscos de negócio do brainstorm alimentam fatores de Impacto |
| [gitlab-merge-request-analysis](../gitlab-merge-request-analysis/SKILL.md) | MR fornece módulos alterados → fatores Mudanças, Complexidade, Interfaces |
| [mr-test-coverage-strategy](../mr-test-coverage-strategy/SKILL.md) | Após matriz Q I: cruzar cobertura existente no ClickUp |
| [heuristic-guide](../heuristic-guide/SKILL.md) | **Opcional:** para itens Q I/II, sugerir heurísticas (VADER, FAILURE, CRUD) no plano de casos — não substituir PRISMA |
| [release-test-strategy](../release-test-strategy/SKILL.md) | Release semanal: usar PRISMA para priorizar dentro da estratégia de release |

**Ordem sugerida upstream:** brainstorm → refinement strategy → **prisma-risk-testing** → criação de CTs.

---

## Modo Squeeze (release iminente)

Quando o usuário indicar pouco tempo ou release amanhã:

1. Reduzir risk items aos **módulos tocados** na release.
2. Executar fases 1, 4, 6 de forma compacta (pular kick-off e consenso formal).
3. Entregar apenas **Q I** + Q II críticos com ordem de execução do dia.
4. Listar explicitamente riscos **aceitos** (Q III/IV) no resumo para stakeholders.
5. Não prometer cobertura total — documentar trade-offs.

---

## Regras e Restrições

1. **Product risk primeiro** — não substituir matriz por lista de riscos de projeto sem scores.
2. **Máximo ~35 risk items** — se houver mais, agrupar ou fazer duas sessões.
3. **Q IV exige aceite documentado** — nunca silenciar áreas não testadas.
4. **Revisitar matriz** quando defeitos relevantes ou escopo mudarem.
5. **Sem scripts obrigatórios** — planilha é opcional; markdown é o entregável padrão.
6. **PT-BR** em prosa; identificadores (`RI-01`, `Must Test`) podem permanecer em inglês técnico.
7. **Não copiar** texto integral de materiais externos — sintetizar em instruções acionáveis.

---

## Checklist

- [ ] Modo de operação identificado (completo / rápido / revisão / upstream / squeeze)?
- [ ] Documentos de entrada coletados ou limitações declaradas?
- [ ] Risk items definidos (≤35) com ID e fonte?
- [ ] Fatores e escala documentados (referência lida)?
- [ ] Stakeholders mapeados ou modo rápido com assumptions?
- [ ] Scoring sem blanks; faixa da escala utilizada?
- [ ] Matriz com quadrantes e limiares explicados?
- [ ] Issue list / ressalvas tratadas?
- [ ] Abordagem diferenciada por quadrante/item?
- [ ] Resumo para stakeholders + good enough testing?
- [ ] Relatório salvo em `output/prisma/`?
- [ ] Distinção clara: tester informa, stakeholder decide release?

---

## Objetivo Final

Permitir que o QA, **só com esta skill**, vá de artefato de entrada (épico, MR, feature) até **matriz de risco priorizada** e **plano de teste diferenciado**, sem reler van Veenendaal — aplicando as seis fases de forma acionável, com transparência sobre o que não será testado e suporte informado à decisão de release.
