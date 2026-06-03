# Catálogo de Técnicas de Prompting

Referência central para a skill `prompt-craft`. Contém o índice das 23 técnicas, a matriz de decisão e as regras de combinação.

## Tabela Resumo

| # | Nome | Sigla | Quando Usar | Complexidade |
|---|------|-------|-------------|-------------|
| 01 | Zero-Shot Prompting | — | Tarefa simples e direta, sem necessidade de exemplos | Baixa |
| 02 | Few-Shot Prompting | — | Tarefa que se beneficia de exemplos de entrada/saída para calibrar formato e estilo | Baixa |
| 03 | Chain-of-Thought | CoT | Raciocínio multi-etapa, problemas de lógica, matemática ou análise sequencial | Média |
| 04 | Self-Consistency | — | Mesmo problema com múltiplos caminhos válidos; precisa da resposta mais robusta | Média |
| 05 | Generate Knowledge Prompting | — | Tarefa que exige gerar conhecimento/fatos relevantes antes de responder | Média |
| 06 | Prompt Chaining | — | Tarefa complexa que pode ser decomposta em subtarefas sequenciais | Média |
| 07 | Tree of Thoughts | ToT | Exploração de alternativas divergentes antes de convergir numa decisão | Alta |
| 08 | Retrieval Augmented Generation | RAG | Tarefa que depende de conhecimento externo, atualizado ou factual específico | Alta |
| 09 | Automatic Reasoning and Tool-use | ART | Raciocínio automático que intercala com uso de ferramentas externas | Alta |
| 10 | Automatic Prompt Engineer | APE | Otimização automática do próprio prompt via geração e avaliação de candidatos | Alta |
| 11 | Active-Prompt | — | Seleção adaptativa de exemplos CoT com base na incerteza do modelo | Alta |
| 12 | Directional Stimulus Prompting | — | Guiar a geração com dicas, palavras-chave ou estímulos direcionais | Média |
| 13 | Program-Aided Language Models | PAL | Usar código (Python, etc.) como ferramenta de raciocínio em vez de linguagem natural | Média |
| 14 | ReAct | — | Tarefas que intercalam raciocínio e ação (busca, cálculo, consulta a APIs) | Alta |
| 15 | Reflexion | — | Auto-reflexão sobre tentativas anteriores para melhoria iterativa | Alta |
| 16 | Multimodal Chain-of-Thought | — | Entrada com imagens + texto; raciocínio que exige análise visual | Alta |
| 17 | Graph Prompting | — | Dados estruturados em grafo, redes de relacionamento, ontologias | Alta |
| 18 | Meta-Prompting | — | Orquestração de múltiplos sub-prompts especializados por um prompt coordenador | Alta |
| 19 | PTF | PTF | Papel definido + tarefa clara + formato de saída esperado | Baixa |
| 20 | TAO | TAO | Tarefa orientada a ação com objetivo mensurável | Baixa |
| 21 | PME | PME | Problema identificado + meta de resolução + estratégia proposta | Baixa |
| 22 | CARE | CARE | Contexto rico + ação desejada + resultado esperado + exemplo ilustrativo | Média |
| 23 | PEPE | PEPE | Papel + entrada estruturada + passos sequenciais + expectativa de saída | Média |

## Matriz de Decisão

Use esta matriz para selecionar a técnica. Avalie as características da tarefa (coluna esquerda) com base nas 7 dimensões coletadas na entrevista.

### Por tipo de tarefa

| Característica da tarefa | Técnica recomendada | Arquivo |
|--------------------------|---------------------|---------|
| Tarefa simples e direta, sem exemplos necessários | **Zero-Shot** | `01-zero-shot.md` |
| Precisa de exemplos para calibrar formato/estilo | **Few-Shot** | `02-few-shot.md` |
| Raciocínio multi-etapa, lógica ou matemática | **Chain-of-Thought (CoT)** | `03-chain-of-thought.md` |
| Múltiplos caminhos válidos, precisa de robustez | **Self-Consistency** | `04-self-consistency.md` |
| Exige gerar conhecimento antes de responder | **Generate Knowledge** | `05-generate-knowledge.md` |
| Tarefa exige informação externa/factual atualizada | **RAG** | `08-rag.md` |
| Decomposição em subtarefas encadeadas | **Prompt Chaining** | `06-prompt-chaining.md` |
| Explorar alternativas divergentes antes de decidir | **Tree of Thoughts (ToT)** | `07-tree-of-thoughts.md` |
| Ação + raciocínio intercalados com ferramentas | **ReAct** | `14-react.md` |
| Código como ferramenta de raciocínio | **PAL** | `13-pal.md` |
| Auto-reflexão e melhoria iterativa | **Reflexion** | `15-reflexion.md` |
| Entrada multimodal (imagem + texto) | **Multimodal CoT** | `16-multimodal-cot.md` |
| Dados em grafo ou rede de relacionamentos | **Graph Prompting** | `17-graph-prompting.md` |
| Orquestração de múltiplos sub-prompts | **Meta-Prompting** | `18-meta-prompting.md` |
| Guiar geração com dicas/estímulos direcionais | **Directional Stimulus** | `12-directional-stimulus.md` |
| Raciocínio automático com ferramentas | **ART** | `09-art.md` |
| Otimizar o próprio prompt automaticamente | **APE** | `10-ape.md` |
| Adaptar exemplos CoT por incerteza do modelo | **Active-Prompt** | `11-active-prompt.md` |

### Por framework estruturado

