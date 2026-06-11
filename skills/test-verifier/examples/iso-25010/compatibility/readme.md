# Exemplos — Compatibility (delegar)

Co-existence (compartilhar ambiente/recursos sem prejudicar outros produtos) e
interoperability (trocar e usar informação com outros sistemas) só se confirmam em
integração de sistema real. No unitário/componente o verificador **sinaliza a lacuna**.

## Cenário 1 — Integração com sistema legado

**Código sob teste:** um adaptador que converte mensagens para um ERP legado.

**O que checar aqui:** com mock do ERP, a correção do mapeamento (Functional Suitability).
A interoperabilidade real (o ERP aceita e entende a mensagem) não é verificável com mock.

**Veredito:** APROVADO na parte de componente, com observação —
`Compatibility: interoperabilidade com o ERP real não verificável com mock — delegar a
teste de integração de sistema.`

## Cenário 2 — Dois serviços compartilhando o mesmo banco

**Código sob teste:** um serviço novo grava numa tabela usada por outro serviço.

**O que checar aqui:** a lógica de escrita do serviço. Co-existência (não corromper o uso
do outro serviço) é cenário de integração.

**Veredito:** sinalizar a co-existência como item do fluxo de integração, sem bloquear.
