# PEPE (Papel, Entrada, Passo e Expectativa)

## O que é
Framework estruturado que combina a atribuição de um **Papel** especialista com uma **Entrada** clara (dados ou artefatos a analisar), **Passos** explícitos a seguir e a **Expectativa** do resultado final. É particularmente eficaz para tarefas analíticas e procedurais onde a sequência de ações importa tanto quanto o resultado.

## Quando usar
- Tarefas de análise que recebem artefatos como entrada (requisitos, código, logs, documentos).
- Cenários onde a ordem dos passos influencia a qualidade do resultado.
- Processos que precisam ser reproduzíveis e auditáveis (QA, revisão de código, compliance).
- Quando a expectativa precisa ser declarada explicitamente para evitar respostas genéricas.
- Instruções para agentes ou pipelines automatizadas onde previsibilidade é crítica.

## Quando NÃO usar
- Tarefas criativas ou abertas sem passos predefinidos (prefira TAO ou CARE).
- Cenários simples onde papel + tarefa + formato já bastam (prefira PTF).
- Quando não há entrada/artefato específico para processar.
- Brainstorming ou exploração de ideias onde rigidez processual limita a criatividade.

## Template / Estrutura

```
Como um especialista em {{ papel }}
Você deve {{ entrada }}
Seguindo os passos {{ passos }}
Pois esperamos que {{ expectativa }}
```

| Componente | Descrição | Dica |
|---|---|---|
| **Papel** | Especialidade ou persona da IA | Combine domínio + senioridade: "arquiteto de testes sênior", "analista de segurança" |
| **Entrada** | Dados, artefatos ou informações a processar | Especifique o tipo: "analisar os requisitos", "revisar o código do módulo X" |
| **Passo** | Sequência de ações a executar | Numere os passos ou use ordem lógica clara |
| **Expectativa** | Resultado esperado e critério de sucesso | Defina o "definition of done" da tarefa |

## Exemplo prático

**Prompt:**
> Como um especialista em qualidade de software, você deve analisar os requisitos funcionais e não funcionais de um novo projeto de software. Seguindo os passos de elaboração de casos de teste e execução de testes funcionais. Pois esperamos que a análise identificada garanta a qualidade do software desde o início do ciclo de vida.

**Por que funciona:**
- O papel "especialista em qualidade de software" define a lente de análise.
- A entrada é clara: "requisitos funcionais e não funcionais de um novo projeto".
- Os passos definem a sequência: elaborar casos de teste → executar testes funcionais.
- A expectativa ancora o resultado: garantir qualidade desde o início do ciclo.

## Fonte
Técnica derivada de frameworks de Role-Input-Steps-Expectation utilizados em engenharia de prompt para tarefas analíticas e procedurais. Referência: guias práticos de prompt engineering da comunidade de IA generativa.
