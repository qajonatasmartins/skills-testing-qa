# Chain-of-Thought Prompting (CoT)

## O que é
Técnica que solicita ao modelo mostrar os passos intermediários de raciocínio antes de chegar
à resposta final. Em vez de pular direto para a conclusão, o modelo "pensa em voz alta",
decompondo o problema em etapas menores. Inclui duas variantes principais:
- **Few-Shot CoT**: exemplos com raciocínio explícito incluídos no prompt
- **Zero-Shot CoT**: adicionar "Vamos pensar passo a passo" sem exemplos

Melhora significativamente resultados em tarefas de raciocínio aritmético, lógico e de senso comum.

## Quando usar
- Problemas matemáticos ou aritméticos
- Raciocínio lógico com múltiplas premissas
- Tarefas que exigem senso comum ou inferência
- Problemas com múltiplas etapas de processamento
- Quando o modelo erra por "pular" etapas de raciocínio
- Depuração de por que o modelo chegou a determinada conclusão

## Quando NÃO usar
- Tarefas simples de classificação ou extração (overhead desnecessário)
- Quando velocidade/economia de tokens é prioridade
- Tarefas puramente criativas sem resposta "correta"
- Quando o modelo já acerta consistentemente sem CoT

## Template / Estrutura

### Few-Shot CoT

```
{{ instrução da tarefa }}

Pergunta: {{ exemplo_pergunta_1 }}
Raciocínio: {{ passos intermediários do exemplo 1 }}
Resposta: {{ resposta_exemplo_1 }}

Pergunta: {{ exemplo_pergunta_2 }}
Raciocínio: {{ passos intermediários do exemplo 2 }}
Resposta: {{ resposta_exemplo_2 }}

Pergunta: {{ nova_pergunta }}
Raciocínio:
```

### Zero-Shot CoT

```
{{ pergunta ou problema }}

Vamos pensar passo a passo.
```

## Exemplo prático

### Exemplo 1 — Few-Shot CoT (aritmético)

**Prompt:**
```
Resolva o problema mostrando o raciocínio.

Pergunta: João tem 5 maçãs. Ele compra 3 sacolas com 4 maçãs cada. Quantas maçãs ele tem?
Raciocínio: João começa com 5 maçãs. Ele compra 3 sacolas × 4 maçãs = 12 maçãs. Total: 5 + 12 = 17 maçãs.
Resposta: 17

Pergunta: Uma loja tinha 120 camisetas. Vendeu 1/3 na segunda e 1/4 do restante na terça. Quantas sobraram?
Raciocínio:
```

**Resposta esperada:**
```
Raciocínio: A loja tinha 120 camisetas. Na segunda vendeu 1/3 de 120 = 40 camisetas. Restaram 120 - 40 = 80 camisetas. Na terça vendeu 1/4 de 80 = 20 camisetas. Sobraram 80 - 20 = 60 camisetas.
Resposta: 60
```

### Exemplo 2 — Zero-Shot CoT (lógica)

**Prompt:**
```
Em um estacionamento há 15 carros. 3 carros saem e o dobro desse número entra.
Depois, metade dos carros do estacionamento sai. Quantos carros restam?

Vamos pensar passo a passo.
```

**Resposta esperada:**
```
1. Início: 15 carros
2. 3 carros saem: 15 - 3 = 12 carros
3. O dobro de 3 (= 6) entra: 12 + 6 = 18 carros
4. Metade sai: 18 / 2 = 9 carros

Restam 9 carros.
```

## Dicas
- Zero-Shot CoT ("Vamos pensar passo a passo") é surpreendentemente eficaz e simples
- Few-Shot CoT é mais confiável quando os problemas seguem um padrão específico
- A qualidade do raciocínio nos exemplos afeta diretamente o modelo
- Combine com Self-Consistency para maior robustez

## Fonte
https://www.promptingguide.ai/pt/techniques/cot
