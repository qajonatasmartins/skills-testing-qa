# Exemplos — Security (verificável aqui, parcial)

Quando o código contém lógica de segurança, a suíte deve prová-la: confidentiality e
authenticity (authn/authz), integrity (proteção contra modificação indevida) e validação
de entrada. Teste de penetração e resistência sob ataque ficam para outro fluxo.

## Cenário 1 — Rota com autorização por papel

**Código sob teste:** `deletarPedido(usuario, pedidoId)` só permite a quem tem papel
`admin` ou é dono do pedido.

**O que a suíte deveria conter:** acesso permitido (admin e dono) **e** negado — papel sem
permissão recebe erro de autorização e o pedido não é apagado.

**Veredito (BLOQUEADO):** se só o caso permitido é testado,
`order.test.ts — autorização negativa não testada — adicionar caso de papel sem
permissão tentando deletar e verificar que é bloqueado.`

## Cenário 2 — Validação de entrada contra payload malicioso

**Código sob teste:** `buscarPorNome(nome)` monta uma query e deve neutralizar entrada
perigosa (ex.: caracteres de injeção).

**O que a suíte deveria conter:** caso com payload malicioso provando que a entrada é
sanitizada/rejeitada e não altera o comportamento da query (integrity).

**Veredito (BLOQUEADO):** ausência →
`search.test.ts — entrada maliciosa não coberta — adicionar caso com payload de injeção
verificando sanitização.`

## Cenário 3 — OK

Toda lógica de authz/validação presente no código tem teste positivo e negativo.
**Veredito:** Security coberta no nível verificável.
