# Retrieval Augmented Generation (RAG)

## O que é
Abordagem que combina a recuperação de informações de fontes externas (documentos,
bases de dados, APIs) com a capacidade de geração do LLM. Em vez de depender
exclusivamente do conhecimento pré-treinado do modelo, o RAG busca documentos
relevantes e os injeta como contexto antes da geração. Isso melhora a factualidade,
reduz alucinações e permite que o modelo trabalhe com informações atualizadas
ou específicas de um domínio.

## Quando usar
- Perguntas sobre dados atualizados que o modelo não tem no treinamento
- QA sobre documentação interna da empresa
- Quando a factualidade é crítica (jurídico, médico, financeiro)
- Chatbots de suporte que precisam consultar base de conhecimento
- Tarefas intensivas em conhecimento de domínio específico
- Quando é necessário citar fontes das respostas

## Quando NÃO usar
- Tarefas puramente criativas ou de geração livre
- Quando os documentos disponíveis são de baixa qualidade ou irrelevantes
- Problemas de raciocínio puro (matemática, lógica) sem necessidade de fatos
- Quando o custo de infraestrutura de retrieval não se justifica
- Conversas casuais ou small talk

## Template / Estrutura

### Prompt com contexto recuperado

```
Use APENAS as informações do contexto abaixo para responder à pergunta.
Se a resposta não estiver no contexto, diga "Não encontrei essa informação
nos documentos disponíveis."

Contexto:
---
{{ documento_1_recuperado }}
---
{{ documento_2_recuperado }}
---
{{ documento_N_recuperado }}
---

Pergunta: {{ pergunta_do_usuário }}

Resposta:
```

### Prompt com citação de fontes

```
Com base nos documentos abaixo, responda à pergunta citando a fonte de cada
afirmação entre colchetes [Fonte X].

Documentos:
[Fonte 1] {{ trecho_doc_1 }}
[Fonte 2] {{ trecho_doc_2 }}
[Fonte 3] {{ trecho_doc_3 }}

Pergunta: {{ pergunta }}

Resposta (com citações):
```

## Exemplo prático

### Exemplo — QA sobre política interna

**Prompt:**
```
Use APENAS as informações do contexto abaixo para responder.

Contexto:
---
Política de Férias v3.2 (Jan/2025): Colaboradores com mais de 12 meses podem
solicitar férias com 30 dias de antecedência via portal RH. O período mínimo
é de 5 dias úteis. Férias podem ser fracionadas em até 3 períodos, sendo que
nenhum pode ser inferior a 5 dias úteis.
---
Política de Home Office v2.1 (Mar/2025): Colaboradores elegíveis podem
trabalhar remotamente até 3 dias por semana. A solicitação deve ser feita
via portal RH com 48h de antecedência.
---

Pergunta: Posso tirar férias de 3 dias úteis?

Resposta:
```

**Resposta esperada:**
```
Não. De acordo com a Política de Férias v3.2, o período mínimo de férias é
de 5 dias úteis. Além disso, mesmo no fracionamento (permitido em até 3
períodos), nenhum período pode ser inferior a 5 dias úteis.
```

## Componentes de um sistema RAG
1. **Indexação**: documentos são divididos em chunks e convertidos em embeddings
2. **Retrieval**: a pergunta do usuário é convertida em embedding e busca os chunks mais similares
3. **Augmentation**: chunks recuperados são inseridos no prompt como contexto
4. **Generation**: o LLM gera a resposta com base no contexto fornecido

## Dicas
- A qualidade do RAG depende mais do retrieval do que do prompt
- Chunks menores e mais focados geralmente funcionam melhor que documentos inteiros
- Instrua o modelo a dizer "não sei" quando o contexto não contém a resposta
- Inclua metadados dos documentos (data, versão) para o modelo avaliar relevância
- Teste com perguntas cujas respostas você conhece para calibrar o pipeline

## Fonte
https://www.promptingguide.ai/pt/techniques/rag
