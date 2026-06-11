# Exemplos — Reliability (verificável aqui, parcial)

Verifica se a suíte exercita o que mantém o componente funcionando sob falha:
faultlessness (sem falha em operação normal), fault tolerance (opera apesar de falhas) e
recoverability (recupera estado após interrupção). Availability é de nível de sistema.

## Cenário 1 — Chamada externa que pode falhar

**Código sob teste:** `buscarPerfil(id)` chama uma API e, em erro de rede, retorna cache.

**O que a suíte deveria conter:** caminho feliz **e** o de falha — mock da API lançando
erro, provando que o cache é retornado (fault tolerance), além do timeout.

**Veredito (BLOQUEADO):** se só o sucesso é testado,
`profile.test.ts — só o caminho feliz coberto — tolerância a falha não verificada:
adicionar caso em que a API falha e o cache é servido.`

## Cenário 2 — Recuperação de estado após interrupção

**Código sob teste:** `retomarUpload()` continua um upload interrompido a partir do
último bloco confirmado.

**O que a suíte deveria conter:** um teste que simula interrupção no meio e verifica que a
retomada parte do ponto certo (recoverability).

**Veredito (BLOQUEADO):** ausência desse teste →
`upload.test.ts — recuperação pós-interrupção não testada — adicionar caso que
interrompe e retoma do último bloco confirmado.`

## Cenário 3 — OK

Cada caminho de erro relevante tem teste de tolerância/recuperação. **Veredito:**
Reliability coberta no nível verificável; resiliência sob carga real fica para o outro fluxo.
