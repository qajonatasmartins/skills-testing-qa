# Pairwise Testing

Empregado quando o sistema tem muitas variáveis e a combinação de todas elas geraria um número impossível de testes. Ele foca em testar todas as combinações em pares, reduzindo drasticamente o esforço de teste.

## Princípio básico

A técnica baseia-se na ideia de testar apenas as combinações de todos os *pares* possíveis de variáveis, em vez de tentar testar de forma exaustiva todas as combinações possíveis do sistema. Isso funciona porque a maioria dos defeitos de software costuma ser de "modo único" (uma funcionalidade falha independentemente) ou de "modo duplo" (a interação exclusiva entre duas funções ou módulos resulta na falha). Ao assegurar que cada par seja exercitado ao menos uma vez, é possível localizar a grande maioria dos problemas usando uma fração minúscula de esforço.

## Quando usar

A técnica deve ser usada em situações onde há múltiplas variáveis que assumem diversos valores diferentes e causam uma grande "explosão combinatória", resultando em uma quantidade de cenários impossível de se cobrir devidos às limitações de tempo e recursos da equipe.

## Quando não usar

Você não deve depender exclusivamente de testes Pairwise se você ou os desenvolvedores tiverem consciência de que combinações específicas (mesmo triplas ou maiores) são muito utilizadas na prática ou envolvem riscos críticos. Por exemplo, uma configuração ou fluxo que acione um cenário do tipo "desligue o reator nuclear" deve obrigatoriamente funcionar. Caso as ferramentas de geração em pares não selecionem essas interações específicas no subconjunto, você não deve deixar de testá-las. A recomendação é usar o Pairwise e *adicionar manualmente* os casos de risco ou de uso frequente. Além disso, se o número de combinações for suficientemente pequeno, pode-se simplesmente testar tudo.

## Como usar

O processo de criação de testes Pairwise geralmente envolve as seguintes etapas:

1. Identifique as variáveis do sistema e determine os possíveis valores/opções que cada uma pode assumir.
2. Como não é viável encontrar todos os pares de cabeça, utiliza-se métodos consolidados para gerar a lista de testes, como os **Arrays Ortogonais** (matrizes bidimensionais matemáticas prontas encontradas em catálogos), ou ferramentas computacionais de lógica desbalanceada baseadas no **Algoritmo Allpairs**.
3. Mapeie o seu problema do mundo real para dentro da matriz escolhida, substituindo as colunas pelas variáveis e os números internos pelos valores reais do software.
4. Se usar Arrays Ortogonais e a matriz ficar com "células sobrando", complemente com valores válidos para não perder a ortogonalidade.
5. Construa os casos de teste finais baseado nas linhas da tabela gerada. Note que a matriz fornece apenas os parâmetros de entrada; você, atuando como o "oráculo" de testes, precisará determinar o resultado esperado de cada cenário de teste.

## Exemplos práticos

Na prática, testadores evitam gerar combinações complexas sozinhos e se apoiam em ferramentas. Um analista poderia usar uma ferramenta da web, como o programa *Allpairs* de James Bach (onde se insere os dados numa planilha, exporta em .txt e o programa cospe apenas os testes a fazer), ou ferramentas focadas em Arrays Ortogonais (como o rdExpert ou o catálogo de Neil J.A. Sloane). Durante a utilização prática, o testador também precisa lidar com restrições lógicas do ambiente. Por exemplo, a matriz matemática pode pedir para testar um servidor IIS com o MacOS (incompatíveis). O testador mapeia essa restrição em ferramentas mais potentes (como rdExpert ou AETG) que irão gerar os pares de forma inteligente sem ferir as limitações físicas conhecidas do projeto.

[Consulte os outros exemplos de como usar no documento](../../../examples/black-box-testing-techniques/pairwise-testing/readme.md)
