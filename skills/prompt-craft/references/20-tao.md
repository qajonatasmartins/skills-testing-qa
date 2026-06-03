# TAO (Tarefa, Ação e Objetivo)

## O que é
Framework orientado a ação que estrutura o prompt em três componentes: a **Tarefa** a ser realizada, a **Ação** concreta que detalha como executá-la e o **Objetivo** final que se deseja alcançar. Diferencia-se do PTF por focar no "porquê" (objetivo) em vez do "quem" (papel), direcionando a IA para resultados com propósito claro.

## Quando usar
- Tarefas orientadas a resultado onde o objetivo final é mais importante que a persona.
- Instruções de ensino, explicação ou tradução de conceitos complexos para públicos específicos.
- Quando a ação precisa ser detalhada para guiar o estilo ou abordagem da resposta.
- Cenários em que o "para quê" da tarefa influencia diretamente a qualidade do output.

## Quando NÃO usar
- Tarefas que dependem fortemente de contexto rico ou exemplos ilustrativos (prefira CARE).
- Cenários que precisam de raciocínio passo a passo explícito (prefira Chain-of-Thought ou PEPE).
- Quando a persona/papel é determinante para o tom e profundidade da resposta (prefira PTF).
- Problemas que partem de uma dor ou limitação conhecida (prefira PME).

## Template / Estrutura

```
Sua tarefa é {{ tarefa }}
{{ ação }}
O objetivo central é {{ objetivo }}
```

| Componente | Descrição | Dica |
|---|---|---|
| **Tarefa** | O que a IA deve fazer, declarado de forma direta | Comece com verbos de ação: "ensinar", "criar", "analisar", "traduzir" |
| **Ação** | Como executar a tarefa — detalhes de abordagem, estilo, restrições | Especifique tom, público-alvo, nível de profundidade |
| **Objetivo** | Resultado final desejado — o propósito que motiva a tarefa | Conecte ao impacto real: "para que o time adote...", "para tornar acessível..." |

## Exemplo prático

**Prompt:**
> Sua tarefa é ensinar conceitos básicos de qualidade de software para iniciantes. Explique os fundamentos de forma clara e com exemplos do dia a dia. O objetivo central é tornar o conteúdo acessível e atrativo para quem está começando na área.

**Por que funciona:**
- A tarefa é clara e delimitada: "ensinar conceitos básicos de qualidade de software".
- A ação especifica a abordagem: "de forma clara e com exemplos do dia a dia".
- O objetivo direciona o resultado: conteúdo acessível para iniciantes, evitando jargão excessivo.

## Fonte
Técnica derivada de frameworks de Task-Action-Objective utilizados em design instrucional e engenharia de prompt. Referência: guias práticos de prompt engineering da comunidade de IA generativa.
