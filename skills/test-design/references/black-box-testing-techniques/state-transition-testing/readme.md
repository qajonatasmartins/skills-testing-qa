# State-Transition Testing

Ideal para sistemas que precisam armazenar um histórico do que aconteceu (estados) e reagir de maneira diferente a eventos dependendo do seu estado atual.

## Princípio básico

A técnica de "State-Transition Testing" (Teste de Transição de Estados) é uma técnica de teste de projeto de caixa-preta na qual os casos de teste são criados para executar transições válidas e inválidas em um sistema. **A técnica modela o sistema ou seus componentes como uma Máquina de Estados Finitos (FSM), cujo comportamento e os resultados dependem não apenas das entradas atuais, mas também das entradas passadas (o histórico de eventos)**. Nesse modelo, o sistema assume um determinado estado que persiste até que ocorra um evento; o evento aciona uma transição para um novo estado, gerando, possivelmente, uma saída ou ação correspondente.

## Quando usar

O teste baseado em estados é **ideal quando lidamos com lógicas de negócios e sequências de eventos onde o tratamento adequado de uma ação atual depende das condições ocorridas no passado**. Esta técnica é altamente recomendada e muito utilizada para:

- Aplicações baseadas em menus de navegação.
- Softwares orientados a objetos, visto que na orientação a objetos o comportamento dos métodos depende diretamente do estado atual da instância (atributos) do objeto.
- Teste de protocolos de rede e comunicação.
- Softwares de instalação, drivers de dispositivos (hardwares) e sistemas de backup e recuperação.
- Aplicações web, transacionais e interfaces de interface gráfica de usuário com diferentes instâncias.

## Quando não usar

Os diagramas e testes de transição de estado não são aplicáveis quando o sistema não possui um estado interno ou não precisa responder a eventos em tempo real provenientes do mundo externo. **Um exemplo prático de quando não usar é um programa de folha de pagamento simples que apenas lê o registro de ponto de um funcionário, calcula o salário, salva o registro, imprime o contracheque e repete o processo**. Além disso, a técnica também não deve ser usada para testar gráficos de estados imensos e não aninhados (como controles de centrais telefônicas com dezenas de milhares de estados), pois o volume de transições torna a abordagem cara e quase impossível de ser conduzida.

## Como usar

A utilização da técnica segue um processo estruturado a partir da especificação do software:

1. **Identifique e defina os estados** possíveis em que o componente ou o sistema pode estar.
2. **Defina as transições** entre esses estados, identificando os eventos (ou gatilhos) e as condições que causam essas mudanças, bem como as ações de saída esperadas.
3. **Crie um Mapa/Diagrama de Transição de Estados ou uma Tabela de Transição de Estados**. Os diagramas facilitam a visualização dos caminhos válidos, enquanto as tabelas de decisão forçam o testador a cobrir de modo analítico as combinações inválidas ou não documentadas.
4. **Defina os cenários e casos de teste** criando caminhos através do modelo. Adote a regra de que o teste deve iniciar em um estado inicial definido e terminar em um estado final (ou retornar ao estado de origem).
5. Escolha o **nível de cobertura** desejado: testar apenas todos os estados, testar todas as transições (nível geralmente recomendado), cobrir "N-switch" (sequências encadeadas de N+1 transições) ou todas as rotas/caminhos (o que pode ser infinito se houver loops).
6. Em sistemas de alto risco, teste também as combinações de estado e eventos inválidas (caminhos furtivos ou "sneak paths") para confirmar que o sistema as rejeita corretamente.

## Exemplos práticos

- **Abertura e Fechamento de Arquivo no SO:** Uma aplicação gráfica (como o Windows Paint) possui seu "estado de startup" e depois transita entre os estados de um documento: "limpo" (clean) e "sujo" (dirty). O estado é alterado conforme edições são feitas no software. Se o usuário tenta fechar o software no estado limpo, ele simplesmente se encerra; se estiver no estado sujo, o sistema muda o comportamento e questiona se o usuário deseja salvar.
- **Reserva de Passagens Aéreas:** Um cliente tenta fazer uma reserva. O sistema cria a reserva que fica no estado "Feita" (Made) e inicia um timer. Se o cliente insere o pagamento ("payMoney"), a reserva vai para "Paga" (Paid). Contudo, se a transição "PayTimerExpires" (o timer expira) for acionada enquanto está no estado "Made", o sistema muda o estado para "Cancelada por Não Pagamento".
- **Pilha (Stack) em Programação:** Uma pilha começa no estado "Inicial" (antes da criação), passa para "Vazia" (Empty) após criada. O evento de "adicionar" a move para "Em espera" (Holding) ou "Cheia" (Full) dependendo da capacidade. Uma tentativa de adicionar dados na Pilha já no estado "Cheia" dispara uma transição de estado inválida que deve gerar uma exceção no teste.

[Consulte os outros exemplos de como usar no documento](../../../examples/black-box-testing-techniques/state-transition-testing/readme.md)
