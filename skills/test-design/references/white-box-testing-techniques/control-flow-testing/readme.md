# Control Flow Testing

Cria fluxos que passam pelos diversos resultados de pontos de decisão no código, como os caminhos que respondem "verdadeiro" e "falso" para comandos "IF".

Para que serve: É uma medida mais robusta que o teste de instrução e serve para garantir a detecção de anomalias que fiquem esquecidas em desvios lógicos condicionais.

## Princípio básico

A técnica de *Control Flow Testing* (Teste de Fluxo de Controle) é uma estratégia de teste de caixa-branca (*white box testing*) estrutural. O princípio fundamental baseia-se em identificar os caminhos de execução por dentro de um módulo de código de programação e, em seguida, criar e executar casos de teste para cobrir especificamente esses caminhos. A técnica transforma o código em **grafos de fluxo de controle**, compostos por nós (blocos de processamento sequencial sem desvios) e arestas (as transições), além de pontos de decisão (como blocos `if` e `case`) que geram múltiplos caminhos de saída. A técnica visa atender a níveis métricos rigorosos de cobertura (como cobrir 100% dos comandos, 100% das decisões ou cobrir condições múltiplas). Uma das abordagens mais estruturadas dessa técnica é o **Teste de Caminho Básico** (*Basis Path Testing*) de Tom McCabe, que usa a topologia do grafo para garantir cobertura completa de comandos e ramificações.

### Níveis de Cobertura (Levels of Coverage)

1. **Nível 0 (Sem Cobertura Formal):** Este nível é como "teste o que você testar; deixe os usuários testarem o resto". É o nível que fica abaixo da cobertura de comandos e, segundo os especialistas, entregar um código não testado nesse nível é algo "estúpido, míope e irresponsável".
2. **Nível 1 (Cobertura de Comandos/Statement Coverage):** O objetivo é a cobertura de 100% das **instruções**. Significa que casos de teste são criados para que cada declaração ou linha dentro do módulo seja executada pelo menos uma vez. Muitos defeitos passam despercebidos apenas com este nível.
3. **Nível 2 (Cobertura de Decisão/Decision or Branch Coverage):** Garante 100% de cobertura de decisões (ou ramificações). Neste nível, os testes forçam cada ponto de decisão do código a resultar em **Verdadeiro** pelo menos uma vez, e em **Falso** pelo menos uma vez.
4. **Nível 3 (Cobertura de Condição/Condition Coverage):** O foco passa a ser as subcondições dentro de instruções complexas. Os testes devem garantir que cada condição individual (dentro de uma instrução que contém múltiplos critérios) seja avaliada como **Verdadeira** e como **Falsa**.
5. **Nível 4 (Cobertura de Decisão/Condição e Múltiplas Condições/Multiple Condition Coverage):** Exige 100% de cobertura sobre as combinações das condições. Avaliar todas as combinações lógicas possíveis geradas pelo compilador dentro daquela decisão. Atingir a cobertura de múltiplas condições cobre automaticamente as exigências dos níveis de Decisão (Nível 2) e Condição (Nível 3).
6. **Nível 5: (Omitido ou não listado na categorização do autor).**
7. **Nível 6 (Limitação de Laços/Loop Coverage):** Como laços de repetição (loops) criam uma infinidade de caminhos impossíveis de se testar totalmente, este nível faz uma redução significativa e inteligente. O teste foca em executar os laços zero vezes, 1 vez, n vezes (representando um número típico), no limite máximo estipulado m, além de testar as fronteiras de m−1 e m+1.
8. **Nível 7 (Cobertura de Caminhos/Path Coverage):** É o nível mais alto e rigoroso de todos, visando 100% de cobertura de caminhos. Significa que um caso de teste deve ser criado para testar cada rota independente existente do início ao fim do módulo. Para módulos sem laços aninhados ou infinitos, o número de caminhos é tratável e um caso de teste pode realmente ser construído para cada caminho.

## Quando usar

* É a **pedra base dos testes de unidade** (*unit testing*) executados nos componentes e deve ser aplicada em todos os módulos de código que não podem ser validados e verificados de forma suficiente apenas através de inspeções e revisões de código.
* Quando for exigido que o teste tenha um rastreamento exato do que foi executado na arquitetura interna e for necessário garantir ou certificar métricas percentuais explícitas de qualidade e segurança do código (como alcançar a "cobertura de 100% de decisões" ou "100% de comandos").
* Para validar a integração e mapear os caminhos não apenas dentro de um único método, mas também entre módulos conectados e até sistemas inteiros.

## Quando não usar

A técnica tem limitações e não deve ser usada de forma cega ou exclusiva nas seguintes situações:

1. Quando a quantidade de caminhos for colossal ou infinita (por exemplo, devido a laços de repetição que disparam milhões de combinações), pois é inviável testar todos os caminhos exaustivamente num prazo aceitável.
2. Quando a prioridade for achar defeitos de sensibilidade de dados (ex: o código tem o caminho exato para fazer uma conta matemática de divisão, mas falha pontualmente se o divisor for zero). O teste de fluxo de controle não tem foco direto nesses problemas se o fluxo direcional estiver correto.
3. Para tentar encontrar lógicas e caminhos que **faltam** no código. Como a técnica baseia seus testes no fluxo existente da aplicação, não é possível cobrir ou descobrir funcionalidades que o programador esqueceu totalmente de implementar.
4. Se o testador não possuir conhecimento detalhado de programação para entender o código-fonte, mapear sua estrutura interna e identificar falhas técnicas no fluxo de controle.

## Como usar

O uso da técnica via a metodologia clássica de Teste Estruturado (Caminhos Básicos de McCabe) segue estes passos:

1. Analise a implementação do software e **derive o grafo de fluxo de controle** a partir do código.
2. **Calcule a Complexidade Ciclomática (C)** do grafo com a fórmula C = arestas - nós + 2. Se todas as decisões do código forem puramente binárias (com apenas duas saídas), também é possível usar a fórmula C = decisões_binárias + 1.
3. **Selecione um conjunto de $C$ caminhos básicos**. Inicie traçando um caminho "típico" desde a entrada até a saída como sua linha de base (*baseline path*). Em seguida, trace novos caminhos alterando sistematicamente a rota de uma decisão por vez ao longo dessa linha de base.
4. **Crie os casos de teste** e escolha os dados de entrada necessários para "sensibilizar" o código (ou seja, valores de dados que farão o programa ser forçado a trilhar o caminho exato que você mapeou).
5. Estipule os resultados esperados e **execute os testes**.

## Exemplos práticos

Um exemplo prático seria visualizar um sistema com a instrução `if (a>0 && c==1) {x=x+1;}`. Embora um leigo possa ver apenas uma "linha de decisão", a técnica ensina a observar o fluxo físico e lógico que o compilador usará para chegar ao resultado dessa instrução. O testador precisaria projetar variáveis exatas para mapear todas as ramificações de ambas as variáveis em separado para garantir uma "cobertura de múltiplas condições". O testador criaria casos práticos de simulação com conjuntos de variáveis forçadas em Verdadeiro/Verdadeiro (a>0, c=1), Verdadeiro/Falso, Falso/Verdadeiro e Falso/Falso.

[Consulte os outros exemplos de como usar no documento](../../../examples/white-box-testing-techniques/control-flow-testing/readme.md)
