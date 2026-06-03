# Directional Stimulus Prompting (DSP)

## O que é
Técnica que utiliza dicas ou estímulos direcionais para guiar a geração do LLM
na direção desejada. Uma política treinável (modelo menor ou heurística) gera
"dicas" — como palavras-chave, frases-guia ou restrições — que são incluídas
no prompt para orientar um LLM maior (congelado, sem fine-tuning) a produzir
saídas melhores. Na prática, é como dar "pistas" ao modelo sobre o que incluir
ou como abordar a resposta.

## Quando usar
- Quando o modelo gera respostas genéricas e você quer direcionar o conteúdo
- Sumarização onde certos aspectos devem ser destacados
- Geração de texto que precisa incluir termos ou conceitos específicos
- Quando quer guiar o estilo ou foco da resposta sem reescrever tudo
- Tarefas de diálogo onde certos tópicos devem ser abordados

## Quando NÃO usar
- Quando a instrução direta já é suficiente para direcionar o modelo
- Tarefas de raciocínio puro (use CoT em vez disso)
- Quando as dicas podem limitar a criatividade necessária
- Se não é possível determinar boas dicas antecipadamente

## Template / Estrutura

### Template com palavras-chave guia

```
{{ instrução da tarefa }}

Dicas: inclua os seguintes elementos na resposta: {{ keyword_1 }}, {{ keyword_2 }}, {{ keyword_3 }}

{{ entrada / texto }}
```

### Template com estímulo direcional

```
{{ instrução da tarefa }}

Direcionamento: {{ aspecto ou perspectiva a enfatizar }}

{{ entrada }}

Responda focando no direcionamento acima.
```

### Template com restrições

```
{{ instrução da tarefa }}

Restrições:
- Deve mencionar: {{ elemento_obrigatório }}
- Deve abordar: {{ perspectiva_desejada }}
- Tom: {{ tom_desejado }}

{{ entrada }}
```

## Exemplo prático

### Exemplo 1 — Sumarização direcionada

**Sem estímulo direcional:**
```
Resuma o artigo abaixo.

[artigo sobre mudanças na legislação trabalhista]
```
*Resultado: resumo genérico cobrindo tudo superficialmente*

**Com estímulo direcional:**
```
Resuma o artigo abaixo focando no impacto para pequenas empresas.

Dicas: inclua informações sobre custos, prazos de adaptação e penalidades.

[artigo sobre mudanças na legislação trabalhista]
```
*Resultado: resumo focado, mencionando os elementos indicados*

### Exemplo 2 — Geração de e-mail com direcionamento

**Prompt:**
```
Escreva um e-mail para o cliente informando sobre o atraso na entrega.

Direcionamento:
- Tom: empático e profissional
- Deve mencionar: nova data prevista, compensação oferecida
- Deve evitar: culpar terceiros, linguagem excessivamente formal

Contexto: Pedido #4521, atraso de 3 dias, cliente é recorrente.
```

**Resposta esperada:**
```
Assunto: Atualização sobre seu pedido #4521

Olá [Nome],

Quero pessoalmente informar que seu pedido #4521 sofreu um atraso
e a nova data prevista de entrega é [data + 3 dias].

Sabemos o quanto isso é frustrante, especialmente para um cliente
fiel como você. Como forma de compensação, estamos aplicando 15%
de desconto no seu próximo pedido.

Acompanhe o status em tempo real pelo link: [link]

Qualquer dúvida, estou à disposição.
```

## Relação com outras técnicas
- **Few-Shot**: exemplos mostram o padrão; DSP direciona o conteúdo
- **Prompt Chaining**: pode usar DSP em um dos elos da cadeia para refinar a saída
- **Zero-Shot**: DSP adiciona dicas a uma instrução que de outra forma seria zero-shot

## Dicas
- As dicas funcionam como "âncoras" que o modelo tende a incluir na resposta
- Menos é mais: 3-5 dicas direcionais funcionam melhor que uma lista longa
- Combine com instrução de formato para melhores resultados
- Teste quais dicas realmente melhoram a saída vs. adicionam ruído

## Fonte
https://www.promptingguide.ai/pt/techniques/dsp
