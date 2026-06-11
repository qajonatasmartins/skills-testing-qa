# Exemplos — Maintainability → Testability (verificável aqui)

Aqui o objeto verificado são os **próprios testes**: eles devem ser modulares, legíveis e
isolados (testability, modularity). Testes frágeis e acoplados degradam a suíte mesmo
quando passam. Reusability, analysability e modifiability do código de produção em si
são avaliadas no design, não neste gate.

## Cenário 1 — Dependência de ordem de execução

**Teste sob análise:** um `it` cria um registro num estado global e outro `it` depende
desse registro já existir.

**Por que falha:** rodar os testes isolados ou em outra ordem quebra a suíte — não são
isolados.

**Veredito (BLOQUEADO):** `cart.test.ts:40 — teste depende de estado deixado por outro —
isolar com setup/teardown próprio para cada caso.`

## Cenário 2 — Muitos comportamentos num bloco

**Teste sob análise:** um único `it` com cinco `expect` cobrindo cadastro, login, edição
e logout.

**Por que falha:** quando quebra, não se sabe qual comportamento regrediu; baixa
legibilidade e analysability.

**Veredito (BLOQUEADO):** `auth.test.ts:12 — cinco comportamentos num só teste — dividir
em um it por comportamento (cruza também a Frente 1 / AAA).`

## Cenário 3 — OK

Cada teste prepara seu próprio estado, tem um comportamento e nome descritivo.
**Veredito:** Testability coberta.
