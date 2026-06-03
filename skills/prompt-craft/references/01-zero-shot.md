# Zero-Shot Prompting

## O que é
Técnica onde se envia uma instrução direta ao modelo sem fornecer nenhum exemplo prévio.
O modelo utiliza apenas seu conhecimento pré-treinado para interpretar a tarefa e gerar a resposta.
Funciona bem quando a tarefa é simples, bem definida e o modelo já possui familiaridade com o domínio.
É a forma mais básica e rápida de prompting.

## Quando usar
- Tarefas simples de classificação (sentimento, categoria, spam)
- Tradução direta entre idiomas
- Extração de informação pontual
- Sumarização de textos curtos
- Formatação ou conversão de dados
- Quando velocidade de iteração importa mais que precisão máxima

## Quando NÃO usar
- Tarefas que exigem formato de saída muito específico ou incomum
- Domínios altamente especializados onde o modelo pode não ter conhecimento suficiente
- Quando a tarefa é ambígua e exemplos ajudariam a desambiguar
- Raciocínio matemático ou lógico complexo
- Quando os resultados zero-shot não atingem a qualidade desejada (escale para few-shot)

## Template / Estrutura

```
{{ instrução clara e direta }}

{{ entrada / texto a ser processado }}

{{ formato esperado da resposta (opcional mas recomendado) }}
```

### Variação com formato explícito

```
Classifique o texto abaixo em uma das categorias: {{ lista de categorias }}.
Responda APENAS com o nome da categoria.

Texto: "{{ texto de entrada }}"
Categoria:
```

## Exemplo prático

### Exemplo 1 — Classificação de sentimento

**Prompt:**
```
Classifique o sentimento do texto abaixo como Positivo, Negativo ou Neutro.

Texto: "O atendimento foi péssimo, esperei 40 minutos e ninguém me ajudou."
Sentimento:
```

**Resposta esperada:**
```
Negativo
```

### Exemplo 2 — Extração de informação

**Prompt:**
```
Extraia o nome do produto e o valor do texto abaixo. Responda em JSON.

Texto: "Comprei o Notebook Dell Inspiron por R$ 3.499,00 na promoção de ontem."
```

**Resposta esperada:**
```json
{
  "produto": "Notebook Dell Inspiron",
  "valor": "R$ 3.499,00"
}
```

## Fonte
https://www.promptingguide.ai/pt/techniques/zeroshot
