# Data Flow Testing

Avalia o ciclo de vida das variáveis do programa — onde elas são criadas (definidas), utilizadas e destruídas. É altamente eficaz para detectar erros de lógica, como variáveis que são usadas antes mesmo de receberem um valor.

## Princípio básico

O Teste de Fluxo de Dados (Data Flow Testing) é uma técnica de teste caixa-branca (white box testing) que constrói e expande as técnicas de teste de fluxo de controle. O princípio central dessa técnica é focar no ciclo de vida das variáveis dentro do sistema: quando elas são criadas (definidas), quando são usadas (em computações ou condições) e quando são destruídas (mortas). A técnica rastreia sequências temporais de pares desses estados (definido, usado, morto) para detectar o uso inadequado de valores de dados resultante de erros de codificação. A ideia fundamental é que um testador não deve confiar em um programa sem antes verificar o efeito do uso do valor produzido por todas as suas computações.

## Quando usar

Esta técnica deve ser utilizada para todos os módulos de código que não podem ser testados de forma suficiente apenas por meio de revisões e inspeções. É uma ferramenta extremamente poderosa para identificar erros comuns de codificação em que o valor de uma variável é referenciado no código antes mesmo de ter um valor atribuído a ela (uso de variável não inicializada).

## Quando não usar

O Teste de Fluxo de Dados possui limitações e não é recomendado ou factível nas seguintes situações:

- Em sistemas com grandes restrições de tempo, pois a técnica pode consumir muito tempo devido à quantidade de módulos, caminhos e variáveis envolvidos no sistema.
- Para avaliar matrizes (arrays) cujos elementos são referenciados e alterados dinamicamente (como `stuff[j]`), pois a análise estática não consegue determinar se as regras de definição/uso foram seguidas corretamente sem analisar cada elemento individualmente.
- Em sistemas que processam interrupções com múltiplos níveis de prioridade de execução, pois a análise estática das possíveis interações torna-se complexa demais para ser feita manualmente.
- Em fluxos de controle tão complexos que contenham caminhos que nunca serão executados, podendo indicar combinações inválidas no modelo de dados que não ocorrerão na prática.

## Como usar

A técnica divide-se na criação de um gráfico de fluxo de dados (semelhante ao de controle, mas detalhando a definição, uso e destruição de cada variável) e na aplicação de dois métodos de teste:

1. **Teste Estático:** O diagrama de fluxo é inspecionado formal ou informalmente em busca de padrões sequenciais problemáticos nas variáveis. Avalia-se pares de ação, classificando-os em aceitáveis (ex: definir e depois usar) ou erros (ex: variável não existe e é usada, variável morta e depois usada, variável destruída sem nunca ser usada).
2. **Teste Dinâmico:** Partindo do pressuposto de que o fluxo de controle está correto, caminhos de teste são gerados através do módulo. O testador começa na entrada do módulo e segue variando as condições de ramificação até listar todos os caminhos. Em seguida, para cada variável, cria-se pelo menos um caso de teste que cubra cada par de "definição-uso" (define-use pair) do início ao fim.

## Exemplos práticos

Na prática, o testador examina combinações das letras "d" (definido), "u" (usado), "k" (morto/destruído) e "~" (não existe). Exemplos práticos de análises desses padrões incluem:

- **~u (Não existe e é usado):** Ocorre quando uma variável tenta ser utilizada sem ter sido definida. É um erro grave e comum.
- **ku (Morto e usado):** Tentar utilizar uma variável que já foi destruída pelo sistema (fora de escopo). É considerado um defeito grave.
- **dd (Definido e definido novamente):** Não é inválido, mas é suspeito e frequentemente aponta para um erro de programação, pois a primeira definição foi inútil.
- **ud (Usado e definido) ou uk (Usado e morto):** Padrões aceitáveis do ciclo de vida normal da variável.

[Consulte os outros exemplos de como usar no documento](../../../examples/white-box-testing-techniques/data-flow-testing/readme.md)
