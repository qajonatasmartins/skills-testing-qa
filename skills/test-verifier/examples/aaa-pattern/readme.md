# Exemplos — Padrão AAA na verificação

Casos concretos do que o verificador encontra ao aplicar a Frente 1. Cada exemplo mostra
o teste sob análise, o que está certo ou errado, e o **item de veredito** que ele gera no
formato da skill (`arquivo:linha — violação — ação`). O princípio e o checklist estão na
reference: `references/aaa-pattern/readme.md`.

---

## 1. Certo — fases claras, uma ação, asserts no fim

```ts
it('rejeita idade abaixo do mínimo de contratação', () => {
  // ARRANGE
  const candidato = { idade: 15 };

  // ACT
  const resultado = podeSerContratado(candidato);

  // ASSERT
  expect(resultado).toBe(false);
});
```

Três fases na ordem certa, um único ACT, asserts só no fim, um comportamento por teste.

**Veredito:** OK — sem item de bloqueio.

---

## 2. Errado — código depois da primeira assertion

```ts
it('valida contratação', () => {
  const candidato = { idade: 15 };
  const resultado = podeSerContratado(candidato);
  expect(resultado).toBe(false);

  const outro = { idade: 16 };                 // declaração após o primeiro expect
  expect(podeSerContratado(outro)).toBe(true); // segundo comportamento no mesmo teste
});
```

Há declaração e segundo ACT depois do primeiro `expect`, e dois comportamentos no mesmo
bloco. O nome do teste ("valida contratação") descreve a função, não o comportamento.

**Veredito (BLOQUEADO):** `hire.test.ts:5 — código e segundo ACT após a primeira
assertion; dois comportamentos num só teste — dividir em dois testes (idade 15 → false;
idade 16 → true), nomeando cada um pelo comportamento.`

---

## 3. Errado — ACT chamado mais de uma vez

```ts
it('incrementa o contador', () => {
  const c = new Counter();
  c.increment();          // ACT #1
  c.increment();          // ACT #2 — qual chamada o assert verifica?
  expect(c.value).toBe(2);
});
```

Duas chamadas à unidade antes do assert: não fica claro qual comportamento é verificado.

**Veredito (BLOQUEADO):** `counter.test.ts:4 — ACT chamado duas vezes — testar o
incremento unitário e o acúmulo em testes separados, ou mover o estado prévio para o
ARRANGE e deixar um único ACT.`

---

## 4. Errado — asserts intercalados com ações

```ts
it('fluxo de saque', () => {
  const conta = new Conta(100);
  expect(conta.saldo).toBe(100);   // assert no meio
  conta.sacar(30);                 // ação após assert
  expect(conta.saldo).toBe(70);
});
```

O primeiro `expect` verifica o estado inicial (que é ARRANGE) e há uma ação depois dele.

**Veredito (BLOQUEADO):** `account.test.ts:3 — assertion intercalada com ação — o estado
inicial é ARRANGE (sem assert), o ACT é sacar(30) e o ASSERT final verifica o saldo
resultante.`
