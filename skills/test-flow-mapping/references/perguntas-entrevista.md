# Roteiro de Perguntas — Entrevista Guiada

Perguntas em blocos pelas 4 dimensões. Regras gerais:

- **Máx. 5 perguntas por mensagem**; aguardar resposta antes do próximo bloco.
- Começar sempre pela **pergunta-âncora** do bloco; aprofundar só no que a resposta abrir.
- Adaptar o vocabulário ao domínio que o usuário usar (linguagem ubíqua).
- Registrar como **pergunta em aberto** tudo que o usuário responder com "não sei" ou "preciso confirmar".

---

## Bloco 1 — Fluxos e decisões (esqueleto do fluxograma)

**Âncora:** *"Descreve o caminho feliz: o que o usuário (ou sistema) faz do início ao fim quando tudo dá certo?"*

Aprofundamento:
- Em quais pontos o usuário ou o sistema **decide** algo (validações, condições, escolhas)?
- Para cada validação: o que acontece quando **falha**? O usuário consegue se recuperar ou o fluxo morre ali?
- Existem **fluxos alternativos** para chegar ao mesmo resultado (ex.: login com e-mail vs SSO)?
- Como o fluxo **termina** — quais são os estados finais possíveis (sucesso, erro, cancelado, pendente)?
- Existe algum passo **assíncrono** (o usuário sai e volta depois, notificação, processamento em fila)?

**Quando parar:** consigo desenhar início → fim com todas as decisões nomeadas e pelo menos um destino para cada ramo do losango.

---

## Bloco 2 — Regras de negócio e dados

**Âncora:** *"Quais regras de negócio esse fluxo precisa respeitar? Tem limites, valores mínimos/máximos ou formatos obrigatórios?"*

Aprofundamento:
- Quais **bordas** existem (0, 1, máximo, máximo+1, vazio, duplicado)?
- Quais **estados** a entidade pode ter e quais transições são proibidas?
- O comportamento muda por **perfil/permissão** de usuário? Quais perfis existem?
- Quais **variações de entrada** importam (tipos de dado, caracteres especiais, tamanhos, idiomas)?
- Existe regra que depende de **data/hora, timezone ou vigência**?

**Quando parar:** cada decisão do Bloco 1 tem suas regras e bordas conhecidas, ou a lacuna está registrada.

---

## Bloco 3 — Integrações e dependências

**Âncora:** *"Esse fluxo conversa com o quê — APIs, serviços externos, filas, banco de outro time?"*

Aprofundamento:
- Para cada dependência: o que acontece se ela **falhar**? E se **demorar** (timeout)? E se retornar **dado inesperado**?
- Existe **retry, fallback ou fila de reprocessamento**? O usuário percebe a falha?
- Alguma integração é **assíncrona** (webhook, evento)? O que acontece se o evento chegar duplicado ou fora de ordem?
- Há dependência de **feature flag, configuração ou ambiente**?

**Quando parar:** cada integração citada tem pelo menos o comportamento de falha mapeado ou registrado como pergunta em aberto.

---

## Bloco 4 — Riscos e impacto (alimenta a criticidade)

**Âncora:** *"Se uma coisa só pudesse quebrar nesse fluxo, qual seria a pior? Por quê?"*

Aprofundamento:
- Quais partes do fluxo, se quebrarem, geram **perda financeira, dado corrompido ou usuário travado**?
- Quais **áreas existentes** essa mudança pode afetar (regressão)?
- Esse módulo/área tem **histórico de bugs**? Onde?
- Tem parte **nova/complexa/feita com pressa** (probabilidade maior de defeito)?
- Já existe **matriz PRISMA** dessa feature? Se sim, pedir para reusar (ver `priorizacao-prisma-cores.md`).

**Quando parar:** consigo dar impacto × probabilidade para cada ramo principal do diagrama, com assumptions explícitas onde faltou resposta.

---

## Encerramento da entrevista

Antes de gerar o diagrama, confirmar em uma mensagem curta:

1. Resumo de 3–5 linhas do que foi entendido.
2. Lista das perguntas em aberto acumuladas.
3. *"Posso gerar o fluxograma com isso, ou falta algo?"*
