# Tree of Thoughts (ToT)

## O que é
Framework que generaliza o Chain-of-Thought ao permitir que o modelo explore
múltiplos caminhos de raciocínio em uma estrutura de árvore. Em cada nó,
o modelo gera possíveis "pensamentos" (passos intermediários), avalia quais
são promissores e pode fazer backtracking (voltar atrás) em caminhos sem futuro.
Combina geração de pensamentos com busca deliberada (BFS ou DFS) e auto-avaliação.
Ideal para problemas que exigem exploração, planejamento estratégico ou tentativa e erro.

## Quando usar
- Problemas que exigem planejamento com múltiplos caminhos possíveis
- Puzzles lógicos e jogos de estratégia
- Escrita criativa com restrições complexas
- Tarefas onde o primeiro caminho de raciocínio raramente é o melhor
- Problemas de otimização ou decisão com trade-offs
- Quando se precisa de justificativa para descartar alternativas

## Quando NÃO usar
- Tarefas simples onde CoT linear já funciona bem
- Quando o custo de tokens/latência é uma restrição forte
- Perguntas factuais diretas
- Tarefas sem "caminhos alternativos" significativos
- Processamento em lote de alto volume

## Template / Estrutura

### Template dos 3 Especialistas

```
Imagine 3 especialistas diferentes resolvendo este problema.
Cada um propõe uma abordagem, avalia as dos outros e descarta as piores.

Problema: {{ descrição do problema }}

Rodada 1 — Propostas iniciais:
Especialista 1: {{ pensamento_1 }}
Especialista 2: {{ pensamento_2 }}
Especialista 3: {{ pensamento_3 }}

Rodada 2 — Avaliação e refinamento:
Cada especialista avalia as propostas e indica qual caminho é mais promissor.
Descarte o caminho menos viável e refine os melhores.

Rodada 3 — Convergência:
Combine os melhores elementos e apresente a solução final.
```

### Template genérico com avaliação

```
Problema: {{ problema }}

Passo 1: Gere 3 abordagens diferentes para resolver este problema.
Para cada abordagem, avalie em uma escala de 1-10:
- Viabilidade
- Completude
- Elegância

Passo 2: Selecione a melhor abordagem e desenvolva-a em detalhes.
Se encontrar um obstáculo, volte e tente a segunda melhor.

Passo 3: Apresente a solução final com justificativa.
```

## Exemplo prático

### Exemplo — Planejamento de arquitetura

**Prompt:**
```
Imagine 3 arquitetos de software resolvendo este problema.
Cada um propõe uma abordagem, depois avaliam as propostas dos colegas.

Problema: Precisamos implementar um sistema de notificações que envie alertas
por e-mail, push e SMS quando uma transação financeira exceder R$ 5.000.
O sistema precisa ser escalável e tolerante a falhas.

Rodada 1 — Cada arquiteto propõe sua abordagem:

Arquiteto 1:
Arquiteto 2:
Arquiteto 3:

Rodada 2 — Cada um avalia as propostas e descarta a menos viável:

Rodada 3 — Solução final convergente:
```

**Resposta esperada (resumida):**
```
Rodada 1:
Arquiteto 1: Fila de mensagens (SQS/RabbitMQ) com workers por canal.
Arquiteto 2: Event-driven com pub/sub (Kafka) e microserviços por canal.
Arquiteto 3: Orquestrador central com circuit breaker e retry.

Rodada 2:
- Arquiteto 3 descartado: orquestrador central é ponto único de falha.
- Arquiteto 1 e 2 refinados: fila + event-driven complementares.

Rodada 3:
Solução: Event-driven com Kafka para ingestão → filas SQS por canal
(e-mail, push, SMS) → workers independentes com retry e dead-letter queue.
Monitoramento com métricas por canal. Escalável horizontalmente.
```

## Dicas
- O template dos 3 especialistas é a forma mais prática de aplicar ToT em prompts
- Use para decisões arquiteturais, planejamento de projetos, resolução de puzzles
- Explicite que os especialistas devem **criticar** e **descartar** abordagens
- Funciona melhor com modelos mais capazes

## Fonte
https://www.promptingguide.ai/pt/techniques/tot
