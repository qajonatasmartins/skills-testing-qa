# Exemplos — Safety (delegar em geral; verificar a lógica fail-safe unitária)

Risk identification, hazard warning e safe integration dependem de análise de sistema. A
exceção: quando o código **contém** lógica fail-safe ou de operational constraint, essa
lógica é verificável em unitário e deve ser testada aqui.

## Cenário 1 — Lógica fail-safe presente no código (verificar aqui)

**Código sob teste:** `controlarValvula(pressao)` deve fechar a válvula (estado seguro)
sempre que a pressão exceder o limite ou a leitura do sensor for inválida.

**O que a suíte deveria conter:** caso de pressão acima do limite → válvula fecha; caso de
leitura inválida (NaN, sensor offline) → válvula fecha (fail-safe).

**Veredito (BLOQUEADO):** se a leitura inválida não é testada,
`valve.test.ts — fail-safe para leitura inválida não coberto — adicionar caso de sensor
offline verificando que a válvula vai ao estado seguro.`

## Cenário 2 — Análise de perigo de sistema (delegar)

**Código sob teste:** o mesmo módulo, mas o requisito é "o sistema nunca deve permitir
sobrepressão em nenhuma combinação de subsistemas".

**O que checar aqui:** apenas a unidade `controlarValvula`. A garantia ponta a ponta entre
subsistemas (safe integration, risk identification) é análise de sistema.

**Veredito:** APROVADO na unidade, com observação —
`Safety: garantia de integração segura entre subsistemas não verificável em unitário —
delegar à análise de segurança de sistema.`
