# Automatic Reasoning and Tool-use (ART)

## O que é
Framework que combina Chain-of-Thought com uso automático de ferramentas externas.
Diante de uma nova tarefa, o ART seleciona demonstrações de raciocínio e uso de
ferramentas de uma biblioteca de tarefas pré-existente, decompõe o problema em
passos e invoca ferramentas (busca, cálculo, APIs) quando necessário.
É extensível: novas ferramentas podem ser adicionadas à biblioteca sem retreinar o modelo.
Essencialmente automatiza o padrão "pensar → agir → observar" de forma estruturada.

## Quando usar
- Tarefas que exigem combinação de raciocínio com dados externos
- Quando o modelo precisa fazer cálculos precisos (usar calculadora)
- Perguntas que exigem busca de informação atualizada
- Pipelines onde ferramentas diferentes são necessárias dependendo da tarefa
- Quando se quer um framework extensível para adicionar novas capacidades

## Quando NÃO usar
- Tarefas puramente textuais sem necessidade de ferramentas
- Quando não se tem ferramentas disponíveis para integrar
- Tarefas simples onde CoT já resolve
- Ambientes onde chamadas a ferramentas externas não são permitidas

## Template / Estrutura

```
Biblioteca de ferramentas disponíveis:
- {{ ferramenta_1 }}: {{ descrição }}
- {{ ferramenta_2 }}: {{ descrição }}
- {{ ferramenta_N }}: {{ descrição }}

Tarefa: {{ descrição_da_tarefa }}
Entrada: {{ input }}

Resolva passo a passo. Quando precisar de informação externa ou cálculo,
use uma das ferramentas disponíveis no formato:
[Ferramenta: {{ nome }}] {{ parâmetros }}
→ Resultado: {{ resultado }}

Em seguida, continue o raciocínio com o resultado obtido.
```

## Exemplo prático

### Exemplo — Pesquisa com cálculo

**Prompt:**
```
Ferramentas disponíveis:
- Busca: pesquisa informações na web
- Calculadora: realiza cálculos matemáticos

Tarefa: Qual é a população combinada de São Paulo e Rio de Janeiro, e qual
porcentagem isso representa da população do Brasil?

Resolva passo a passo usando as ferramentas quando necessário.
```

**Resposta esperada:**
```
Passo 1: Buscar a população de São Paulo.
[Ferramenta: Busca] "população São Paulo 2024"
→ Resultado: ~12,3 milhões (município)

Passo 2: Buscar a população do Rio de Janeiro.
[Ferramenta: Busca] "população Rio de Janeiro 2024"
→ Resultado: ~6,7 milhões (município)

Passo 3: Buscar a população do Brasil.
[Ferramenta: Busca] "população Brasil 2024"
→ Resultado: ~203 milhões

Passo 4: Calcular a população combinada.
[Ferramenta: Calculadora] 12.300.000 + 6.700.000
→ Resultado: 19.000.000

Passo 5: Calcular a porcentagem.
[Ferramenta: Calculadora] (19.000.000 / 203.000.000) × 100
→ Resultado: 9,36%

Resposta: São Paulo e Rio de Janeiro juntos têm aproximadamente 19 milhões
de habitantes, representando cerca de 9,4% da população brasileira.
```

## Relação com outras técnicas
- **ReAct**: ART automatiza e generaliza o padrão ReAct com biblioteca de tarefas
- **CoT**: ART estende CoT adicionando ferramentas nos passos intermediários
- **PAL**: enquanto PAL gera código, ART pode usar qualquer tipo de ferramenta

## Dicas
- Defina claramente o formato de invocação de cada ferramenta
- A biblioteca de demonstrações deve cobrir os tipos de tarefa mais comuns
- Novas ferramentas podem ser adicionadas com uma demonstração de uso

## Fonte
https://www.promptingguide.ai/pt/techniques/art
