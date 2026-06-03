# ReAct (Reasoning + Acting)

## O que é
Framework que intercala etapas de raciocínio (Thought) com ações concretas (Action)
e observação dos resultados (Observation). O modelo pensa sobre o que precisa fazer,
executa uma ação (como buscar informação ou consultar uma ferramenta), observa o
resultado e então raciocina novamente com base na nova informação. Esse ciclo
Thought → Action → Observation se repete até que o modelo tenha informação
suficiente para responder. Reduz alucinações ao ancorar o raciocínio em dados reais.

## Quando usar
- Tarefas que precisam de informação externa para serem respondidas
- QA sobre fatos que o modelo pode não saber ou pode alucinar
- Navegação em bases de conhecimento ou APIs
- Tarefas de múltiplos passos com dependência de informação intermediária
- Agentes que interagem com ferramentas e ambientes
- Quando a transparência do raciocínio + ação é importante

## Quando NÃO usar
- Tarefas puramente baseadas em conhecimento do modelo (sem necessidade de busca)
- Quando não há ferramentas/ações disponíveis para o modelo usar
- Tarefas simples de geração de texto
- Quando a latência de múltiplas ações é inaceitável
- Conversas casuais sem necessidade de factualidade

## Template / Estrutura

```
Responda à pergunta usando o ciclo Pensamento/Ação/Observação.

Ferramentas disponíveis:
- Buscar[termo]: busca informações sobre o termo
- Calcular[expressão]: calcula uma expressão matemática
- {{ outras_ferramentas }}

Pergunta: {{ pergunta }}

Pensamento 1: {{ raciocínio sobre o que precisa descobrir }}
Ação 1: {{ ferramenta }}[{{ parâmetro }}]
Observação 1: {{ resultado da ação }}

Pensamento 2: {{ raciocínio com base na observação }}
Ação 2: {{ ferramenta }}[{{ parâmetro }}]
Observação 2: {{ resultado }}

...

Pensamento N: {{ raciocínio final }}
Resposta: {{ resposta final }}
```

## Exemplo prático

### Exemplo 1 — QA com busca

**Prompt:**
```
Responda usando o ciclo Pensamento/Ação/Observação.

Ferramentas: Buscar[termo], Calcular[expressão]

Pergunta: O PIB do Brasil é maior que o da Argentina e do Chile juntos?
```

**Resposta esperada:**
```
Pensamento 1: Preciso descobrir o PIB do Brasil, Argentina e Chile para comparar.
Ação 1: Buscar[PIB Brasil 2024]
Observação 1: PIB do Brasil: ~US$ 2,2 trilhões.

Pensamento 2: Agora preciso do PIB da Argentina.
Ação 2: Buscar[PIB Argentina 2024]
Observação 2: PIB da Argentina: ~US$ 640 bilhões.

Pensamento 3: Falta o PIB do Chile.
Ação 3: Buscar[PIB Chile 2024]
Observação 3: PIB do Chile: ~US$ 340 bilhões.

Pensamento 4: Agora posso comparar. Argentina + Chile = ?
Ação 4: Calcular[640 + 340]
Observação 4: 980

Pensamento 5: Brasil (US$ 2,2 tri) vs Argentina + Chile (US$ 980 bi).
2.200 > 980, então sim, o PIB do Brasil é maior.

Resposta: Sim. O PIB do Brasil (~US$ 2,2 trilhões) é significativamente
maior que o PIB combinado da Argentina (~US$ 640 bi) e do Chile (~US$ 340 bi),
que somam aproximadamente US$ 980 bilhões.
```

### Exemplo 2 — Debugging com ReAct

**Prompt:**
```
Ferramentas: LerArquivo[path], ExecutarTeste[comando], BuscarLog[filtro]

Pergunta: O teste de login está falhando. Diagnostique o problema.

Pensamento 1: Preciso ver o log do teste para entender a falha.
Ação 1: BuscarLog[test:login:failure]
Observação 1: "Expected status 200, received 401. Token expired."

Pensamento 2: O token está expirando. Preciso verificar a configuração de token.
Ação 2: LerArquivo[config/auth.ts]
Observação 2: tokenExpiry: "1s" // deveria ser "1h"

Pensamento 3: Encontrei o problema. O tempo de expiração do token está
configurado como 1 segundo em vez de 1 hora.

Resposta: O teste falha porque o token de autenticação expira em 1 segundo
(config/auth.ts, linha tokenExpiry: "1s"). Corrija para "1h".
```

## Relação com outras técnicas
- **CoT**: ReAct adiciona ações ao raciocínio do CoT
- **ART**: ART automatiza a seleção de demonstrações ReAct
- **PAL**: PAL pode ser uma "ação" dentro do loop ReAct

## Dicas
- Defina claramente as ferramentas disponíveis e seu formato de uso
- O modelo deve raciocinar ANTES de agir (nunca agir sem pensar)
- Limite o número de ciclos para evitar loops infinitos (5-10 é razoável)
- ReAct é a base de frameworks de agentes como LangChain e AutoGPT

## Fonte
https://www.promptingguide.ai/pt/techniques/react
