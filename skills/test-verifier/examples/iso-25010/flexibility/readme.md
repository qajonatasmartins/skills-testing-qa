# Exemplos — Flexibility (delegar)

Adaptability (rodar em outros ambientes), scalability (crescer/encolher carga),
installability e replaceability se confirmam em ambiente de implantação real, não em
unitário. O verificador **sinaliza a lacuna** e cobre só a lógica configurável.

## Cenário 1 — Feature configurável por ambiente

**Código sob teste:** `obterConfig()` lê variáveis e habilita/desabilita um recurso por
ambiente (dev/staging/prod).

**O que checar aqui:** que, dada cada combinação de variáveis, a função retorna a config
certa (Functional Suitability). Que o deploy em cada ambiente real funciona
(adaptability/installability) é outro fluxo.

**Veredito:** APROVADO na lógica de config, com observação —
`Flexibility: comportamento em cada ambiente real e instalação não verificáveis em
unitário — delegar ao fluxo de implantação.`

## Cenário 2 — Troca de provedor (replaceability)

**Código sob teste:** uma interface de storage com duas implementações (S3 e local).

**O que checar aqui:** que ambas as implementações cumprem o contrato (testes contra a
interface). Que uma substitui a outra em produção sem regressão é cenário de implantação.

**Veredito:** sinalizar a substituição em ambiente real para o outro fluxo, sem bloquear.
