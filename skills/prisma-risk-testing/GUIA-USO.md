# Guia rápido — skill `prisma-risk-testing`

Como priorizar testes com o método **PRISMA** (risco de **produto**: impacto × probabilidade de defeito).

---

## O que essa skill faz

Ela ajuda o QA a responder, com método:

- **O que testar primeiro** quando o tempo é curto
- **Com que profundidade** testar cada área (Must / Should / Could / Won't)
- **O que deixar de fora** — com aceite explícito, não “esquecido”

**Não é** o PRISMA de artigo científico (revisão sistemática em saúde).

---

## Quando usar

Use a skill quando você precisar de:

| Situação | Exemplo |
|----------|---------|
| Planejar testes de um épico ou release | “Tenho 2 semanas para testar o PIX” |
| Priorizar após mudança pequena | “Só mudou autenticação no MR” |
| Decidir o que rodar **hoje** | “Release amanhã, 6h de QA” |
| Reorganizar após bugs graves | “Achamos 3 bugs críticos, reprioriza” |
| Refinamento / upstream | “Quero matriz de risco antes do código” |

**Gatilhos que acionam a skill:** matriz de risco, teste baseado em risco, priorizar testes, must test, squeeze, good enough testing, PRISMA (de produto).

---

## Como executar (3 passos)

### 1. Abra o chat com a skill

No Cursor ou Claude Code, mencione a skill:

```
@prisma-risk-testing
```

Ou descreva o pedido em linguagem natural (a skill também dispara sem citar “PRISMA”).

### 2. Escolha o modo e envie o contexto

Diga **qual é a sua situação** e cole os links/textos abaixo.

### 3. Revise o relatório

A skill gera um arquivo em:

```
output/prisma/[nome-do-epico-ou-release]-[data].md
```

Leve o **resumo para stakeholders** na reunião de go/no-go. A decisão de liberar release é dos stakeholders; o QA **informa** os riscos.

---

## O que passar em cada modo

### Modo A — Planejamento completo

**Quando:** início de testes de épico, sprint ou release com tempo razoável (dias/semanas).

**Envie:**

- Link do **épico** (ou story) no ClickUp
- **Critérios de aceite** (se não estiverem no épico, cole aqui)
- **Prazo** de teste (ex.: “2 semanas”)
- Quem pode pontuar:
  - **Negócio** (PO, suporte) → impacto
  - **Técnico** (dev, arquiteto) → probabilidade de defeito

**Exemplo de mensagem:**

```
@prisma-risk-testing

Épico PIX: https://app.clickup.com/t/86abc123
AC: geração QR, confirmação assíncrona, estorno parcial, timeout 30s
Prazo: 2 semanas de teste
Stakeholders: PO Maria (impacto), dev João (likelihood)
Monta a matriz PRISMA completa com as 6 fases.
```

---

### Modo B — Rápido (solo QA)

**Quando:** MR pequeno, hotfix, ou não há tempo para workshop com o time.

**Envie:**

- Link da **MR** ou lista do que mudou
- Módulos/funcionalidades tocadas (ex.: JWT, logout, rate limit)

**Exemplo:**

```
@prisma-risk-testing

MR alterou só autenticação: refresh JWT, logout em todos os dispositivos, rate limit no login.
Modo rápido — sem reunião de consenso. Risk items + abordagem diferenciada.
```

A skill documenta que **você simula** negócio + técnico, com premissas explícitas.

---

### Modo C — Squeeze (release iminente)

**Quando:** deploy amanhã ou poucas horas de QA.

**Envie:**

- O que **mudou** na release (módulos: checkout, carrinho, cupom…)
- **Quantas horas** de QA você tem

**Exemplo:**

```
@prisma-risk-testing

Release amanhã. Mudou checkout, carrinho e cupom.
Só tenho 6 horas de QA. O que testar HOJE com PRISMA?
```

**Saída esperada:** lista **Must Test (Q I)** em ordem de execução + o que fica **aceito** para não testar agora.

---

### Modo D — Revisão pós-defeitos

**Quando:** já existe matriz e surgiram bugs importantes em teste ou produção.

**Envie:**

- Matriz anterior (arquivo ou resumo)
- Defeitos encontrados (onde, severidade)

**Exemplo:**

```
@prisma-risk-testing

Reprioriza a matriz: 3 bugs críticos em confirmação PIX assíncrona.
Anexo: output/prisma/epic-pix-20260516.md
```

---

## O que você recebe de volta

| Entregável | Para quê |
|------------|----------|
| Lista de **risk items** (RI-01, RI-02…) | Unidades testáveis ligadas ao épico/MR |
| **Matriz** por quadrante (I a IV) | Prioridade visual |
| **Plano de abordagem** | Técnicas, profundidade, ordem de execução |
| **Resumo para stakeholders** | Linguagem de negócio para release |
| **Good enough testing** | Checklist para apoiar decisão (não decide sozinho) |

Quadrantes em uma linha:

| Quadrante | Nome | Ação |
|-----------|------|------|
| **I** | Must Test | Testar primeiro, com mais profundidade |
| **II** | Should Test | Cobertura sólida |
| **III** | Could Test | Se sobrar tempo |
| **IV** | Won't Test | Só com **aceite documentado** |

---

## Checklist antes de pedir

- [ ] Sei **o que** está no escopo (épico, MR ou lista de mudanças)
- [ ] Informei **prazo** ou modo (completo / rápido / squeeze)
- [ ] No modo completo, indiquei **quem pontua** impacto e likelihood
- [ ] Não espero que a skill “aprove” release sozinha — ela **prioriza e documenta riscos**

---

## Dúvidas comuns

**Preciso citar “PRISMA”?**  
Não. Frases como “o que testar primeiro com pouco tempo” já acionam a skill.

**Preciso de planilha Excel?**  
Não. O padrão é Markdown; CSV é opcional.

**Combina com outras skills?**  
Sim. Ex.: `upstream-refinement-strategy` (estratégia por camada) depois da matriz PRISMA (prioridade).

---

## Referência técnica (agente)

Instruções completas da skill: [SKILL.md](SKILL.md)