Quando o usuário quer uma estrutura fixa e previsível, use os frameworks customizados:

| Cenário | Técnica recomendada | Arquivo |
|---------|---------------------|---------|
| Papel claro + tarefa definida + formato esperado | **PTF** | `19-ptf.md` |
| Tarefa orientada a ação com objetivo mensurável | **TAO** | `20-tao.md` |
| Problema com meta e estratégia de resolução | **PME** | `21-pme.md` |
| Contexto rico + exemplo ilustrativo | **CARE** | `22-care.md` |
| Papel + entrada + passos sequenciais + expectativa | **PEPE** | `23-pepe.md` |

### Fluxo de decisão simplificado

```
O usuário forneceu exemplos?
├─ Sim → Few-Shot (ou Few-Shot + CoT se raciocínio complexo)
└─ Não
    ├─ Tarefa simples, resposta direta?
    │   ├─ Sim → Zero-Shot
    │   └─ Não
    │       ├─ Precisa de raciocínio passo-a-passo?
    │       │   ├─ Sim → CoT
    │       │   └─ Não
    │       │       ├─ Precisa decompor em subtarefas?
    │       │       │   ├─ Sim → Prompt Chaining
    │       │       │   └─ Não
    │       │       │       ├─ Precisa explorar alternativas?
    │       │       │       │   ├─ Sim → ToT
    │       │       │       │   └─ Não → ver frameworks abaixo
    │       │       │       └─
    │       │       └─
    │       └─
    └─

O usuário quer estrutura fixa (papel/tarefa/formato)?
├─ Papel + Tarefa + Formato → PTF
├─ Tarefa + Ação + Objetivo → TAO
├─ Problema + Meta + Estratégia → PME
├─ Contexto + Ação + Resultado + Exemplo → CARE
└─ Papel + Entrada + Passos + Expectativa → PEPE
```

## Regras de Combinação

Algumas tarefas se beneficiam de combinar técnicas. A primeira técnica define a **estrutura**, a segunda define o **mecanismo de raciocínio**.

### Combinações recomendadas

| Combinação | Quando usar | Como aplicar |
|------------|-------------|--------------|
| **Few-Shot + CoT** | Tarefa com raciocínio complexo onde exemplos ajudam a calibrar o formato da explicação | Forneça exemplos que incluem o raciocínio passo-a-passo na resposta, não só a resposta final |
| **PTF + CoT** | Papel definido + tarefa que exige raciocínio estruturado | Defina o papel e formato via PTF, e instrua a IA a pensar passo-a-passo |
| **CARE + Few-Shot** | Contexto rico onde exemplos ajudam a calibrar o resultado esperado | Use a estrutura CARE e inclua 1-2 exemplos completos no bloco de Exemplo |
| **PEPE + CoT** | Entrada estruturada + passos que exigem raciocínio explícito | Defina papel/entrada/expectativa via PEPE, instrua passos com raciocínio encadeado |
| **Few-Shot + Self-Consistency** | Precisa de exemplos + robustez na resposta | Forneça exemplos e peça múltiplas resoluções; selecione a mais consistente |
| **CoT + Self-Consistency** | Raciocínio complexo que pode seguir caminhos diferentes | Peça para raciocinar passo-a-passo por múltiplos caminhos e verificar convergência |
| **Prompt Chaining + CoT** | Subtarefas encadeadas onde cada etapa exige raciocínio | Decomponha em etapas; cada prompt intermediário usa CoT |
| **PTF + Few-Shot** | Papel + tarefa + formato, com exemplos para calibrar | Defina PTF e inclua 2-3 exemplos que seguem exatamente o formato esperado |
| **RAG + CoT** | Conhecimento externo + raciocínio sobre os dados recuperados | Recupere informações e instrua a IA a raciocinar sobre elas passo-a-passo |
| **PME + CoT** | Problema com meta onde a estratégia exige análise em etapas | Use PME para enquadrar o problema e instrua raciocínio encadeado na estratégia |

### Regras gerais de combinação

1. **Máximo de 2 técnicas** por prompt — mais que isso adiciona complexidade sem ganho proporcional.
2. **Framework + Mecanismo** é a combinação mais natural (ex: PTF + CoT, CARE + Few-Shot).
3. **Evite combinar frameworks entre si** (ex: PTF + CARE) — eles competem pela estrutura do prompt.
4. **Self-Consistency é sempre a segunda** — ela opera sobre a saída, não sobre a estrutura.
5. **CoT combina com quase tudo** — é o mecanismo de raciocínio mais versátil.

### Quando NÃO combinar

- Tarefa simples → Zero-Shot puro basta.
- Usuário quer resposta rápida → combinações adicionam verbosidade.
- Prompt já tem mais de 500 tokens → complexidade demais, simplifique.

## Mapeamento: Dimensão → Técnica

Referência cruzada para a Fase 3 da skill — qual dimensão mais influencia a escolha:

| Dimensão dominante | Técnica primária |
|--------------------|-----------------|
| **Objetivo** simples e direto | Zero-Shot |
| **Objetivo** com raciocínio | CoT |
| **Persona/Papel** definido | PTF ou PEPE |
| **Contexto** rico e específico | CARE |
| **Formato** muito estruturado | PTF (+ Few-Shot se precisa de exemplos) |
| **Complexidade** alta, multi-etapa | CoT, Prompt Chaining ou ToT |
| **Exemplos** fornecidos pelo usuário | Few-Shot |
| **Restrições** fortes (tom, tamanho) | Directional Stimulus ou PTF com restrições explícitas |
