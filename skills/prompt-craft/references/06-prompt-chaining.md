# Prompt Chaining (Encadeamento de Prompts)

## O que é
Técnica que decompõe uma tarefa complexa em uma sequência de subtarefas menores,
onde a saída de um prompt se torna a entrada do próximo. Cada elo da cadeia
realiza uma transformação específica, tornando o processo mais confiável,
transparente e fácil de depurar. Se um elo falha, é possível identificar
e corrigir exatamente onde o problema ocorreu.

## Quando usar
- Tarefas complexas com múltiplas etapas distintas
- Quando um único prompt longo produz resultados inconsistentes
- Processos que exigem transformações sequenciais (ex: extrair → analisar → formatar)
- Quando se precisa de checkpoints intermediários para validação
- Pipelines de processamento de dados com LLM
- Tarefas onde a depuração (debuggability) é importante

## Quando NÃO usar
- Tarefas simples resolvíveis em um único prompt
- Quando a latência entre chamadas é uma restrição crítica
- Se as subtarefas são tão interdependentes que não podem ser separadas
- Quando o custo de múltiplas chamadas de API é proibitivo

## Template / Estrutura

```
CADEIA DE PROMPTS:

Prompt 1 ({{ nome_etapa_1 }}):
  Entrada: {{ entrada_original }}
  Instrução: {{ o que fazer nesta etapa }}
  Saída esperada: {{ formato_saída_1 }}

        ↓ (saída_1 alimenta prompt 2)

Prompt 2 ({{ nome_etapa_2 }}):
  Entrada: {{ saída_1 }}
  Instrução: {{ o que fazer nesta etapa }}
  Saída esperada: {{ formato_saída_2 }}

        ↓ (saída_2 alimenta prompt 3)

Prompt 3 ({{ nome_etapa_3 }}):
  Entrada: {{ saída_2 }}
  Instrução: {{ o que fazer nesta etapa }}
  Saída esperada: {{ resultado_final }}
```

## Exemplo prático

### Exemplo — Análise de feedback de clientes

**Prompt 1 (Extração):**
```
Extraia todas as reclamações mencionadas no feedback abaixo.
Liste cada reclamação em um bullet point separado.

Feedback: "O app é bonito mas muito lento. Tentei fazer um Pix e demorou
30 segundos pra carregar. Além disso, não achei onde consultar minha fatura
e quando perguntei no chat o bot não entendeu nada do que eu disse."
```

**Saída 1:**
```
- App lento (30s para carregar Pix)
- Dificuldade em encontrar consulta de fatura
- Chatbot não entende as mensagens do usuário
```

**Prompt 2 (Classificação):**
```
Para cada reclamação abaixo, classifique a área responsável e a severidade (alta/média/baixa).

Reclamações:
- App lento (30s para carregar Pix)
- Dificuldade em encontrar consulta de fatura
- Chatbot não entende as mensagens do usuário

Responda em formato de tabela.
```

**Saída 2:**
```
| Reclamação                        | Área         | Severidade |
|-----------------------------------|--------------|------------|
| App lento (30s para Pix)          | Performance  | Alta       |
| Fatura difícil de encontrar       | UX/Design    | Média      |
| Chatbot ineficaz                  | IA/Chatbot   | Alta       |
```

**Prompt 3 (Plano de ação):**
```
Com base na análise abaixo, gere um plano de ação priorizado com responsável sugerido.

| Reclamação                        | Área         | Severidade |
|-----------------------------------|--------------|------------|
| App lento (30s para Pix)          | Performance  | Alta       |
| Fatura difícil de encontrar       | UX/Design    | Média      |
| Chatbot ineficaz                  | IA/Chatbot   | Alta       |
```

**Saída final:**
```
Plano de Ação (por prioridade):

1. [URGENTE] Performance do Pix — Investigar gargalo no carregamento.
   Responsável: Time de Backend/Infra
2. [URGENTE] Chatbot — Retreinar modelo de NLU com exemplos reais.
   Responsável: Time de IA
3. [MÉDIO] Navegação da fatura — Redesenhar fluxo de acesso à fatura.
   Responsável: Time de Produto/UX
```

## Dicas
- Cada prompt da cadeia deve ter **uma única responsabilidade** clara
- Valide a saída de cada etapa antes de alimentar a próxima
- Use formatação estruturada (JSON, tabelas) entre etapas para reduzir ambiguidade
- Inclua instruções de formato explícitas em cada prompt
- Considere adicionar um prompt de "verificação" ao final da cadeia

## Fonte
https://www.promptingguide.ai/pt/techniques/prompt_chaining
