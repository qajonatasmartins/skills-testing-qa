# Few-Shot Prompting

## O que é
Técnica onde se fornecem exemplos demonstrativos (pares de entrada→saída) dentro do prompt
para que o modelo aprenda o padrão desejado antes de processar a nova entrada.
Permite condicionar o comportamento do modelo sem fine-tuning, usando apenas o contexto do prompt.
Variações incluem one-shot (1 exemplo) e few-shot (2+ exemplos).
A qualidade e diversidade dos exemplos impactam diretamente o resultado.

## Quando usar
- Zero-shot não produz resultados satisfatórios
- A tarefa exige formato de saída específico ou incomum
- Classificação com categorias que o modelo pode confundir
- Tarefas de transformação de dados com padrão bem definido
- Quando é mais fácil mostrar o que se quer do que explicar

## Quando NÃO usar
- Tarefas de raciocínio matemático/lógico complexo (few-shot sozinho não resolve — use CoT)
- Quando a janela de contexto é limitada e os exemplos consomem tokens demais
- Se zero-shot já funciona bem (não adicione complexidade desnecessária)
- Quando os exemplos podem introduzir viés indesejado
- Tarefas onde a distribuição dos rótulos nos exemplos pode influenciar o resultado

## Template / Estrutura

```
{{ instrução da tarefa (opcional mas recomendado) }}

{{ entrada exemplo 1 }}
{{ saída exemplo 1 }}

{{ entrada exemplo 2 }}
{{ saída exemplo 2 }}

{{ entrada exemplo N }}
{{ saída exemplo N }}

{{ nova entrada }}
```

### Variação estruturada

```
Tarefa: {{ descrição da tarefa }}

Exemplos:
---
Entrada: "{{ exemplo_input_1 }}"
Saída: {{ exemplo_output_1 }}
---
Entrada: "{{ exemplo_input_2 }}"
Saída: {{ exemplo_output_2 }}
---
Entrada: "{{ novo_input }}"
Saída:
```

## Exemplo prático

### Exemplo 1 — Classificação de intenção

**Prompt:**
```
Classifique a intenção da mensagem do cliente.

Mensagem: "Quero cancelar minha assinatura"
Intenção: cancelamento

Mensagem: "Quanto custa o plano premium?"
Intenção: consulta_preço

Mensagem: "Meu boleto não chegou no e-mail"
Intenção: suporte_pagamento

Mensagem: "Vocês têm plano para empresas?"
Intenção:
```

**Resposta esperada:**
```
consulta_preço
```

### Exemplo 2 — Transformação de formato

**Prompt:**
```
Converta a descrição do bug para o formato estruturado.

Bug: "O app trava quando clico em salvar no formulário de cadastro no Android"
Formato:
- Ação: Clicar em salvar
- Tela: Formulário de cadastro
- Plataforma: Android
- Resultado: App trava

Bug: "Login por biometria não funciona depois de atualizar o iOS"
Formato:
- Ação: Login por biometria
- Tela: Login
- Plataforma: iOS
- Resultado: Não funciona após atualização

Bug: "A busca de produtos retorna lista vazia quando uso acentos no navegador Chrome"
Formato:
```

**Resposta esperada:**
```
- Ação: Busca de produtos com acentos
- Tela: Busca
- Plataforma: Chrome (Web)
- Resultado: Lista vazia
```

## Dicas de seleção de exemplos
- Use exemplos **diversos** que cubram os casos mais representativos
- Mantenha os exemplos **balanceados** entre categorias
- A **ordem** dos exemplos pode influenciar — coloque os mais relevantes por último
- Prefira exemplos **reais** do domínio em vez de genéricos
- Comece com 2-3 exemplos e aumente se necessário

## Limitações conhecidas
- Não resolve tarefas de raciocínio complexo sozinho (combine com CoT)
- A distribuição de rótulos nos exemplos pode enviesar o modelo
- O formato dos exemplos é mais determinante que os rótulos em si

## Fonte
https://www.promptingguide.ai/pt/techniques/fewshot
