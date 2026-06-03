# Self-Consistency

## O que é
Técnica que melhora o Chain-of-Thought gerando múltiplos caminhos de raciocínio
para o mesmo problema e selecionando a resposta mais frequente (voto majoritário).
Em vez de confiar em um único caminho de raciocínio (que pode conter erros),
a self-consistency explora a diversidade de raciocínios possíveis e escolhe
o consenso. Funciona porque o caminho pode variar, mas a resposta correta tende
a convergir.

## Quando usar
- Tarefas de raciocínio onde a precisão é crítica
- Problemas matemáticos ou lógicos com múltiplas abordagens possíveis
- Quando CoT simples produz respostas inconsistentes entre execuções
- Decisões de alto impacto onde vale o custo extra de tokens
- Validação de respostas em pipelines automatizados

## Quando NÃO usar
- Tarefas simples onde CoT já é consistente
- Quando custo de tokens/latência é uma restrição forte
- Tarefas criativas ou abertas sem resposta "correta" (não há convergência)
- Classificação binária simples
- Quando não se tem controle sobre temperatura/amostragem do modelo

## Template / Estrutura

### Passo 1 — Gerar N raciocínios

```
{{ problema }}

Resolva mostrando o raciocínio passo a passo.
```
*(executar o mesmo prompt N vezes com temperatura > 0)*

### Passo 2 — Agregar respostas

```
Dadas as seguintes respostas para o mesmo problema, identifique a resposta
mais frequente (voto majoritário):

Raciocínio 1: {{ caminho_1 }} → Resposta: {{ resposta_1 }}
Raciocínio 2: {{ caminho_2 }} → Resposta: {{ resposta_2 }}
Raciocínio 3: {{ caminho_3 }} → Resposta: {{ resposta_3 }}
...
Raciocínio N: {{ caminho_N }} → Resposta: {{ resposta_N }}

Resposta final (mais frequente):
```

### Variação prática (prompt único simulando self-consistency)

```
{{ problema }}

Resolva este problema por 3 caminhos de raciocínio diferentes.
Para cada caminho, mostre os passos e a resposta.
Depois, indique a resposta final com base no consenso.
```

## Exemplo prático

### Exemplo — Problema aritmético com 3 caminhos

**Prompt:**
```
Uma padaria fez 200 pães. Vendeu 2/5 de manhã e 1/3 do restante à tarde.
Quantos pães sobraram?

Resolva por 3 caminhos diferentes e indique a resposta consensual.
```

**Resposta esperada:**
```
Caminho 1:
- De manhã: 2/5 de 200 = 80 vendidos → restam 120
- À tarde: 1/3 de 120 = 40 vendidos → restam 80
- Resposta: 80

Caminho 2:
- Pães vendidos de manhã: 200 × 0,4 = 80
- Restante: 200 - 80 = 120
- Vendidos à tarde: 120 ÷ 3 = 40
- Sobraram: 120 - 40 = 80
- Resposta: 80

Caminho 3:
- Fração vendida de manhã: 2/5
- Fração restante após manhã: 3/5
- Fração vendida à tarde: 1/3 × 3/5 = 1/5
- Total vendido: 2/5 + 1/5 = 3/5
- Sobrou: 2/5 de 200 = 80
- Resposta: 80

Consenso (3/3): 80 pães sobraram.
```

## Considerações de implementação
- **Temperatura**: use valores > 0 (ex: 0.7) para gerar diversidade nos caminhos
- **Número de amostras**: 5-10 caminhos é um bom equilíbrio custo/benefício
- **Custo**: multiplica o consumo de tokens pelo número de amostras
- **Automação**: ideal para pipelines onde se pode chamar a API múltiplas vezes

## Fonte
https://www.promptingguide.ai/pt/techniques/consistency
