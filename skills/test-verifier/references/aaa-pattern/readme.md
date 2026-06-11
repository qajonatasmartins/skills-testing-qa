# Padrão AAA (Arrange-Act-Assert)

Todo teste segue três fases, **nesta ordem**, e só nesta ordem:

1. **ARRANGE** — prepara tudo: declara variáveis, monta entradas, configura mocks.
2. **ACT** — chama a unidade sob teste **exatamente uma vez**.
3. **ASSERT** — todas as expectativas. Depois da **primeira** assertion não pode haver
   nenhuma declaração, atribuição ou chamada.

Cada teste cobre **um único comportamento**. O nome do teste descreve o comportamento,
não o nome da função.

## Por que importa

A ordem estrita torna o teste legível em três blocos óbvios e impede o anti-padrão mais
comum: "agir → verificar → agir de novo → verificar de novo", que esconde dois
comportamentos num teste só e quebra de forma ambígua. Quando há código depois do
primeiro `expect`, o leitor não sabe mais o que é causa e o que é efeito.

## Itens verificados (checklist)

- [ ] As três fases aparecem na ordem ARRANGE → ACT → ASSERT.
- [ ] O ACT chama a unidade sob teste **uma única vez**.
- [ ] Nenhuma declaração/atribuição/chamada **depois** da primeira assertion.
- [ ] **Um** comportamento por bloco `it`/`test`.
- [ ] O nome do teste descreve o comportamento esperado.

## Exemplos

Casos concretos (certo e errado) com o item de veredito que cada um gera:
`examples/aaa-pattern/readme.md`.
