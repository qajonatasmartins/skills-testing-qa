# Equivalence Class Testing

A técnica de **Teste de Classe de Equivalência** (*Equivalence Class Testing*) é um método fundamental de caixa preta projetado para reduzir o número de casos de teste a uma quantidade gerenciável, ao mesmo tempo em que mantém uma cobertura de testes razoável.

## Princípio básico

A técnica baseia-se na ideia de que um conjunto (ou classe) de dados será tratado exatamente da mesma maneira pelo sistema e produzirá o mesmo resultado. Portanto, se um teste dentro dessa classe de equivalência revelar um defeito, todos os outros valores dessa classe provavelmente revelarão o mesmo defeito; inversamente, se um teste não encontrar um erro, nenhum outro valor da mesma classe o fará.

## Quando usar

- Você deve usar essa técnica em sistemas onde os dados de entrada assumem valores dentro de intervalos (ranges) específicos ou dentro de conjuntos pré-definidos.
- Ela pode e deve ser aplicada em **todos os níveis de teste**: teste de unidade, de integração, de sistema e de aceitação, exigindo apenas que os requisitos do sistema permitam o particionamento das entradas ou saídas.

## Quando não usar

Você NÃO deve usar (ou deve usar em conjunto com outras técnicas) nas seguintes situações descritas:

- **Para testar as fronteiras/limites dos dados**: A Partição de Equivalência foca em pegar um valor "típico" dentro de uma classe, mas a maior parte dos defeitos se esconde exatamente nas bordas dessas classes. Neste caso, você não deve usar apenas a Partição de Equivalência; ela deve servir como base para aplicar a técnica de Teste de Valor Limite (Boundary Value Testing), testando os pontos exatos de transição.
- **Sistemas que dependem de estados passados**: Se o sistema precisa "lembrar" o que aconteceu antes (por exemplo, um sistema de reservas que tem prazos e reage diferente se o cliente já pagou ou não), particionar apenas a entrada atual não funcionará. Para isso, deve-se usar o Teste de Transição de Estado (State-Transition Testing).
- **Quando múltiplas variáveis interagem logicamente**: Se o valor de uma variável restringe os valores aceitáveis de outra variável, testar as classes de equivalência delas individualmente pode não revelar defeitos de interação. Nessas situações, o Teste de Análise de Domínio (Domain Analysis Testing) para tratar as fronteiras multidimensionais.
- **Regras de negócios e lógicas complexas**: Se as funcionalidades dependem do cruzamento de muitas condições diferentes para tomar uma decisão (ex: aprovar um crédito dependendo da idade, salário, histórico e estado civil simultaneamente), a técnica falha em organizar as combinações. O correto é usar o Teste de Tabela de Decisão (Decision Table Testing).
- **Explosão combinatória de dezenas de variáveis**: Se o sistema interage com muitas variáveis com poucas opções cada (ex: testar um software em 8 navegadores, 3 sistemas operacionais e 3 servidores web diferentes), tentar cruzar todas as partições seria impossível. Para isso, use o Teste em Pares (Pairwise Testing).

## Como usar (Passo a passo)

1. **Identifique as classes de equivalência:** Particione os dados de entrada em classes válidas (que o sistema deve aceitar) e classes inválidas (que o sistema deve rejeitar).
   1. Por exemplo, se a entrada é um intervalo contínuo, você geralmente terá uma classe de valores válidos e duas classes de valores inválidos (uma abaixo do limite e outra acima).
2. **Crie os casos de teste:** Selecione apenas um valor para representar cada classe de equivalência.
   1. **Importante**: Embora possa parecer mais seguro criar vários testes para a mesma classe, selecionar mais de um valor da classe para representar a classe de equivalência, raramente vai descobrir defeitos adicionais, apenas desperdiça tempo e dinheiro.
3. **Teste dados inválidos isoladamente:** Ao testar dados de classes inválidas, **teste um valor inválido de cada vez** misturado com dados válidos. Colocar múltiplos dados inválidos em um único caso de teste é uma má prática, pois o sistema pode rejeitar a transição no primeiro erro encontrado e mascarar as outras falhas de validação.

## Exemplo Prático

Considere um sistema de Recursos Humanos que determina as regras de contratação baseadas na idade do candidato, conforme os seguintes requisitos:

- 0–16 anos: Não contratar
- 16–18 anos: Pode contratar apenas meio período
- 18–55 anos: Pode contratar em período integral
- 55–99 anos: Não contratar

Em vez de criar casos de teste para cada idade individual de 0 até 99 (o que exigiria 100 testes), você identifica as classes de equivalência formadas por essas faixas e testa apenas um valor dentro de cada uma.

Dessa forma, o esforço é reduzido de 100 casos de teste para apenas **quatro testes válidos**, usando, por exemplo, as idades: 10, 17, 30 e 70.

Para testar entradas da **classe de equivalência inválida**, você deve perguntar como o sistema foi projetado. Se ele aceita qualquer dado e gera mensagens de erro (*defensive design*), você precisaria de testes para idades abaixo de 0 (ex: -5), acima de 99 (ex: 150) e até mesmo entradas textuais inválidas (ex: "FRED" ou "#$@").

[Consulte os outros exemplos de como usar no documento](../../../examples/black-box-testing-techniques/equivalence-class-testing/readme.md)
