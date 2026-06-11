# Exemplos — Interaction Capability (delegar)

Usabilidade, aprendizado, operabilidade, proteção contra erro do usuário e acessibilidade
dependem da interface real e de usuários — verificam-se em teste manual/E2E, não em
unitário. O verificador **sinaliza a lacuna** e cobre só a lógica por trás da UI.

## Cenário 1 — Componente de formulário com fluxo de usuário

**Código sob teste:** um componente de checkout com validação de campos e mensagens de
erro.

**O que checar aqui:** a lógica de validação (estado de erro calculado para entrada
inválida) — isso é Functional Suitability. Se a mensagem é clara, se o fluxo é intuitivo,
se é acessível por teclado/leitor de tela → Interaction Capability, fora do escopo.

**Veredito:** APROVADO na lógica, com observação —
`Interaction Capability: clareza das mensagens e acessibilidade não verificáveis aqui —
delegar a teste manual/E2E.`

## Cenário 2 — Proteção contra erro do usuário

**Código sob teste:** confirmação antes de uma ação destrutiva (apagar conta).

**O que checar aqui:** que a ação só dispara após confirmação (lógica). Se o usuário
entende o aviso a tempo (hazard warning de UI) é avaliação de usabilidade.

**Veredito:** sinalizar a experiência de confirmação para o fluxo de E2E/manual, sem bloquear.
