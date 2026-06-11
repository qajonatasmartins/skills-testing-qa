# Exemplos de MC/DC

Cada exemplo abaixo mantém a descrição do cenário e acrescenta o **método sob teste** e os **testes Jest** derivados pela técnica (mínimo de *n+1* casos por decisão, com pares independentes). Os testes seguem **AAA estrito**: ARRANGE → ACT (uma chamada) → ASSERT (um `expect`). Cada teste é um `it()` separado, com nome que **começa por um verbo no infinitivo** (`retornar`, `validar`, `demonstrar`…) e em seguida descreve o comportamento validado e a condição independente exercitada.

Aplicações da técnica em linguagens de programação na avaliação passo a passo:

---

## Exemplo 1 (`A := (B and C) or D`)

Mostra como preencher o esquema de portas e como a variável *D* se for *Verdadeira* causa "mascaramento" na sub-expressão `B and C`, o que desqualifica testes para as portas inferiores e exige a análise correta para atingir a conformidade.

### Método

```typescript
function exemplo1(B: boolean, C: boolean, D: boolean): boolean {
  return (B && C) || D;
}
```

### Testes gerados (n+1 = 4 casos)

```typescript
describe("exemplo1 — (B and C) or D", () => {
  it("retornar true quando B=T,C=T,D=F, par base AND verdadeiro", () => {
    const resultado = exemplo1(true, true, false);

    expect(resultado).toBe(true);
  });

  it("retornar false quando B=F,C=T,D=F, B muda resultado independentemente", () => {
    const resultado = exemplo1(false, true, false);

    expect(resultado).toBe(false);
  });

  it("retornar false quando B=T,C=F,D=F, C muda resultado independentemente", () => {
    const resultado = exemplo1(true, false, false);

    expect(resultado).toBe(false);
  });

  it("retornar true quando B=F,C=F,D=T, D muda resultado independentemente", () => {
    const resultado = exemplo1(false, false, true);

    expect(resultado).toBe(true);
  });
});
```

### Tabela verdade MC/DC

| Caso | B | C | D | A | Par independente |
|------|---|---|---|-----|------------------|
| 1 | T | T | F | T | Base |
| 2 | F | T | F | F | B: T→F muda A |
| 3 | T | F | F | F | C: T→F muda A |
| 4 | F | F | T | T | D: F→T muda A |

> Quando D=T, B e C ficam **mascarados** — testes com D=T não provam independência de B ou C.

---

## Exemplo 2 (`Z := (A and not B) or (C xor D)`)

Inclui o uso prático de blocos *XOR* (que possuem 4 combinações válidas diferentes) e inversores *NOT* avaliando se os requerimentos foram suficientes.

### Método

```typescript
function exemplo2(A: boolean, B: boolean, C: boolean, D: boolean): boolean {
  return (A && !B) || (C !== D);
}
```

### Testes gerados

```typescript
describe("exemplo2 — (A and not B) or (C xor D)", () => {
  it("retornar true quando A=T,B=F,C=F,D=F, par base (A and not B)", () => {
    const resultado = exemplo2(true, false, false, false);

    expect(resultado).toBe(true);
  });

  it("retornar false quando A=T,B=T,C=F,D=F, B muda resultado via NOT", () => {
    const resultado = exemplo2(true, true, false, false);

    expect(resultado).toBe(false);
  });

  it("retornar false quando A=F,B=F,C=F,D=F, A muda resultado independentemente", () => {
    const resultado = exemplo2(false, false, false, false);

    expect(resultado).toBe(false);
  });

  it("retornar true quando A=F,B=F,C=T,D=F, C muda resultado via XOR", () => {
    const resultado = exemplo2(false, false, true, false);

    expect(resultado).toBe(true);
  });

  it("retornar false quando A=F,B=F,C=T,D=T, D muda resultado via XOR", () => {
    const resultado = exemplo2(false, false, true, true);

    expect(resultado).toBe(false);
  });
});
```

### Tabela verdade MC/DC

| Caso | A | B | C | D | Z | Par |
|------|---|---|---|---|---|-----|
| 1 | T | F | F | F | T | Base |
| 2 | T | T | F | F | F | B (via NOT) |
| 3 | F | F | F | F | F | A |
| 4 | F | F | T | F | T | C (via XOR) |
| 5 | F | F | T | T | F | D (via XOR) |

---

## Exemplo 3 (Lógica de curto-circuito)

Código avaliado com o uso de operadores booleanos rápidos em linguagem *Ada* ou *C* (`Z := A and then B and then C…`). Explica como tratar as variáveis subsequentes que o compilador nem sequer processa como dados neutros "Don't cares" sem invalidar a regra do teste.

### Método

```typescript
function exemplo3(A: boolean, B: boolean, C: boolean): boolean {
  if (!A) return false;
  if (!B) return false;
  return C;
}
```

### Testes gerados

```typescript
describe("exemplo3 — short-circuit (and then)", () => {
  it("retornar false quando A=F, B e C são don't care por curto-circuito", () => {
    const resultado = exemplo3(false, true, true);

    expect(resultado).toBe(false);
  });

  it("retornar false quando A=T,B=F, C é don't care por curto-circuito", () => {
    const resultado = exemplo3(true, false, true);

    expect(resultado).toBe(false);
  });

  it("retornar true quando A=T,B=T,C=T, caminho completo", () => {
    const resultado = exemplo3(true, true, true);

    expect(resultado).toBe(true);
  });

  it("retornar false quando A=T,B=T,C=F, C muda resultado independentemente", () => {
    const resultado = exemplo3(true, true, false);

    expect(resultado).toBe(false);
  });
});
```

### Tabela verdade MC/DC (short-circuit)

| Caso | A | B | C | Z | Don't care |
|------|---|---|---|---|------------|
| 1 | F | DC | DC | F | B, C |
| 2 | T | F | DC | F | C |
| 3 | T | T | T | T | — |
| 4 | T | T | F | F | C independente |
