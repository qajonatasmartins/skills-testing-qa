# Domain Analysis Testing

Serve para identificar casos de teste quando múltiplas variáveis interagem e precisam ser testadas simultaneamente, generalizando os conceitos de equivalência e limite para cenários multidimensionais.

## Princípio básico

O *Domain Analysis Testing* é uma técnica que constrói e generaliza os conceitos de testes de classes de equivalência e de valor limite, aplicando-os em múltiplas dimensões simultâneas (variáveis que interagem entre si). O objetivo principal desta técnica é procurar falhas onde as fronteiras (os limites lógicos entre as variáveis) foram definidas ou implementadas incorretamente no código. Em dimensões simultâneas, esses defeitos costumam aparecer como fronteiras deslocadas (horizontalmente ou verticalmente), fronteiras inclinadas em ângulos errados, fronteiras ausentes ou fronteiras inseridas de forma extra.

## Quando usar

O *Domain Analysis Testing* deve ser utilizado sempre que múltiplas variáveis, como campos de entrada, precisarem ser testadas juntas devido à eficiência ou porque interagem logicamente. Ele é especialmente necessário quando o valor aceitável de uma variável é restrito ou condicionado pelo valor de outra variável. Se testadas separadamente nesses cenários, certos defeitos de fronteira jamais seriam descobertos. Como raramente temos tempo para criar testes para cada variável do sistema individualmente, testar as variáveis interagentes de forma simultânea poupa recursos e cobre regras de negócio mais complexas.

## Quando não usar

Você não deve usar essa técnica quando as variáveis do sistema são completamente independentes e não possuem nenhuma interação lógica entre si. Além disso, a análise de domínio é mais adequada para valores numéricos, sendo menos eficaz (embora adaptável) em casos que não envolvem faixas contínuas, mesmo podendo ser generalizada para booleanos, strings e enumerações.

## Como usar

A técnica baseia-se em identificar as condições (fronteiras) e definir pontos de teste de forma estratégica:

- **Identifique os tipos de pontos:**
  - **On point**: um valor que fica exatamente em cima da fronteira.
  - **Off point**: um valor que não fica na fronteira.
  - **In point**: um valor que satisfaz as condições da fronteira, mas não fica sobre ela (um ponto válido e típico).
  - **Out point**: um valor que não satisfaz as condições da fronteira e também não está nela (um ponto totalmente inválido).

- **Analise fronteiras fechadas vs abertas:** Se a fronteira é definida por >=, <= ou =, o **on point** pertence ao domínio e o ***off point*** fica do lado de fora. Se a fronteira for definida por < ou >, o **on point** fica de fora do domínio e o **off point** fica do lado de dentro.

- **Gere casos de teste (Técnica 1x1):** Para cada condição relacional (<=, >=, <, >), escolha um **on point** e um **off point**. Para condições de igualdade estrita (=), escolha um **on point** e dois *off points* (um ligeiramente menor e outro ligeiramente maior).

- **Matriz de Teste de Domínio:** Ao testar os pontos *on* e **off** de uma variável, você deve manter as outras variáveis do teste num ponto típico (um **in point**) e não precisa repetir testes para domínios adjacentes.

## Exemplos práticos

Para entender a escolha de pontos, considere que temos um limite em que uma variável `X` precisa ser >= 10 (fronteira fechada). O **on point** é exatamente 10; o **off point** (que fica fora do domínio) seria 9; um **in point** (típico) seria 15; e um **out point** (inválido) seria 5. Se a fronteira fosse aberta, como `X > 10`, o **on point** continuaria sendo 10 (agora inválido), e o **off point** (agora dentro do domínio) seria 11.

Ao aplicar isso para duas variáveis (`X1` e `X2`), você criaria testes variando `X1` em seus pontos críticos (*on* e *off*) enquanto usa um valor típico seguro para `X2`. Depois, faria o oposto, variando os pontos limites de `X2` enquanto mantém um valor seguro e constante para `X1`.

[Consulte os outros exemplos de como usar no documento](../../../examples/black-box-testing-techniques/domain-analysis-testing/readme.md)
