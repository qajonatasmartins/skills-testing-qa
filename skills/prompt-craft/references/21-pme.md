# PME (Problema, Meta e Estratégia)

## O que é
Framework orientado a resolução de problemas que estrutura o prompt partindo de uma **dor ou problema** real, define a **Meta** desejada e especifica a **Estratégia** para alcançá-la. Ao começar pelo problema, força a IA a contextualizar a resposta na realidade do usuário, gerando soluções mais relevantes e aplicáveis.

## Quando usar
- Cenários de problem-solving onde existe uma dor ou limitação clara a ser resolvida.
- Consultorias, diagnósticos e recomendações baseadas em situação real.
- Quando o ponto de partida é "algo não funciona" ou "preciso melhorar X".
- Tarefas de planejamento estratégico, priorização ou tomada de decisão.

## Quando NÃO usar
- Geração criativa ou aberta sem um problema definido (prefira TAO ou PTF).
- Tarefas que dependem de exemplos concretos para calibrar a saída (prefira CARE).
- Instruções puramente procedurais com passos já conhecidos (prefira PEPE).
- Quando o problema ainda não está claro — primeiro investigue, depois use PME.

## Template / Estrutura

```
{{ problema }}
{{ meta }}
{{ estratégia }}
```

| Componente | Descrição | Dica |
|---|---|---|
| **Problema** | Dor, dificuldade ou limitação que motiva o pedido | Seja específico: "profissionais de QA têm dificuldade em X" > "testes são difíceis" |
| **Meta** | O que se deseja alcançar ao resolver o problema | Defina de forma mensurável quando possível |
| **Estratégia** | Como a resposta deve ser estruturada para atingir a meta | Indique formato, abordagem ou sequência esperada |

## Exemplo prático

**Prompt:**
> Muitos profissionais de QA têm dificuldade em entender como priorizar casos de teste em projetos grandes. Ensine uma abordagem eficaz para priorização de testes com base em risco e impacto. Apresente uma explicação teórica seguida de um exemplo prático aplicável ao dia a dia.

**Por que funciona:**
- O problema é concreto e identificável: dificuldade de priorização em projetos grandes.
- A meta é clara: ensinar priorização baseada em risco e impacto.
- A estratégia guia a estrutura: teoria primeiro, depois exemplo prático, garantindo profundidade e aplicabilidade.

## Fonte
Técnica derivada de frameworks de Problem-Goal-Strategy utilizados em consultoria e engenharia de prompt. Referência: guias práticos de prompt engineering da comunidade de IA generativa.
