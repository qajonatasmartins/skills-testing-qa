# Boundary Value Testing

A técnica de **Boundary Value Testing** (Teste de Valor Limite) é um método utilizado para reduzir o número de casos de teste a um tamanho gerenciável e, ao mesmo tempo, manter uma cobertura de testes razoável. Esta técnica complementa e expande o conceito do teste de classes de equivalência.

## Princípio básico

O princípio básico fundamenta-se no fato de que **os defeitos se escondem com muito mais frequência nos limites das classes de equivalência**. Testadores experientes sabem intuitivamente que erros acontecem muito nos extremos, e isso geralmente ocorre porque programadores costumam cometer falhas ao codificar testes de desigualdade (por exemplo, escrevendo erroneamente `>` (maior que) em vez de `>=` (maior ou igual)).

## Quando usar

A técnica deve ser usada **quando a entrada consiste em um intervalo contínuo ou discreto de valores**. Também é aplicável para testar limites de **estrutura dos dados**, como o comprimento ou a quantidade de caracteres numéricos permitidos. Ela é versátil e pode ser aplicada em **todos os níveis de teste** (unidade, integração, sistema e aceitação), exigindo apenas que os requisitos do sistema permitam particionar as entradas e identificar seus limites.

## Quando não usar

Como a técnica exige que as entradas possam ser particionadas e que seus limites lógicos sejam identificáveis com base nos requisitos, **ela não deve ser usada se o sistema não possuir entradas particionáveis ou limites definidos**. Além disso, se o comportamento do sistema depender de interações lógicas simultâneas entre múltiplas variáveis restritivas (onde o valor de uma altera o limite aceitável de outra), usar o teste de valor limite isoladamente pode deixar lacunas. Para cenários com variáveis dependentes, recomenda-se uma técnica mais abrangente, como a Análise de Domínio (Domain Analysis Testing).

## Como usar

A aplicação envolve três passos simples:

1. **Identifique as classes de equivalência**.
2. **Identifique os limites** de cada classe.
3. **Crie casos de teste para cada valor limite escolhendo três pontos específicos**: um ponto exatamente **sobre** o limite, um ponto logo **abaixo** do limite e um ponto logo **acima** do limite.

Os termos "abaixo" e "acima" são relativos e dependem exclusivamente da unidade do valor que está sendo testado. Por exemplo, se a variável é a idade (números inteiros) e o limite é 16, os três testes seriam 15, 16 e 17; se a variável for monetária (dólares e centavos) e o limite for $5.00, os testes seriam $4.99, $5.00 e $5.01. Caso o ponto testado logo acima ou abaixo já pertença a uma classe de equivalência diferente sendo testada separadamente, não há motivo para duplicar esse teste.

[Consulte os outros exemplos de como usar no documento](../../../examples/black-box-testing-techniques/boundary-value-testing/readme.md)
