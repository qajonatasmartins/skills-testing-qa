# Exemplos de Control Flow Testing

Cada exemplo abaixo mantém a descrição do cenário e acrescenta o **método sob teste** e os **testes Jest** derivados pela técnica (caminhos básicos sensibilizados por dados de entrada). Os testes seguem **AAA estrito**: ARRANGE → ACT (uma chamada) → ASSERT (um `expect`). Cada teste é um `it()` separado, com nome que **começa por um verbo no infinitivo** (`rejeitar`, `retornar`, `executar`, `validar`…) e em seguida descreve o comportamento validado e o caminho exercitado.

Exemplos lógicos e de código:

---

## 1. Laço inviável (Loop infinito/exaustivo)

Um código em `for (i=1; i<=1000...)` com três camadas aninhadas, usado para mostrar que testar todo caminho exaustivamente faria a função rodar um bilhão de vezes.

> **Adaptação didática:** loop reduzido a 3 iterações para demonstrar cobertura de loop (0, 1, n, m-1, m+1) sem bilhões de execuções.

### Método

```typescript
function somaLoop(limit: number, vezes: number): number {
  let total = 0;
  for (let i = 0; i < vezes; i++) {
    total += limit;
  }
  return total;
}
```

### Testes gerados

```typescript
describe("somaLoop — cobertura de loop", () => {
  it("retornar 0 quando vezes é 0, caminho zero iterações", () => {
    const resultado = somaLoop(5, 0);

    expect(resultado).toBe(0);
  });

  it("retornar 5 quando vezes é 1, caminho uma iteração", () => {
    const resultado = somaLoop(5, 1);

    expect(resultado).toBe(5);
  });

  it("retornar 15 quando vezes é 3, caminho n iterações típicas", () => {
    const resultado = somaLoop(5, 3);

    expect(resultado).toBe(15);
  });
});
```

### Cobertura de caminhos

| Caminho | vezes | Esperado | Nível |
|---------|-------|----------|-------|
| 0 iterações | 0 | 0 | Loop coverage |
| 1 iteração | 1 | 5 | Loop coverage |
| n iterações | 3 | 15 | Loop coverage |

---

## 2. Erro de caminho ausente (Missing path)

Um pequeno código onde apenas o `if (a>0)` e o `if (a==0)` foram programados, ilustrando que a ferramenta não pegaria a ausência esquecida do `if (a<0)`.

### Método

```typescript
function classificarA(a: number): string {
  if (a > 0) return "positivo";
  if (a === 0) return "zero";
  return "indefinido";
}
```

### Testes gerados

```typescript
describe("classificarA — caminho ausente", () => {
  it("retornar positivo quando a=1, caminho a>0", () => {
    const resultado = classificarA(1);

    expect(resultado).toBe("positivo");
  });

  it("retornar zero quando a=0, caminho a==0", () => {
    const resultado = classificarA(0);

    expect(resultado).toBe("zero");
  });

  it("retornar indefinido quando a=-1, caminho a<0 ausente no design", () => {
    const resultado = classificarA(-1);

    expect(resultado).toBe("indefinido");
  });
});
```

### Cobertura de caminhos

| Caminho | Entrada | Resultado | Coberto? |
|---------|---------|-----------|----------|
| a > 0 | 1 | positivo | Sim |
| a == 0 | 0 | zero | Sim |
| a < 0 | -1 | indefinido | **Caminho ausente no código original** |

---

## 3. Trecho de duas decisões sequenciais

O livro traz o grafo de um código pequeno: `if a>0` e logo após `if b==3`. O autor usa esse exemplo para destrinchar os Níveis 1 e 2 de Cobertura (comandos x decisões), exibindo visualmente como apenas dois testes podem não cobrir todos os 4 caminhos possíveis que o programa tem.

### Método

```typescript
function duasDecisoes(a: number, b: number): string {
  if (a > 0) {
    if (b === 3) return "AB";
    return "A";
  }
  if (b === 3) return "B";
  return "none";
}
```

### Testes gerados

```typescript
describe("duasDecisoes — cobertura de decisão", () => {
  it("retornar AB quando a=1 e b=3, caminho A=true B=true", () => {
    const resultado = duasDecisoes(1, 3);

    expect(resultado).toBe("AB");
  });

  it("retornar A quando a=1 e b=0, caminho A=true B=false", () => {
    const resultado = duasDecisoes(1, 0);

    expect(resultado).toBe("A");
  });

  it("retornar B quando a=-1 e b=3, caminho A=false B=true", () => {
    const resultado = duasDecisoes(-1, 3);

    expect(resultado).toBe("B");
  });

  it("retornar none quando a=-1 e b=0, caminho A=false B=false", () => {
    const resultado = duasDecisoes(-1, 0);

    expect(resultado).toBe("none");
  });
});
```

### Cobertura de caminhos

| Caminho | a | b | Resultado | Decisão A | Decisão B |
|---------|---|---|-----------|-----------|-----------|
| 1 | 1 | 3 | AB | T | T |
| 2 | 1 | 0 | A | T | F |
| 3 | -1 | 3 | B | F | T |
| 4 | -1 | 0 | none | F | F |

> Complexidade ciclomática: **C = 3** (4 caminhos). Nível 2 (decisão) exige T e F em cada decisão.
