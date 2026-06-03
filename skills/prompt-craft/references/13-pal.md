# Program-Aided Language Models (PAL)

## O que é
Técnica onde, em vez de resolver problemas de raciocínio com texto livre
(como no CoT), o modelo gera código como passo intermediário. O código é
então executado por um interpretador (Python, por exemplo) para obter a
resposta exata. Isso elimina erros de cálculo que LLMs frequentemente
cometem em raciocínio aritmético, pois a parte computacional é delegada
a um programa real. O modelo foca no que faz bem (entender o problema
e traduzi-lo em lógica) e delega o que faz mal (calcular) para o código.

## Quando usar
- Problemas matemáticos ou aritméticos onde precisão é essencial
- Tarefas de lógica que podem ser expressas como código
- Manipulação de dados estruturados (listas, datas, strings)
- Quando CoT produz erros de cálculo intermediário
- Processamento de regras de negócio complexas
- Qualquer tarefa onde um interpretador pode verificar a resposta

## Quando NÃO usar
- Tarefas puramente textuais sem componente computacional
- Quando não se tem acesso a um interpretador de código
- Perguntas de opinião, criatividade ou subjetividade
- Tarefas onde o raciocínio textual é o produto desejado (não apenas a resposta)
- Domínios onde gerar código introduz mais complexidade do que resolve

## Template / Estrutura

### Template básico

```
Resolva o problema abaixo gerando código Python que calcule a resposta.

Problema: {{ descrição_do_problema }}

Código Python:
```

### Template com exemplos (few-shot)

```
Resolva problemas gerando código Python executável.

Problema: João tem 5 maçãs e compra 3 sacolas com 4 maçãs cada.
Código:
maças_iniciais = 5
sacolas = 3
maças_por_sacola = 4
total = maças_iniciais + (sacolas * maças_por_sacola)
print(total)  # 17

Problema: {{ novo_problema }}
Código:
```

## Exemplo prático

### Exemplo 1 — Cálculo financeiro

**Prompt:**
```
Resolva gerando código Python executável.

Problema: Um investimento de R$ 10.000 rende 1,2% ao mês com juros compostos.
Qual o valor acumulado após 24 meses? Quanto foi o rendimento total?
```

**Resposta do modelo (código gerado):**
```python
investimento_inicial = 10000
taxa_mensal = 0.012
meses = 24

valor_final = investimento_inicial * (1 + taxa_mensal) ** meses
rendimento = valor_final - investimento_inicial

print(f"Valor acumulado: R$ {valor_final:.2f}")
print(f"Rendimento total: R$ {rendimento:.2f}")
```

**Saída da execução:**
```
Valor acumulado: R$ 13.314,94
Rendimento total: R$ 3.314,94
```

### Exemplo 2 — Lógica com datas

**Prompt:**
```
Resolva gerando código Python.

Problema: Um projeto começou em 15/03/2025. O sprint dura 14 dias,
com 2 dias de intervalo entre sprints. Qual a data de início do 5º sprint?
```

**Resposta do modelo:**
```python
from datetime import datetime, timedelta

inicio_projeto = datetime(2025, 3, 15)
duracao_sprint = 14
intervalo = 2
sprint_alvo = 5

dias_ate_sprint = (sprint_alvo - 1) * (duracao_sprint + intervalo)
inicio_5_sprint = inicio_projeto + timedelta(days=dias_ate_sprint)

print(f"Início do 5º sprint: {inicio_5_sprint.strftime('%d/%m/%Y')}")
```

**Saída:**
```
Início do 5º sprint: 18/05/2025
```

## Relação com outras técnicas
- **CoT**: PAL substitui raciocínio textual por código, eliminando erros de cálculo
- **ART**: ART pode usar PAL como uma das ferramentas disponíveis
- **ReAct**: PAL pode ser a "ação" de execução em um loop ReAct

## Dicas
- Instrua o modelo a gerar código limpo e comentado
- Inclua `print()` com formatação clara no código gerado
- Para problemas complexos, peça que o modelo explique a lógica em comentários
- Valide o código gerado antes de executar em produção
- Funciona especialmente bem com Python pela legibilidade

## Fonte
https://www.promptingguide.ai/pt/techniques/pal
