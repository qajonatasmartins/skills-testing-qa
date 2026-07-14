# Priorização — Quadrantes PRISMA → Cores no Diagrama

A criticidade dos ramos/cenários usa os quadrantes da skill `prisma-risk-testing` (Impacto × Likelihood).

## Mapeamento quadrante → classe Mermaid

| Quadrante | Significado | Classe | Cor | Legenda |
|-----------|-------------|--------|-----|---------|
| Q I — Must Test | impacto alto × probabilidade alta | `q1` | `#d94f4f` (vermelho) | 🟥 |
| Q II — Should Test | impacto alto × probabilidade baixa | `q2` | `#e8a33d` (laranja) | 🟧 |
| Q III — Could Test | impacto baixo × probabilidade alta | `q3` | `#e8d44d` (amarelo) | 🟨 |
| Q IV — Won't Test | impacto baixo × probabilidade baixa | `q4` | `#b0b0b0` (cinza) | ⬜ |

Nós sem classificação (fluxo puramente estrutural, ex.: nó raiz da árvore) ficam sem classe.

## Fonte da classificação — nesta ordem

### 1. Matriz PRISMA existente (preferencial)
Perguntar: *"Já rodou a análise PRISMA dessa feature? Se sim, me passa a matriz."*
Se existir, mapear cada ramo/cenário ao risk item correspondente (`RI-xx`) e herdar o quadrante. Registrar o vínculo na tabela de cenários (coluna Criticidade: `Q I (RI-03)`).

### 2. Mini-scoring rápido (fallback)
Sem matriz, classificar cada ramo principal com duas perguntas binárias:

- **Impacto**: se isso quebrar em produção, dói muito (dinheiro, dado, usuário travado, imagem)? → alto/baixo
- **Probabilidade**: é código novo, complexo, integrado ou com histórico de bug? → alta/baixa

Combinação → quadrante (tabela acima). **Sempre** registrar as assumptions na seção "Premissas assumidas" do relatório, ex.:

> - CN-03 marcado Q I assumindo que bloqueio indevido de conta impede o uso do produto (não confirmado com negócio).

### 3. Sem informação nenhuma
Se o usuário não souber responder nem o mini-scoring, **não colorir** o diagrama e registrar como pergunta em aberto: *"classificar criticidade dos ramos com o time"*. Nunca inventar quadrante.

## Análise completa

Este mapeamento é **leve** — para análise de risco formal (fatores, scoring com stakeholders, abordagem diferenciada por quadrante), direcionar o usuário à skill `prisma-risk-testing`. O fluxograma desta skill pode servir de entrada para a identificação dos risk items lá.
