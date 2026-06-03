# Automatic Prompt Engineer (APE)

## O que é
Framework para geração e seleção automática de instruções/prompts usando LLMs.
Em vez de um humano iterar manualmente sobre prompts, o APE usa o próprio modelo
para gerar múltiplos candidatos de instrução, avalia cada um com uma métrica de
desempenho e seleciona o melhor automaticamente. Demonstrou que instruções geradas
automaticamente podem superar prompts escritos por humanos. É mais um conceito
e framework de pesquisa do que uma técnica aplicável diretamente, mas seus
princípios são valiosos para otimização de prompts.

## Quando usar
- Otimização sistemática de prompts em pipelines de produção
- Quando se precisa encontrar a melhor formulação para uma tarefa recorrente
- Benchmarking de diferentes formulações de instrução
- Pesquisa sobre eficácia de prompts
- Quando se tem um dataset de avaliação para medir qualidade

## Quando NÃO usar
- Tarefas pontuais ou de uso único
- Quando não se tem métricas claras de avaliação
- Interações conversacionais informais
- Quando a iteração manual de prompts é suficiente
- Se o custo de múltiplas avaliações é proibitivo

## Template / Estrutura

### Etapa 1 — Gerar candidatos de instrução

```
Tenho uma tarefa com os seguintes exemplos de entrada e saída:

Entrada: {{ exemplo_input_1 }} → Saída: {{ exemplo_output_1 }}
Entrada: {{ exemplo_input_2 }} → Saída: {{ exemplo_output_2 }}
Entrada: {{ exemplo_input_3 }} → Saída: {{ exemplo_output_3 }}

Gere 5 instruções diferentes que, quando colocadas antes de uma nova entrada,
produziriam a saída correta. Varie o estilo e a abordagem.
```

### Etapa 2 — Avaliar candidatos

```
Para cada instrução abaixo, aplique-a à entrada de teste e avalie a qualidade
da saída em uma escala de 1-10.

Instrução 1: {{ candidato_1 }}
Instrução 2: {{ candidato_2 }}
...
Instrução N: {{ candidato_N }}

Entrada de teste: {{ input_teste }}
Saída esperada: {{ output_esperado }}
```

### Etapa 3 — Selecionar melhor

```
Com base nas avaliações, selecione a instrução com melhor desempenho e
sugira refinamentos finais.
```

## Exemplo prático

### Exemplo — Otimizar prompt de classificação

**Etapa 1:**
```
Exemplos de classificação de e-mails:

"Sua fatura vence amanhã" → urgente
"Newsletter semanal de tecnologia" → informativo
"GANHE um iPhone GRÁTIS!!!" → spam
"Reunião remarcada para 15h" → urgente

Gere 5 instruções que realizem essa classificação corretamente.
```

**Saída (candidatos):**
```
1. "Classifique o e-mail como urgente, informativo ou spam."
2. "Leia o e-mail e determine sua categoria: urgente (requer ação imediata),
    informativo (apenas leitura) ou spam (indesejado/fraudulento)."
3. "Você é um filtro de e-mail. Categorize em: urgente, informativo, spam."
4. "Analise o tom e conteúdo do e-mail. Se requer ação → urgente.
    Se é conteúdo útil → informativo. Se é suspeito → spam."
5. "Classifique: [urgente] ação necessária, [informativo] sem ação,
    [spam] mensagem indesejada."
```

**Resultado da avaliação (aplicado a 20 e-mails de teste):**
- Instrução 4: 95% acurácia (melhor)
- Instrução 2: 90% acurácia
- Instrução 5: 85% acurácia

## Aplicação prática dos princípios do APE
Mesmo sem implementar o framework completo, você pode aplicar seus princípios:

1. **Gere variações**: peça ao modelo para reformular seu prompt de 3-5 formas
2. **Teste sistematicamente**: aplique cada variação a casos de teste conhecidos
3. **Selecione e refine**: escolha a melhor formulação e itere

## Fonte
https://www.promptingguide.ai/pt/techniques/ape
