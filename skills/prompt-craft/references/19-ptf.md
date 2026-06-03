# PTF (Papel, Tarefa e Formato)

## O que é
Framework simples e direto de engenharia de prompt que estrutura a instrução em três componentes: o **Papel** que a IA deve assumir, a **Tarefa** que deve executar e o **Formato** esperado na saída. Ideal para quem está começando com prompt engineering por ser intuitivo e fácil de memorizar.

## Quando usar
- Tarefas com escopo bem definido onde atribuir uma persona já direciona a qualidade da resposta.
- Geração de conteúdo com formato de saída específico (lista, tabela, relatório, e-mail).
- Consultas técnicas onde o papel de especialista agrega profundidade.
- Situações em que simplicidade e velocidade de escrita do prompt importam.

## Quando NÃO usar
- Tarefas que exigem raciocínio multi-etapa ou cadeia lógica (prefira Chain-of-Thought ou PEPE).
- Cenários que dependem de exemplos concretos para calibrar o tom (prefira Few-Shot ou CARE).
- Problemas abertos ou criativos sem formato de saída claro.
- Quando o contexto do domínio é extenso e precisa ser fornecido explicitamente.

## Template / Estrutura

```
Como especialista em {{ papel }}
Seu objetivo é {{ tarefa }}
No formato {{ formato }}
```

| Componente | Descrição | Dica |
|---|---|---|
| **Papel** | Persona ou especialidade que a IA deve assumir | Quanto mais específico, melhor: "engenheiro de testes sênior com experiência em Selenium" > "testador" |
| **Tarefa** | O que deve ser feito, de forma clara e delimitada | Inclua escopo e restrições relevantes |
| **Formato** | Estrutura esperada da resposta | Seja explícito: "lista numerada com no máximo 10 itens", "tabela com colunas X, Y, Z" |

## Exemplo prático

**Prompt:**
> Como especialista em automação de testes de software, seu objetivo é explicar os principais benefícios e desafios de implementar testes automatizados em um projeto de desenvolvimento ágil, no formato de uma lista com tópicos numerados.

**Por que funciona:**
- O papel "especialista em automação de testes de software" ativa vocabulário e profundidade técnica adequados.
- A tarefa delimita o tema (benefícios + desafios) e o contexto (desenvolvimento ágil).
- O formato (lista numerada) garante uma saída organizada e fácil de consumir.

## Fonte
Técnica derivada de frameworks de Role-Task-Format amplamente utilizados em engenharia de prompt. Referência: guias práticos de prompt engineering da comunidade de IA generativa.
