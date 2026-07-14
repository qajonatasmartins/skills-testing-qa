# Template de Saída

Estrutura fixa do entregável, salvo em `output/test-flow-mapping/[slug]-[YYYYMMDD].md` (slug = nome curto da feature em kebab-case).

Seções marcadas *(condicional)* só entram quando aplicáveis; as demais são obrigatórias — se vazias, escrever "Nenhum(a)" em vez de omitir.

````markdown
# Mapa de testes — [nome da feature]

## Contexto e modo utilizado

- **Modo:** entrevista guiada | reunião ao vivo | texto pronto
- **Tipo de diagrama:** fluxo funcional com pontos de teste | árvore de cenários | ambos
- **Fonte da criticidade:** matriz PRISMA ([link/data]) | mini-scoring com assumptions | não classificada
- **Data:** YYYY-MM-DD
- [2–4 linhas resumindo a feature e o objetivo do mapeamento]

## Fluxograma

```mermaid
[diagrama conforme sintaxe-mermaid-testes.md]
```

🟥 Q I Must Test · 🟧 Q II Should Test · 🟨 Q III Could Test · ⬜ Q IV Won't Test

## Cenários mapeados

| ID | Cenário | Tipo | Criticidade |
|----|---------|------|-------------|
| CN-01 | [descrição one-liner] | feliz / alternativo / exceção / borda | Q I (RI-xx) |

## Perguntas em aberto

- [ ] [dúvida não respondida — levar ao time / a quem foi citado]

## Ata resumida da discussão *(modo reunião)*

**Pontos discutidos:**
- [...]

**Decisões do time:**
- [...]

**Responsáveis citados:**
- [pessoa] — [o que ficou de fazer]

## Premissas assumidas

- [assumption de scoring ou de fluxo inferido, com justificativa]

## Próximos passos sugeridos

- [ex.: rodar `prisma-risk-testing` para análise formal; usar CN-xx como entrada de `test-design` / `write-test-case`]
````

## Regras de preenchimento

1. IDs de cenário (`CN-01`…) idênticos aos dos nós-folha da árvore de cenários (quando esse tipo for gerado).
2. No fluxo funcional, a tabela de cenários deriva dos ramos: cada caminho distinto início→fim relevante para teste vira um `CN-xx`.
3. "Perguntas em aberto" nunca vazia no modo reunião se houve anotação classificada como dúvida.
4. Se o diagrama foi dividido em subdiagramas (>30 nós), cada um ganha um subtítulo `### Parte N — [área]` dentro de "Fluxograma".
