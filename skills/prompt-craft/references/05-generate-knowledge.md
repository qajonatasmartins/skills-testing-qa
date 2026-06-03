# Generate Knowledge Prompting

## O que é
Técnica em dois estágios: primeiro solicita-se ao modelo que gere conhecimento ou fatos
relevantes sobre o tema, depois utiliza-se esse conhecimento como contexto adicional
para responder à pergunta original. Isso "ancora" o modelo em informações factuais
antes de raciocinar, melhorando a precisão em tarefas que dependem de senso comum
ou conhecimento de domínio.

## Quando usar
- Perguntas que dependem de senso comum ou conhecimento factual
- Tarefas onde o modelo tende a "alucinar" por falta de contexto
- Quando não se tem documentos externos para RAG, mas o modelo possui o conhecimento internamente
- Perguntas de verdadeiro/falso sobre fatos do mundo real
- Tarefas de QA que exigem raciocínio sobre fatos implícitos

## Quando NÃO usar
- Quando se tem acesso a documentos/bases de dados reais (use RAG em vez disso)
- Tarefas puramente computacionais ou lógicas (o conhecimento gerado não ajuda)
- Quando o conhecimento necessário é muito recente ou específico (o modelo pode não ter)
- Domínios altamente técnicos onde conhecimento impreciso é pior que nenhum

## Template / Estrutura

### Estágio 1 — Gerar conhecimento

```
Gere {{ N }} fatos relevantes sobre: {{ tema ou contexto da pergunta }}

Fatos:
```

### Estágio 2 — Usar conhecimento para responder

```
Contexto (fatos conhecidos):
{{ fatos gerados no estágio 1 }}

Com base nos fatos acima, responda:
{{ pergunta original }}
```

### Variação em prompt único

```
Primeiro, liste 3-5 fatos relevantes sobre {{ tema }}.
Depois, com base nesses fatos, responda: {{ pergunta }}
```

## Exemplo prático

### Exemplo 1 — Conhecimento de domínio (nutrição)

**Estágio 1:**
```
Gere 3 fatos relevantes sobre o consumo de ovos e colesterol.

Fatos:
```

**Resposta do Estágio 1:**
```
1. Estudos recentes mostram que o colesterol dietético (dos alimentos) tem impacto menor
   no colesterol sanguíneo do que se acreditava anteriormente.
2. Um ovo grande contém cerca de 186mg de colesterol, concentrado na gema.
3. A American Heart Association não estabelece mais um limite diário específico de
   colesterol dietético para a maioria das pessoas saudáveis.
```

**Estágio 2:**
```
Contexto (fatos conhecidos):
1. Estudos recentes mostram que o colesterol dietético tem impacto menor no colesterol sanguíneo.
2. Um ovo grande contém cerca de 186mg de colesterol, concentrado na gema.
3. A AHA não estabelece mais um limite diário de colesterol dietético para pessoas saudáveis.

Pergunta: Comer 2 ovos por dia faz mal à saúde?
```

**Resposta esperada:**
```
Com base nos fatos conhecidos, para a maioria das pessoas saudáveis, consumir 2 ovos por dia
não é considerado prejudicial. Embora contenham colesterol, estudos recentes indicam que o
impacto do colesterol dietético no colesterol sanguíneo é menor do que se pensava. No entanto,
pessoas com condições cardíacas pré-existentes devem consultar um médico.
```

### Exemplo 2 — Prompt único (senso comum)

**Prompt:**
```
Primeiro, liste 3 fatos sobre como vidro reage a mudanças bruscas de temperatura.
Depois, responda: Posso colocar uma travessa de vidro quente direto na água fria?
```

## Dicas
- A qualidade do conhecimento gerado é proporcional à capacidade do modelo no domínio
- Gerar mais fatos (5-10) e selecionar os mais relevantes melhora os resultados
- Combine com Self-Consistency: gere conhecimentos diferentes e vote na melhor resposta

## Fonte
https://www.promptingguide.ai/pt/techniques/knowledge
