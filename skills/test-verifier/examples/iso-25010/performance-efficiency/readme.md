# Exemplos — Performance Efficiency (delegar)

Tempo de resposta, throughput e uso de recursos (time behaviour, resource utilization,
capacity) não se medem de forma confiável em teste unitário — pertencem a teste de
carga/performance. Aqui o verificador **sinaliza a lacuna**, não bloqueia por ela.

## Cenário 1 — Endpoint com SLA de latência

**Código sob teste:** `listarProdutos()` tem requisito de responder em < 200 ms sob carga.

**O que NÃO fazer:** transformar isso em assert de tempo (`expect(ms).toBeLessThan(200)`)
no unitário — resultado instável, depende da máquina do CI.

**Veredito:** APROVADO na parte unitária, com observação —
`Performance Efficiency: SLA de 200 ms não verificável em unitário — delegar a teste de
carga/perf no outro fluxo.`

## Cenário 2 — Estrutura de dados com custo declarado

**Código sob teste:** uma função que promete busca O(log n).

**O que checar aqui:** apenas a correção do resultado (Functional Suitability). A garantia
de complexidade/recursos é medição de performance.

**Veredito:** sinalizar a complexidade como item para o fluxo de performance, sem bloquear.
