# Active-Prompt

## O que é
Método que adapta prompts Chain-of-Thought selecionando os exemplos mais
informativos para anotação humana. Em vez de usar exemplos fixos de CoT,
o Active-Prompt identifica quais perguntas o modelo tem mais incerteza
(baseado na divergência entre múltiplas respostas) e prioriza essas
perguntas para anotação humana de raciocínio. Isso resulta em exemplares
de CoT mais eficazes e específicos para a tarefa em questão.

## Quando usar
- Quando se está construindo prompts few-shot CoT para uma tarefa específica
- Se os exemplos CoT genéricos não funcionam bem no domínio
- Quando se tem orçamento limitado de anotação humana e quer maximizar impacto
- Otimização de pipelines de raciocínio em produção
- Tarefas onde a seleção de exemplos impacta significativamente a qualidade

## Quando NÃO usar
- Uso casual ou ad-hoc de prompts
- Quando não se tem capacidade de anotação humana
- Tarefas simples onde CoT padrão já funciona
- Quando não se pode executar múltiplas inferências para medir incerteza
- Prototipagem rápida (overhead alto)

## Template / Estrutura

### Processo em 4 etapas

```
ETAPA 1 — Gerar respostas iniciais:
Para cada pergunta do dataset, gerar K respostas com CoT (temperatura > 0).

ETAPA 2 — Medir incerteza:
Calcular a divergência/desacordo entre as K respostas para cada pergunta.
Perguntas com maior desacordo = maior incerteza.

ETAPA 3 — Anotar exemplos incertos:
Selecionar as N perguntas mais incertas para anotação humana
de raciocínio CoT correto.

ETAPA 4 — Usar exemplos anotados como few-shot:
Montar prompt few-shot CoT com os exemplos anotados na Etapa 3.
```

### Template prático (simulado)

```
Vou testar a incerteza do modelo em {{ N }} perguntas.

{{ pergunta_1 }}
{{ pergunta_2 }}
...
{{ pergunta_N }}

Para cada pergunta, gere 3 respostas com raciocínios diferentes.
Indique o nível de concordância (alta/média/baixa) entre as respostas.
As perguntas com baixa concordância são prioridade para anotação humana.
```

## Exemplo prático

### Exemplo — Identificar perguntas incertas para anotação

**Etapa 1 — Testar incerteza:**
```
Para cada pergunta abaixo, gere 3 respostas diferentes com raciocínio.
Depois, indique se as 3 respostas concordam.

P1: Um trem sai de A às 8h a 60km/h. Outro sai de B (300km de A)
    às 9h a 90km/h. Quando se encontram?
P2: 15% de 200 é quanto?
P3: Se todos os gatos são animais e alguns animais são ferozes,
    todos os gatos são ferozes?
```

**Resultado:**
```
P1: Respostas divergem (2h40 vs 2h vs 2h24) → INCERTEZA ALTA ⚠️
P2: Todas concordam (30) → INCERTEZA BAIXA ✓
P3: Respostas divergem (sim vs não vs depende) → INCERTEZA ALTA ⚠️
```

**Etapa 2 — Anotar P1 e P3 com raciocínio humano correto:**
```
P1 (anotação humana):
- Às 9h, o trem A já percorreu 60km. Distância restante: 240km.
- Velocidade de aproximação: 60 + 90 = 150km/h.
- Tempo: 240/150 = 1,6h = 1h36min.
- Encontro: 9h + 1h36 = 10h36.
```

**Etapa 3 — Usar como exemplos few-shot para novas perguntas similares.**

## Relação com outras técnicas
- **Few-Shot CoT**: Active-Prompt otimiza a seleção de exemplos do few-shot CoT
- **Self-Consistency**: usa o mesmo princípio de múltiplas respostas, mas para medir incerteza
- **APE**: ambos buscam otimizar prompts, mas APE foca na instrução e Active-Prompt nos exemplos

## Dicas
- Comece com 5-10 perguntas representativas para medir incerteza
- Use temperatura > 0.5 para gerar diversidade nas respostas
- Os exemplos mais incertos (onde o modelo erra mais) são os mais valiosos para anotar
- Na prática, mesmo a intuição de "onde o modelo erra" já ajuda a selecionar bons exemplos

## Fonte
https://www.promptingguide.ai/pt/techniques/activeprompt
