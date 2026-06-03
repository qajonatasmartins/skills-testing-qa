# Fatores de Impacto e Likelihood — PRISMA

Referência detalhada para as fases **Planning** e **Individual Preparation** do método PRISMA (Product Risk Management, van Veenendaal).

## Índice

1. [Conceito de risco](#conceito-de-risco)
2. [Fatores de Impacto](#fatores-de-impacto)
3. [Fatores de Likelihood](#fatores-de-likelihood)
4. [Escalas de pontuação](#escalas-de-pontuação)
5. [Pesos opcionais](#pesos-opcionais)
6. [Regras de scoring](#regras-de-scoring)
7. [Stakeholders por fator](#stakeholders-por-fator)
8. [Exemplos: scoring bom vs ruim](#exemplos-scoring-bom-vs-ruim)

---

## Conceito de risco

**Risco de produto** = combinação de:

- **Impacto** — consequência de uma falha para negócio, usuário ou operação (avaliado por stakeholders de negócio).
- **Likelihood** — probabilidade de defeitos na área (avaliado por stakeholders técnicos).

Não confundir com riscos de **projeto** (prazo, orçamento, recursos). Riscos de projeto podem complementar a análise, mas o foco do PRISMA é **product risk**.

---

## Fatores de Impacto

Avaliados por representantes de **negócio** (PO, suporte, operações, compliance).

| Fator | O que mede | Escala sugerida (exemplos) |
|-------|------------|----------------------------|
| **Área crítica / custo da falha** | Gravidade se a falha ocorrer | **Catastrófico** — para sistema, negócio ou vidas · **Danoso** — perda ou corrupção de dados · **Impedimento** — workaround difícil ou inexistente · **Irritante** — cosmético, baixo impacto operacional |
| **Visibilidade** | Quem vê a falha e tolerância do usuário | **Externa** (cliente final, mídia) vs **interna** (só operador/backoffice) · Usuário leigo (baixa tolerância) vs operador experiente |
| **Intensidade de uso** | Com que frequência a funcionalidade é usada | **Inevitável** (toda sessão) → **Frequente** → **Ocasional** → **Raro** (ainda testar falhas críticas em áreas raras) |
| **Importância de negócio** | Valor estratégico, compliance, sanções | Selling points, regulatório (PCI, LGPD), multas, custo de retrabalho, SLA contratual |

### Perguntas-guia por fator (Impacto)

- **Área crítica:** "Se falhar em produção na sexta à noite, o que acontece amanhã de manhã?"
- **Visibilidade:** "O cliente final vê isso ou só o operador do PDV?"
- **Intensidade:** "Quantas vendas por dia passam por este fluxo?"
- **Negócio:** "Perdemos contrato, multa ou reputação se isso quebrar?"

---

## Fatores de Likelihood

Avaliados por representantes **técnicos** (arquiteto, dev sênior, QA com histórico da área).

| Fator | Indicadores | Sinal de alto likelihood |
|-------|-------------|--------------------------|
| **Complexidade** | Lógica, ramificações, tamanho do módulo | Muitas regras, estados, integrações na mesma função |
| **Mudanças** | Volume de alterações recentes (CM, MRs) | Área muito tocada nesta release ou sprint |
| **Tecnologia/métodos novos** | Stack ou padrão inédito no time | Primeira vez com lib, protocolo ou padrão |
| **Pessoas** | Quantidade e senioridade envolvidas | Muitos autores, terceiros sem follow-up, rotatividade |
| **Pressão de tempo** | Atalhos por deadline | Overtime, menos revisão, "só mergear" |
| **Otimização** | Trade-off performance vs robustez | Código otimizado à custa de validações |
| **Histórico de defeitos** | Densidade em reviews/testes anteriores | Defeitos tendem a agrupar na mesma área |
| **Dispersão geográfica** | Times distantes no mesmo código | Handoff fraco, fusos diferentes |
| **Novo vs reúso** | Greenfield vs componente maduro | Código novo sem histórico de produção |
| **Interfaces** | Acoplamento interno/externo | Muitas APIs, filas, callbacks entre módulos |
| **Tamanho** | Tamanho do componente | Arquivo/módulo grande demais para visão holística |

**Atenção — inversão de sinal:** times muito experientes em área complexa podem reduzir likelihood real; não pontuar "alto" só porque a área é complexa — considerar competência do time.

---

## Escalas de pontuação

Escolha **uma** escala por sessão e mantenha em todos os fatores.

### Escala simples (recomendada para kick-off rápido)

| Valor | Impacto | Likelihood |
|-------|---------|------------|
| 1 | Baixo | Improvável |
| 2 | Médio | Possível |
| 3 | Alto | Provável |

### Escala de alta distinção (releases críticas)

| Valor | Uso |
|-------|-----|
| 0 | Desprezível / quase impossível |
| 1 | Baixo |
| 3 | Médio |
| 5 | Alto |
| 9 | Extremo |

A soma dos scores de Impacto posiciona no eixo vertical; a soma dos scores de Likelihood, no horizontal (ver [matriz-risco-quadrantes.md](matriz-risco-quadrantes.md)).

---

## Pesos opcionais

Cada fator pode ter peso de influência:

| Peso | Significado |
|------|-------------|
| 1 | Baixa influência no eixo |
| 2 | Influência normal (padrão) |
| 3 | Forte influência |

**Regra prática:** comece sem pesos; introduza pesos só se stakeholders concordarem que um fator domina (ex.: compliance com peso 3 em Impacto).

---

## Regras de scoring

1. **Usar a faixa completa** da escala — evitar que todos os itens fiquem em "2".
2. **Sem blanks** — todo risk item × fator deve ter score ou justificativa explícita de N/A.
3. **Distribuição homogênea por fator** — scores são **relativos** entre risk items no mesmo fator, não absolutos no universo.
4. **Documentar assumptions** — cada stakeholder registra premissas ("assumo que PIX já está em prod em 80% dos clientes").
5. **Mínimo 2 pessoas por fator** — stakeholder esquecido = risco não identificado.
6. **Revisitar** quando surgirem defeitos, mudanças de escopo ou novos requisitos.

---

## Stakeholders por fator

| Papel típico | Eixo | Fatores |
|--------------|------|---------|
| PO / negócio / suporte | Impacto | Área crítica, visibilidade, intensidade, importância |
| Arquiteto / dev sênior / QA técnico | Likelihood | Complexidade, mudanças, interfaces, histórico, etc. |

Se o usuário não informar stakeholders, **pergunte** antes da fase Individual Preparation — exceto no **modo rápido**, onde o QA documenta que simula ambos os papéis.

---

## Exemplos: scoring bom vs ruim

### Bom

| Risk item | Área crítica (I) | Complexidade (L) | Assumption |
|-----------|------------------|------------------|------------|
| RI-03 Pagamento PIX | 3 — perda financeira direta | 3 — código novo + integração banco | "Volume PIX > 40% das vendas no piloto" |
| RI-12 Tema escuro PDV | 1 — cosmético | 1 — CSS isolado, sem lógica | "Não afeta transação" |

- Usa extremos da escala.
- Assumptions explícitas.
- Distribuição variada entre itens.

### Ruim

| Risk item | Área crítica | Complexidade |
|-----------|--------------|--------------|
| RI-01 | 2 | 2 |
| RI-02 | 2 | 2 |
| RI-03 | 2 | 2 |

- Tudo no meio — matriz inútil.
- Sem assumptions.
- Impossível priorizar.
