# Template de Matriz e Entregáveis — PRISMA

Use este arquivo ao gerar o relatório final. Copie as seções abaixo e preencha com os dados da sessão.

## Índice

1. [Cabeçalho do relatório](#cabeçalho-do-relatório)
2. [Lista de risk items](#lista-de-risk-items)
3. [Tabela de scoring](#tabela-de-scoring)
4. [Matriz posicionada](#matriz-posicionada)
5. [Issue list](#issue-list)
6. [Abordagem diferenciada (resumo)](#abordagem-diferenciada-resumo)
7. [Resumo para stakeholders](#resumo-para-stakeholders)
8. [Export CSV](#export-csv)

---

## Cabeçalho do relatório

```markdown
# Análise PRISMA — [Nome do produto / épico / release]

**Data:** YYYY-MM-DD
**Modo:** planejamento-completo | rapido | revisao-pos-defeitos | upstream
**Contexto:** [URL épico ClickUp / MR / descrição]
**Escala:** 1–3 | 0,1,3,5,9
**Stakeholders:** [nomes e papéis]
**Facilitador:** [QA / agente]
```

Salvar em: `output/prisma/[slug]-[YYYYMMDD].md`

---

## Lista de risk items

Máximo recomendado: **30–35** itens. Agrupe requisitos/funcionalidades/atributos de qualidade em unidades testáveis.

```markdown
## Risk items

| ID | Descrição (one-liner) | Fonte |
|----|------------------------|-------|
| RI-01 | [O que pode falhar / o que testamos] | [AC épico / MR / doc] |
| RI-02 | ... | ... |
```

**Boas práticas de ID:**

- `RI-NN` sequencial.
- Descrição testável ("Pagamento PIX confirmação assíncrona"), não genérica ("Módulo PIX").

---

## Tabela de scoring

Uma linha por risk item; colunas por fator (abrevie cabeçalhos se necessário).

```markdown
## Scoring individual (médias por fator)

| ID | I: Área crítica | I: Visibilidade | I: Intensidade | I: Negócio | L: Complex. | L: Mudanças | ... | Σ Impacto | Σ Likelihood | Quadrante |
|----|-----------------|-----------------|----------------|------------|-------------|-------------|-----|-----------|--------------|-----------|
| RI-01 | 3 | 3 | 3 | 2 | 3 | 3 | ... | 11 | 14 | I |
```

Inclua subseção **Assumptions** por stakeholder ou consolidada:

```markdown
### Assumptions

- **Negócio (PO):** "PIX representa 50% do volume no cliente piloto."
- **Técnico (dev):** "Autenticação OAuth é código legado estável exceto o refresh token."
```

---

## Matriz posicionada

```markdown
## Matriz de risco

Limiares: Impacto ≥ [X] e Likelihood ≥ [Y] → Q I (ajustar conforme escala).

### Q I — Must Test
| ID | Descrição | Σ Impacto | Σ Likelihood |
|----|-----------|-----------|--------------|
| RI-01 | ... | 11 | 14 |

### Q II — Should Test
...

### Q III — Could Test
...

### Q IV — Won't Test (aceite documentado)
| ID | Descrição | Motivo do adiamento |
|----|-----------|---------------------|
| RI-12 | Tema escuro | Cosmético; sem tempo neste ciclo — aceite PO em DD/MM |
```

Opcional: diagrama ASCII de [matriz-risco-quadrantes.md](matriz-risco-quadrantes.md) com IDs plotados.

---

## Issue list

```markdown
## Issue list (consenso)

| ID | Tipo | Fator / tema | Situação | Resolução |
|----|------|--------------|----------|-----------|
| RI-03 | Outlier | Complexidade | Dev=1, QA=3 | Ajustado para 2 após revisar MR |
| RI-07 | Centro | Ambos eixos | Scores médios | Promovido a Q II por compliance |
| — | Requisito | AC pagamento parcial | Ambíguo | Change request #123 |
```

---

## Abordagem diferenciada (resumo)

```markdown
## Plano de abordagem diferenciada

| ID | Quadrante | Ordem exec. | Técnicas | Revisão | Executor | Exit criteria |
|----|-----------|-------------|----------|---------|----------|---------------|
| RI-01 | I | 1 | Decision table + E2E | Revisão externa CT | QA sênior | 100% casos P0; 0 defeitos abertos críticos |
| RI-05 | II | 2 | Use case básico + API | Par review | QA | 80% casos; sem blocker |
```

Detalhamento por quadrante: [abordagem-diferenciada-teste.md](abordagem-diferenciada-teste.md).

---

## Resumo para stakeholders

Linguagem de negócio — suporta decisão de release, **sem** jargão de técnica de teste.

```markdown
## Resumo para stakeholders

### O que mais importa testar
1. **[RI-01]** — [risco em linguagem de negócio]
2. **[RI-02]** — ...

### O que estamos conscientemente não testar agora
- **[RI-12]** — [motivo e aceite]

### Recomendação do tester (testemunha)
Com o tempo disponível ([N dias]), recomendamos focar em [N] áreas Must Test.
**Decisão de release** permanece com stakeholders; esta matriz informa trade-offs.

### Good enough testing (Bach) — checklist
- [ ] Benefícios da release serão entregues?
- [ ] Sem problemas que impeçam uso crítico?
- [ ] Benefícios superam problemas não críticos conhecidos?
- [ ] Adiar release custaria mais que liberar com riscos documentados?
```

---

## Export CSV

Estrutura para planilha (opcional):

```csv
id,descricao,fonte,sum_impacto,sum_likelihood,quadrante,ordem_exec,tecnicas,aceite_adiado
RI-01,"Pagamento PIX confirmação",clickup-86xxx,11,14,I,1,"decision table; E2E",false
RI-12,"Tema escuro PDV",figma,3,2,IV,,,true
```

Arquivo sugerido: `output/prisma/[slug]-[YYYYMMDD].csv`
