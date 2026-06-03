# CARE (Contexto, Ação, Resultado e Exemplo)

## O que é
Framework rico e completo que estrutura o prompt com **Contexto** situacional, a **Ação** esperada, o **Resultado** desejado e um **Exemplo** ilustrativo. A inclusão explícita de exemplo funciona como few-shot embutido, calibrando tom, profundidade e estilo da resposta. É um dos frameworks mais robustos para tarefas que exigem nuance.

## Quando usar
- Tarefas que exigem contexto situacional rico para gerar respostas relevantes.
- Quando fornecer um exemplo concreto melhora significativamente a qualidade do output.
- Cenários de persuasão, argumentação ou comunicação onde tom e abordagem importam.
- Geração de conteúdo técnico-educacional com estudos de caso.
- Quando a IA precisa entender "a cena completa" antes de agir.

## Quando NÃO usar
- Tarefas simples de classificação, extração ou transformação de dados.
- Cenários onde não há exemplo relevante disponível (prefira PTF ou TAO).
- Instruções puramente procedurais com passos fixos (prefira PEPE).
- Quando brevidade é prioridade — CARE tende a gerar prompts mais longos.

## Template / Estrutura

```
{{ contexto }}
{{ ação }}
O objetivo é {{ resultado }}
{{ exemplo }}
```

| Componente | Descrição | Dica |
|---|---|---|
| **Contexto** | Cenário, audiência, situação atual — a "cena" completa | Inclua quem é o público, onde isso acontece, premissas relevantes |
| **Ação** | O que a IA deve fazer com esse contexto | Detalhe o tipo de output: "elabore uma explicação", "crie um argumento" |
| **Resultado** | Impacto ou consequência esperada da ação | Foque no efeito desejado: "convencer o time", "reduzir dúvidas" |
| **Exemplo** | Caso real ou hipotético que ancora a resposta | Use "Use o caso de...", "Como no cenário onde...", "Exemplo: quando..." |

## Exemplo prático

**Prompt:**
> Você está explicando para um time de desenvolvedores a importância de testes automatizados no fluxo de CI/CD. Elabore uma explicação detalhada, destacando os benefícios e os impactos na qualidade do software. O objetivo é convencer o time a adotar testes automatizados em suas pipelines. Use o caso de um projeto que reduziu falhas na produção após implementar automação de testes.

**Por que funciona:**
- O contexto situa a audiência (devs) e o cenário (CI/CD), permitindo vocabulário adequado.
- A ação é específica: "explicação detalhada com benefícios e impactos".
- O resultado define o propósito persuasivo: "convencer o time a adotar".
- O exemplo ancora a resposta em um caso real, tornando o argumento tangível.

## Fonte
Técnica derivada do framework CARE (Context, Action, Result, Example) utilizado em engenharia de prompt avançada. Referência: guias práticos de prompt engineering da comunidade de IA generativa.
