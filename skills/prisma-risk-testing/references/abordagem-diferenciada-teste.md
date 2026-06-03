# Abordagem Diferenciada de Teste — PRISMA

Referência para a fase **Define Differentiated Test Approach**: transformar a matriz em plano de teste executável.

## Índice

1. [Princípio](#princípio)
2. [Tabela consolidada por quadrante](#tabela-consolidada-por-quadrante)
3. [Q I — Must Test](#q-i--must-test)
4. [Q II — Should Test](#q-ii--should-test)
5. [Q III — Could Test](#q-iii--could-test)
6. [Q IV — Won't Test](#q-iv--wont-test)
7. [Plano de execução priorizado](#plano-de-execução-priorizado)
8. [Good enough testing](#good-enough-testing)
9. [Monitoramento e reporting baseado em risco](#monitoramento-e-reporting-baseado-em-risco)

---

## Princípio

A matriz responde **onde** focar; a abordagem diferenciada responde **como** e **quanto** testar cada área. Áreas de maior risco recebem:

- Mais profundidade de design de teste.
- Mais revisão (estático e de casos).
- Maior independência (quem escreve ≠ quem executa).
- Exit criteria mais rígidos.

---

## Tabela consolidada por quadrante

| Dimensão | Q I Must | Q II Should | Q III Could | Q IV Won't |
|----------|----------|-------------|-------------|------------|
| **Prioridade execução** | 1º | 2º | 3º | Fora ou smoke |
| **Design de teste** | Use case + alternativas, decision tables, pairwise | Use case básico, EP | Smoke, EP superficial | Nenhum ou documentado N/A |
| **Static testing / review** | Revisão formal de requisitos + CT + código crítico | Revisão por par | Checklist | — |
| **Independência** | Autor do CT ≠ executor; idealmente ≠ dev | Par review | Mesmo executor OK | — |
| **Re-test** | Obrigatório após fix em área Q I | Após fix de defeitos médios+ | Selecionado | — |
| **Regressão** | Suite regressão focada + áreas adjacentes | Regressão do módulo | Smoke geral | — |
| **% casos executados (exit)** | 100% dos casos do item | ≥80% | O que couber | 0% formal |
| **Defeitos pendentes (exit)** | 0 críticos/altos abertos | 0 críticos; altos com workaround aceito | Documentados | Aceite explícito |
| **Perfil do testador** | Mais experiente na área | QA pleno | Qualquer | — |

---

## Q I — Must Test

**Objetivo:** máxima confiança nas áreas que mais combinam impacto e probabilidade de falha.

### Práticas recomendadas

1. **Kick-off de test design** — alinhar escopo dos casos com PO antes de executar.
2. **Técnicas formais** — decision tables para regras; use cases com fluxos alternativos e exceções.
3. **Dados de teste realistas** — volumes e tipos próximos de produção (sem dados sensíveis reais).
4. **Re-test dedicado** — após cada fix, reexecutar casos do item + adjacentes.
5. **Regressão direcionada** — incluir interfaces upstream/downstream do risk item.

### Exit criteria (exemplo)

- Todos os casos de teste do risk item executados.
- Cobertura dos critérios de aceite do épico relacionados ao item.
- Nenhum defeito severidade crítica/alta aberto sem aceite formal.
- Evidências anexadas (logs, prints, vídeo) para fluxos financeiros ou compliance.

---

## Q II — Should Test

**Objetivo:** cobertura sólida sem o custo total de Q I.

### Práticas recomendadas

1. Use cases do **fluxo feliz** + principais exceções.
2. Equivalence partitioning para inputs variados.
3. Revisão por par dos casos de teste (não precisa revisão externa).
4. Regressão do **módulo** após mudanças.

### Exit criteria (exemplo)

- ≥80% dos casos planejados executados.
- Defeitos críticos zerados; altos com plano de correção ou aceite.

---

## Q III — Could Test

**Objetivo:** cobertura básica se sobrar capacidade após Q I e II.

### Práticas recomendadas

1. Smoke test manual ou automatizado curto.
2. EP superficial nos inputs mais óbvios.
3. Executar apenas se squeeze não consumiu todo o tempo.

### Exit criteria (exemplo)

- Smoke passou OU item explicitamente não executado com motivo no relatório.

---

## Q IV — Won't Test

**Objetivo:** transparência — o que **não** será testado e por quê.

### Práticas obrigatórias

1. **Registro escrito** no relatório e, se aplicável, comentário no ClickUp/épico.
2. **Aceite do stakeholder** de negócio (PO ou equivalente) — QA não decide sozinho adiar Q IV de item que negócio considera crítico.
3. Reavaliar se dependência de Q I surgir (ex.: tema "cosmético" que quebra layout de botão de pagamento).

---

## Plano de execução priorizado

Monte a fila de execução nesta ordem:

1. Todos os **Q I**, ordenados por `Σ Impacto + Σ Likelihood`.
2. **Q II** na mesma lógica.
3. **Q III** se `tempo_restante > 0`.
4. **Q IV** — apenas documentar; não planejar execução.

### Modo squeeze (release amanhã)

- Recalcule apenas itens tocados pela release.
- Limite escopo a **Q I** + Q II críticos para negócio.
- Converta Q III em smoke único por módulo.
- Atualize resumo para stakeholders com riscos **aceitos** explicitamente.

### Modo rápido (solo QA)

- Documente no relatório: "Papéis negócio e técnico simulados pelo QA com fatores default."
- Use assumptions explícitas para cada score inferido.
- Issue list pode ser "ressalvas" em vez de reunião formal.

---

## Good enough testing

Critérios de James Bach — o tester **informa**; stakeholders **decidem** release.

| Pergunta | Se "não" → ação |
|----------|-----------------|
| Os benefícios desta release serão entregues? | Reavaliar escopo da release |
| Há problemas que impedem uso do fluxo crítico? | Bloquear ou rebaixar Q I não testado |
| Benefícios superam problemas não críticos conhecidos? | Documentar na issue list |
| Adiar custa mais que liberar com riscos documentados? | Incluir no resumo para stakeholders |

O QA é **testemunha especialista**, não juiz único de "bom o suficiente".

---

## Monitoramento e reporting baseado em risco

Durante a execução:

| Métrica | Uso |
|---------|-----|
| % casos Q I executados | Indicador principal de prontidão |
| Defeitos por quadrante | Alta densidade em Q I pode elevar Likelihood de itens adjacentes |
| Tempo gasto vs planejado | Sinal de squeeze — cortar Q III antes de Q I |
| Mudanças de escopo | Gatilho para revisar matriz |

Ao final:

- Anexar matriz final + abordagem ao relatório de teste da release.
- Se defeitos graves surgirem em Q IV adiado, **reabrir** matriz (modo revisão pós-defeitos).
