# Matriz de Risco e Quadrantes — PRISMA

Referência para as fases **Gather Individual Scores**, **Consensus Meeting** e posicionamento visual dos risk items.

## Índice

1. [Eixos da matriz](#eixos-da-matriz)
2. [Diagrama ASCII](#diagrama-ascii)
3. [Quadrantes e prioridade](#quadrantes-e-prioridade)
4. [Como calcular posição](#como-calcular-posição)
5. [Técnicas por quadrante](#técnicas-por-quadrante)
6. [Issue list e consenso](#issue-list-e-consenso)
7. [Revisão da matriz](#revisão-da-matriz)

---

## Eixos da matriz

| Eixo | Direção | Origem dos scores |
|------|---------|-------------------|
| **Impact** (Impacto) | Vertical ↑ | Soma (ou média ponderada) dos fatores de Impacto |
| **Likelihood** (Probabilidade) | Horizontal → | Soma (ou média ponderada) dos fatores de Likelihood |

Cada **risk item** é um ponto (ou bolha) na matriz 2D. Itens no mesmo quadrante competem por ordem de execução dentro do plano de teste.

---

## Diagrama ASCII

```
Impacto (soma fatores Impacto)
    ↑
    │  Q II                    Q I
    │  Should Test             Must Test
    │  (alto impacto,           (alto impacto,
    │   likelihood média)       alta likelihood)
    │         ┌─────────────────────────┐
    │         │                         │
    │  Q III  │                         │  Q I
    │  Could  │      MATRIZ             │
    │  Test   │                         │
    │         │                         │
    │  Q IV   │                         │
    │  Won't  │                         │
    │  Test   └─────────────────────────┘
    └────────────────────────────────────────→ Likelihood
              (soma fatores Likelihood)
```

**Linhas de corte:** defina limiares após ver a distribuição dos scores (ex.: terços ou mediana). Documente os limiares no relatório — não use valores mágicos sem explicar ao time.

---

## Quadrantes e prioridade

| Quadrante | Nome prático | Prioridade de teste | Ordem típica |
|-----------|--------------|---------------------|--------------|
| **I** | Must Test | Máxima — primeiro, mais profundo, testadores mais experientes | 1º |
| **II** | Should Test | Alta — cobertura sólida | 2º |
| **III** | Could Test | Média — cobertura básica se houver tempo | 3º |
| **IV** | Won't Test / Would Test | Mínima ou adiada — **documentar aceite explícito** | 4º ou fora do escopo |

### Critério de classificação (regra prática)

- **Q I:** Impacto alto **e** Likelihood alta.
- **Q II:** Impacto alto, Likelihood média/baixa **ou** Impacto médio com Likelihood alta.
- **Q III:** Ambos médios.
- **Q IV:** Ambos baixos — candidatos a não testar formalmente neste ciclo.

Ajuste fino na **Consensus Meeting** — common sense prevalece sobre rigidez mecânica.

---

## Como calcular posição

1. Para cada risk item, calcule **média por fator** (se ≥2 scores por fator).
2. **Some** as médias dos fatores de Impacto → `score_impacto`.
3. **Some** as médias dos fatores de Likelihood → `score_likelihood`.
4. Compare com limiares acordados para obter o quadrante.
5. Liste itens **por quadrante**, ordenados por `score_impacto + score_likelihood` decrescente dentro do Q I.

### Exemplo numérico (escala 1–3, 4 fatores Impacto, 5 Likelihood)

| ID | Σ Impacto (max 12) | Σ Likelihood (max 15) | Quadrante |
|----|--------------------|------------------------|-----------|
| RI-01 | 11 | 13 | I |
| RI-05 | 9 | 7 | II |
| RI-08 | 5 | 4 | III |
| RI-12 | 3 | 2 | IV |

---

## Técnicas por quadrante

Exemplos baseados em van Veenendaal — adapte ao contexto (mobile, API, web).

| Quadrante | Design de teste | Revisão / estático | Execução |
|-----------|-----------------|-------------------|----------|
| **I — Must Test** | Use cases com alternativas, decision tables, pairwise | Revisão externa de casos de teste e código crítico | Testadores experientes; re-test rigoroso |
| **II — Should Test** | Use cases fluxo básico, equivalence partitioning | Revisão por par | Cobertura sólida dos fluxos principais |
| **III — Could Test** | EP superficial, smoke | Checklist leve | Se sobrar tempo após Q I e II |
| **IV — Won't Test** | Nenhum formal ou smoke mínimo | — | Decisão documentada; aceite do stakeholder |

Detalhes de práticas (independência, regressão, exit criteria): [abordagem-diferenciada-teste.md](abordagem-diferenciada-teste.md).

---

## Issue list e consenso

Itens para a **Consensus Meeting** (issue list):

| Tipo | Critério | Ação |
|------|----------|------|
| **Outlier** | Min e max no mesmo fator divergem ≥2 níveis na escala | Debater assumptions; ajustar ou manter com nota |
| **Centro da matriz** | Scores médios em ambos eixos — quadrante ambíguo | Decisão explícita com negócio |
| **Violação de regras** | Blank, só 1 score, escala não usada | Corrigir antes de fechar matriz |
| **Ambiguidade de requisito** | Stakeholders discordam por requisito vago | Registrar como change request |
| **Surpresa** | Item tecnicamente simples com impacto alto (ou inverso) | Validar visibilidade e histórico |

Pergunta de fechamento: **"Esta matriz reflete o que vocês aceitariam levar para um go/no-go de release?"**

---

## Revisão da matriz

Reabra a matriz quando:

- Defeitos relevantes forem encontrados em produção ou teste (aumenta Likelihood local).
- Escopo mudar (novos risk items ou fatores).
- Pressão de tempo mudar (squeeze — repriorizar Q I apenas).
- Release parcial — itens Q IV podem subir se dependência crítica surgir.

No **modo revisão pós-defeitos**, recalcule Likelihood dos itens afetados e mova quadrantes; mantenha histórico (versão da matriz com data).
