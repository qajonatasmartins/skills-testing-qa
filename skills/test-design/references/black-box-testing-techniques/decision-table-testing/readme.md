# Decision Table Testing

Excelente para capturar e documentar regras de negócios complexas. Utiliza tabelas que cruzam combinações de condições de entrada com as ações esperadas do sistema.

## Princípio básico

As Tabelas de Decisão (Decision Tables) são uma excelente ferramenta para capturar requisitos e documentar o design interno de um sistema, representando regras de negócios complexas baseadas em um conjunto de condições. A estrutura divide-se em duas partes principais: **Condições** (que representam as várias entradas do sistema) e **Ações** (os processos que devem ser executados com base nessas entradas). Cada coluna na tabela é chamada de "Regra", e cada regra define uma combinação única de condições que resultará no disparo (*firing*) das ações associadas àquela regra.

## Quando usar

O Teste de Tabela de Decisão deve ser usado sempre que o sistema precisar implementar **regras de negócios complexas**, desde que essas regras possam ser representadas como uma combinação de condições independentes e tenham ações discretas associadas a elas.

## Quando não usar

Você não deve usar tabelas de decisão quando as ações do sistema dependerem de entradas anteriores ou do estado atual/histórico do sistema. Nestes casos em que o sistema precisa "lembrar" do que aconteceu antes ou possui uma ordem válida/inválida de operações, a técnica de Diagramas de Transição de Estado (*State-Transition Diagrams*) é a ferramenta adequada. Além disso, se o sistema não possuir regras de negócios complexas lógicas, a técnica pode ser desnecessária.

## Como usar

A utilização das tabelas de decisão para criar casos de teste é bastante direta:

- **Crie pelo menos um caso de teste para cada regra:** Cada coluna vertical da tabela se torna um caso de teste, onde as Condições especificam as entradas e as Ações especificam os resultados esperados. Para fazer isso, basta alterar os cabeçalhos da tabela, substituindo "Regra 1, Regra 2" por "Caso de Teste 1, Caso de Teste 2".
- **Tratamento de Condições:** Se a condição for binária (ex: Sim/Não), um único teste para cada combinação é suficiente. Se a condição for um intervalo de valores numéricos, recomenda-se combinar esta técnica com o Teste de Valor Limite (*Boundary Value Testing*), testando dados tanto na extremidade inferior quanto na superior do intervalo.
- **Evite Tabelas Colapsadas ("Don't Care"):** Frequentemente, analistas simplificam as tabelas agrupando condições que não afetam o resultado final e marcando-as como "DC" (*Don't Care* ou Não Importa). Embora seja bom para desenvolvedores escreverem menos código, **para testadores isso é perigoso**. O código pode ter sido escrito incorretamente, portanto, a tabela original (não-colapsada) deve ser sempre usada como base para a criação dos testes.

## Exemplos práticos

Na prática, o processo envolve transformar lógicas com múltiplos "If/Else" em uma matriz tabular. Se uma condição não for binária (por exemplo, "Idade < 5", "Idade = 5" ou "Idade > 7"), o testador não testa a palavra "Sim" ou "Não", mas seleciona **valores reais que satisfaçam a condição** (como 0, 5 ou 500) para criar seus casos de teste práticos e garantir que a ação correspondente seja disparada com precisão.

[Consulte os outros exemplos de como usar no documento](../../../examples/black-box-testing-techniques/decision-table-testing/readme.md)
