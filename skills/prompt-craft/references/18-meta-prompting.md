# Meta-Prompting

## O que é
Técnica onde um "meta-prompt" age como orquestrador, coordenando múltiplos
sub-prompts especializados para resolver tarefas complexas. O meta-prompt
define a estratégia geral, delega partes específicas a "especialistas"
(personas ou perspectivas diferentes) e integra os resultados em uma
resposta coesa. É essencialmente um prompt que gera e gerencia outros prompts,
combinando as vantagens de diferentes perspectivas ou expertise em um único fluxo.

## Quando usar
- Tarefas que se beneficiam de múltiplas perspectivas (revisão, análise)
- Problemas interdisciplinares que exigem expertise combinada
- Quando um único prompt não captura toda a complexidade necessária
- Planejamento de projetos com múltiplas dimensões
- Revisão de código, arquitetura ou documentação sob diferentes ângulos
- Tomada de decisão com trade-offs entre áreas diferentes

## Quando NÃO usar
- Tarefas simples e unidimensionais
- Quando uma única perspectiva já é suficiente
- Se o overhead de múltiplos sub-prompts não se justifica pelo ganho de qualidade
- Tarefas com resposta objetiva e factual
- Quando velocidade é mais importante que profundidade

## Template / Estrutura

### Template de orquestração

```
Você é um orquestrador que coordena múltiplos especialistas para resolver
a tarefa abaixo. Para cada etapa:
1. Identifique qual especialista é necessário
2. Formule a pergunta para esse especialista
3. Integre a resposta no contexto geral

Tarefa: {{ descrição_da_tarefa }}

Especialistas disponíveis:
- {{ especialista_1 }}: {{ área_de_expertise }}
- {{ especialista_2 }}: {{ área_de_expertise }}
- {{ especialista_3 }}: {{ área_de_expertise }}

Comece identificando quais especialistas consultar e em qual ordem.
```

### Template de mesa redonda

```
Simule uma mesa redonda com os seguintes especialistas discutindo o tema:

- {{ papel_1 }}: foco em {{ perspectiva_1 }}
- {{ papel_2 }}: foco em {{ perspectiva_2 }}
- {{ papel_3 }}: foco em {{ perspectiva_3 }}

Tema: {{ tema_ou_problema }}

Cada especialista deve:
1. Apresentar sua análise sob sua perspectiva
2. Apontar riscos ou preocupações que os outros podem ter ignorado
3. Sugerir ações específicas

Ao final, sintetize as contribuições em um plano de ação consolidado.
```

### Template de prompt que gera prompts

```
Meu objetivo é: {{ objetivo }}

Gere 3 prompts especializados, cada um otimizado para um aspecto diferente:
1. Prompt para análise de {{ aspecto_1 }}
2. Prompt para análise de {{ aspecto_2 }}
3. Prompt para análise de {{ aspecto_3 }}

Para cada prompt gerado, indique o que ele deve produzir e como
os resultados devem ser combinados.
```

## Exemplo prático

### Exemplo 1 — Revisão de feature por múltiplas perspectivas

**Prompt:**
```
Simule uma mesa redonda analisando a feature abaixo:

- Product Manager: foco em valor de negócio e priorização
- Tech Lead: foco em viabilidade técnica e arquitetura
- QA Engineer: foco em testabilidade e riscos de qualidade
- UX Designer: foco em usabilidade e experiência do usuário

Feature: "Implementar Pix por aproximação (NFC) no app de pagamentos"

Cada especialista deve apresentar sua análise, riscos e recomendações.
Ao final, consolidem em uma decisão com condições.
```

**Resposta esperada:**
```
Product Manager:
- Valor alto: diferencial competitivo, conveniência para o usuário
- Risco: adoção pode ser baixa em dispositivos sem NFC
- Recomendação: lançar como beta para dispositivos compatíveis

Tech Lead:
- Requer integração com SDK NFC nativo (Android/iOS diferem bastante)
- Segurança: tokenização do Pix precisa de certificação
- Estimativa: 3-4 sprints com incerteza alta na parte de certificação
- Recomendação: PoC de 1 sprint antes de comprometer roadmap

QA Engineer:
- Testabilidade difícil: depende de hardware NFC físico
- Cenários críticos: timeout NFC, duplo pagamento, NFC desligado
- Recomendação: investir em ambiente de teste com emuladores NFC

UX Designer:
- Fluxo deve ser intuitivo: aproximar → confirmar → pago
- Preocupação: feedback visual/sonoro de sucesso precisa ser imediato
- Acessibilidade: usuários com deficiência visual precisam de feedback háptico
- Recomendação: protótipo de alta fidelidade antes do desenvolvimento

Consolidação:
DECISÃO: Prosseguir com PoC de 1 sprint focado em viabilidade técnica.
CONDIÇÕES: (1) PoC bem-sucedida, (2) protótipo UX validado com 5 usuários,
(3) plano de teste com emuladores NFC aprovado pelo QA.
```

### Exemplo 2 — Meta-prompt para gerar prompts de teste

**Prompt:**
```
Meu objetivo é testar uma API de transferência bancária.

Gere 3 prompts especializados:
1. Prompt para gerar casos de teste funcionais
2. Prompt para gerar cenários de segurança
3. Prompt para gerar cenários de performance/carga

Para cada prompt, indique como combinar os resultados em uma estratégia única.
```

## Relação com outras técnicas
- **Tree of Thoughts**: ToT explora caminhos; Meta-Prompting combina perspectivas
- **Prompt Chaining**: encadeia prompts sequenciais; Meta-Prompting orquestra paralelos
- **ReAct**: executa ações; Meta-Prompting delega a especialistas

## Dicas
- Defina claramente o papel e escopo de cada especialista
- O orquestrador deve ter uma visão clara do objetivo final
- Peça explicitamente que cada especialista critique as perspectivas dos outros
- A síntese final é tão importante quanto as análises individuais
- Funciona especialmente bem para decisões de arquitetura e planejamento

## Fonte
https://www.promptingguide.ai/pt/techniques
