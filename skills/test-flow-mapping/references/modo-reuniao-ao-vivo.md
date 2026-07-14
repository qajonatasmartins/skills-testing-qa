# Protocolo — Modo Reunião ao Vivo

O usuário está em uma reunião (refinamento, planning, discussão técnica) e vai jogando anotações soltas no terminal. A skill atua como **escriba silencioso-organizador**.

## Postura

- **Confirmar cada anotação em 1 linha**, sem elaborar. Ex.: `✓ anotado: bloqueio após 3 tentativas`.
- **Nunca fazer perguntas durante a coleta** — a reunião tem prioridade. Lacunas detectadas vão para a lista interna de perguntas em aberto.
- Não corrigir, não sugerir, não resumir sem ser pedido.
- Aceitar anotações em qualquer formato: frases soltas, meia palavra, colagem de chat.

## Classificação interna de cada anotação

Ao receber, classificar silenciosamente como:

| Tipo | Sinal típico | Vai para |
|------|--------------|----------|
| **Passo** | ação do usuário/sistema ("usuário preenche X") | nó do fluxo |
| **Decisão** | condição, "se", "quando", "depende" | losango |
| **Exceção** | falha, erro, timeout, "e se não" | ramo de exceção |
| **Regra** | limite, formato, permissão, estado | anotação no nó ou nó de validação |
| **Dúvida** | "não sabemos", "perguntar pro fulano", "?" | perguntas em aberto |
| **Decisão do time** | "ficou decidido", "vamos fazer X", "fora de escopo" | ata |
| **Responsável** | "fulano vai ver isso" | ata (quem ficou com o quê) |

Em caso de ambiguidade, classificar pelo mais provável e marcar internamente como incerto — vira pergunta em aberto se continuar ambíguo no fechamento.

## Comandos do usuário

| Comando (ou variação natural) | Ação |
|-------------------------------|------|
| `mostra o fluxo` / "como tá o diagrama?" | Gerar **prévia parcial** do Mermaid com o que existe até agora (sem criticidade, sem ata) — rápido, para projetar na reunião |
| `isso é dúvida` | Reclassificar a última anotação como pergunta em aberto |
| `isso é decisão` | Reclassificar a última anotação como decisão do time (ata) |
| `fechar` / "acabou a reunião" | Iniciar o **fechamento** (abaixo) |

## Fechamento

Ao receber `fechar`:

1. Perguntar (agora sim pode): **tipo de diagrama**, se ainda não escolhido no início, e até **3 lacunas críticas** que impedem o desenho — o resto fica como pergunta em aberto.
2. Classificar criticidade dos ramos (`priorizacao-prisma-cores.md`) — se não houver informação de risco nas anotações, usar mini-scoring com assumptions.
3. Gerar o entregável completo conforme `template-saida.md`, incluindo:
   - **Ata resumida**: pontos discutidos, decisões do time, responsáveis citados.
   - **Perguntas em aberto**: dúvidas anotadas + lacunas detectadas passivamente.
4. Salvar em `output/test-flow-mapping/[slug]-[YYYYMMDD].md`.

## Início da sessão

Na primeira mensagem do modo reunião, responder curto (máx. 4 linhas):

> Modo reunião ativado. Vou anotando em silêncio.
> Comandos: `mostra o fluxo` (prévia) · `isso é dúvida` · `isso é decisão` · `fechar` (entrega final).
> Tipo de diagrama: fluxo funcional ou árvore de cenários? (pode decidir no fechar)
