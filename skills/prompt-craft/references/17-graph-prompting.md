# Graph Prompting

## O que é
Framework para aplicar técnicas de prompting a dados estruturados como grafos
(nós e arestas). Em vez de trabalhar apenas com texto linear, o Graph Prompting
permite que LLMs raciocinem sobre entidades interconectadas, relacionamentos,
hierarquias e redes. Isso é relevante porque muitos problemas do mundo real
envolvem estruturas de grafo: redes sociais, dependências de código, ontologias,
fluxos de processo e knowledge graphs. A técnica explora como representar,
descrever e consultar grafos via prompts textuais.

## Quando usar
- Análise de dependências entre componentes de software
- Raciocínio sobre hierarquias organizacionais ou de dados
- Tarefas envolvendo knowledge graphs ou ontologias
- Detecção de ciclos, caminhos ou clusters em relacionamentos
- Análise de impacto (o que é afetado se X mudar)
- Mapeamento de relacionamentos entre entidades de negócio

## Quando NÃO usar
- Dados puramente tabulares sem relacionamentos (use análise tabular)
- Grafos muito grandes para caber na janela de contexto
- Quando uma ferramenta de grafos dedicada (Neo4j, NetworkX) seria mais eficiente
- Tarefas que não envolvem relacionamentos entre entidades

## Template / Estrutura

### Representação textual do grafo

```
Considere o seguinte grafo:

Nós: {{ lista_de_nós }}
Arestas:
- {{ nó_A }} → {{ nó_B }} ({{ tipo_relação }})
- {{ nó_B }} → {{ nó_C }} ({{ tipo_relação }})
- {{ nó_C }} → {{ nó_A }} ({{ tipo_relação }})

Pergunta: {{ pergunta sobre o grafo }}
```

### Template com adjacência

```
Mapa de relacionamentos:

{{ entidade_1 }}:
  - conecta com {{ entidade_2 }} via {{ relação }}
  - conecta com {{ entidade_3 }} via {{ relação }}

{{ entidade_2 }}:
  - conecta com {{ entidade_4 }} via {{ relação }}

Tarefa: {{ o que analisar no grafo }}
```

### Template para análise de impacto

```
Grafo de dependências:

{{ componente_1 }} depende de: {{ dep_1 }}, {{ dep_2 }}
{{ componente_2 }} depende de: {{ dep_3 }}, {{ componente_1 }}
{{ componente_3 }} depende de: {{ componente_1 }}, {{ componente_2 }}

Pergunta: Se {{ componente_X }} falhar, quais componentes são afetados?
Raciocine seguindo as dependências do grafo.
```

## Exemplo prático

### Exemplo 1 — Análise de dependências de microsserviços

**Prompt:**
```
Grafo de dependências dos microsserviços:

Nós: [API Gateway, Auth Service, User Service, Payment Service, Notification Service, Database]
Arestas:
- API Gateway → Auth Service (autentica)
- API Gateway → User Service (roteia)
- API Gateway → Payment Service (roteia)
- Auth Service → Database (consulta tokens)
- User Service → Database (consulta usuários)
- Payment Service → Auth Service (valida token)
- Payment Service → Notification Service (envia alerta)
- Notification Service → User Service (busca e-mail)

Pergunta: Se o Database ficar indisponível, quais serviços são diretamente
afetados e quais são indiretamente afetados? Qual o impacto para o usuário final?
```

**Resposta esperada:**
```
Análise de impacto — Database indisponível:

Diretamente afetados (dependem do Database):
- Auth Service: não consegue validar tokens
- User Service: não consegue consultar dados de usuários

Indiretamente afetados (via cadeia de dependência):
- API Gateway: falha na autenticação (via Auth Service)
- Payment Service: falha na validação de token (via Auth Service)
- Notification Service: falha ao buscar e-mail (via User Service)

Impacto para o usuário:
- Login impossível (Auth Service → Database)
- Pagamentos bloqueados (Payment → Auth → Database)
- Notificações não enviadas (Notification → User → Database)
- Severidade: TOTAL — sistema completamente indisponível
```

### Exemplo 2 — Detecção de dependência circular

**Prompt:**
```
Verifique se há dependências circulares no grafo:

módulo-A depende de: módulo-B, módulo-C
módulo-B depende de: módulo-D
módulo-C depende de: módulo-E
módulo-D depende de: módulo-E
módulo-E depende de: módulo-A

Existe ciclo? Se sim, qual o caminho?
```

**Resposta esperada:**
```
Sim, há um ciclo:
módulo-A → módulo-C → módulo-E → módulo-A

Também por outro caminho:
módulo-A → módulo-B → módulo-D → módulo-E → módulo-A

Recomendação: Quebrar a dependência de módulo-E para módulo-A
extraindo a funcionalidade compartilhada para um módulo-F independente.
```

## Dicas
- Represente grafos de forma textual clara (lista de adjacência é mais legível que matriz)
- Para grafos grandes, foque no subgrafo relevante para a pergunta
- Peça raciocínio explícito sobre travessia de caminhos
- Combine com CoT para análise de caminhos e impacto
- Para grafos muito grandes, use ferramentas dedicadas e passe apenas o resultado relevante

## Fonte
https://www.promptingguide.ai/pt/techniques/graph
