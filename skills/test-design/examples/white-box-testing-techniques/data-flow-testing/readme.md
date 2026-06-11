# Exemplos de Data Flow Testing

Cada exemplo abaixo mantém a descrição do cenário e acrescenta o **método sob teste** e os **testes Jest** derivados pela técnica (pares define-use e padrões d/u/k/~). Os testes seguem **AAA estrito**: ARRANGE → ACT (uma chamada) → ASSERT (um `expect`). Cada teste é um `it()` separado, com nome que **começa por um verbo no infinitivo** (`rejeitar`, `retornar`, `detectar`, `validar`…) e em seguida descreve o comportamento validado e o par def-use exercitado.

---

## 1. Escopo de bloco em TypeScript/Java/Kotlin

Veja como o escopo afeta a existência das variáveis:

**TypeScript:**

```typescript
   {
     let x = 1; // definido
     {
       let y = 2; // definido
       // y está válido dentro deste bloco
     } // y é automaticamente destruído aqui (morto)
     // x ainda está válido aqui
   }
   // x não existe mais (fora do escopo)
```

**Java/Kotlin:**

```java
   {
     int x = 1; // definido
     {
       int y = 2; // definido
       // y está disponível apenas neste bloco
     } // y morreu aqui
     // x ainda existe
   }
   // x morreu aqui
```

### Método

```typescript
function escopoBloco(): { xValido: boolean; yMorto: boolean } {
  let x = 1;
  let yMorto = false;
  {
    let y = 2;
    void y;
  }
  yMorto = true;
  return { xValido: x === 1, yMorto };
}
```

### Testes gerados

```typescript
describe("escopoBloco — ciclo de vida por bloco", () => {
  it("retornar xValido true quando x sobrevive ao bloco interno, par d-u de x", () => {
    const resultado = escopoBloco();

    expect(resultado.xValido).toBe(true);
  });

  it("retornar yMorto true quando y é destruído ao sair do bloco interno, par k de y", () => {
    const resultado = escopoBloco();

    expect(resultado.yMorto).toBe(true);
  });
});
```

### Tabela define-use

| Variável | Sequência | Padrão | Aceitável? |
|----------|-----------|--------|------------|
| x | d → u (fora do bloco) | du | Sim |
| y | d → k (fim do bloco) | dk | Sim (escopo) |

---

## 2. Fluxo de Controle: Definições, Usos e Mortes

Exemplo analisando variáveis `a`, `b` e `c` em TypeScript:

```typescript
   let a; // ~ (não existe valor definido)
   a = 10; // d (definido)
   let b = a + 5; // u (a é usado), d (b é definido)
   let c;
   // c está apenas declarado (~)
   c = b * 2; // u (b), d (c)
   // a e b continuam vivos aqui, c também.
```

Possíveis defeitos a serem identificados:

- Usar uma variável antes de definir (`console.log(d);` gera erro).
- Definir uma variável e nunca usar (`let z = 42;`).
- Redefinir uma variável sem uso (`a = 20; // dd se não houver uso entre as definições`).

### Método

```typescript
function fluxoNormal(): number {
  let a: number;
  a = 10;
  const b = a + 5;
  let c: number;
  c = b * 2;
  return c;
}

function usoAntesDefinir(): number {
  let d: number;
  // d é usado em um cálculo (~u) antes de receber valor: undefined + 1 = NaN em runtime
  return (d as number) + 1;
}

function definirSemUsar(): number {
  const z = 42;
  return 0;
}

function redefinirSemUso(): number {
  let a = 10;
  a = 20;
  return a;
}
```

### Testes gerados

```typescript
describe("fluxo de dados — padrões d/u/k/~", () => {
  it("retornar 30 quando a,b,c seguem sequência d-u-d-u-d, par def-use normal", () => {
    const resultado = fluxoNormal();

    expect(resultado).toBe(30);
  });

  it("retornar NaN quando d é usado antes de definir, padrão ~u inválido", () => {
    const resultado = usoAntesDefinir();

    expect(Number.isNaN(resultado)).toBe(true);
  });

  it("retornar 0 quando z é definido mas nunca usado, padrão d~ suspeito", () => {
    const resultado = definirSemUsar();

    expect(resultado).toBe(0);
  });

  it("retornar 20 quando a é redefinido sem uso intermediário, padrão dd suspeito", () => {
    const resultado = redefinirSemUso();

    expect(resultado).toBe(20);
  });
});
```

### Tabela define-use

| Função | Variável | Sequência | Padrão | Defeito? |
|--------|----------|-----------|--------|----------|
| fluxoNormal | a | d → u (em b) | du | Não |
| fluxoNormal | b | d → u (em c) | du | Não |
| fluxoNormal | c | d → u (return) | du | Não |
| usoAntesDefinir | d | ~ → u | ~u | **Sim (grave)** |
| definirSemUsar | z | d → (nunca u) | d~ | Suspeito |
| redefinirSemUso | a | d → d | dd | Suspeito |

---

## 3. Limitação com Arrays Dinâmicos

Mesmo em linguagens modernas, analisar dinamicamente elementos individuais em arrays pode ser um desafio para análise estática:

**TypeScript:**

```typescript
   let arr: number[] = [];
   arr[0] = 10; // definido arr[0]
   arr[1] = arr[0] + 5; // uso arr[0], definição arr[1]
   arr[2] = arr[3] + 2; // ~u: arr[3] usado sem definição
```

Só a presença do array não garante que todos os índices tenham valores definidos, dificultando a análise estática de cada elemento.

### Método

```typescript
function manipularArray(): number[] {
  const arr: number[] = [];
  arr[0] = 10;
  arr[1] = arr[0] + 5;
  arr[2] = (arr[3] ?? 0) + 2;
  return arr;
}

function arrayComUsoIndefinido(): number {
  const arr: number[] = [];
  arr[0] = 10;
  return arr[3] + 2;
}
```

### Testes gerados

```typescript
describe("data flow em arrays", () => {
  it("retornar arr[1]=15 quando arr[0] é definido e usado, par def-use arr[0]→arr[1]", () => {
    const resultado = manipularArray();

    expect(resultado[1]).toBe(15);
  });

  it("retornar arr[2]=2 quando arr[3] é undefined e tratado com ??, par ~u mitigado", () => {
    const resultado = manipularArray();

    expect(resultado[2]).toBe(2);
  });

  it("retornar NaN quando arr[3] é usado sem definição, padrão ~u em arr[3]", () => {
    const resultado = arrayComUsoIndefinido();

    expect(Number.isNaN(resultado)).toBe(true);
  });
});
```

### Tabela define-use (arrays)

| Índice | Sequência | Padrão | Aceitável? |
|--------|-----------|--------|------------|
| arr[0] | d → u (em arr[1]) | du | Sim |
| arr[1] | d (de arr[0]+5) | d | Sim |
| arr[3] | ~ → u (em arr[2] sem ??) | ~u | **Não** |
| arr[3] | ~ → u (com ?? 0) | ~u mitigado | Aceitável com fallback |

---

## Resumo dos padrões de data flow

| Padrão | Significado | Exemplo no código | Gravidade |
|--------|-------------|-------------------|-----------|
| **du** | definido → usado | `a=10; b=a+5` | Normal |
| **~u** | não existe → usado | `return d` sem atribuir | **Erro grave** |
| **ku** | morto → usado | usar `y` fora do bloco | **Erro grave** |
| **dd** | definido → redefinido | `a=10; a=20` sem uso | Suspeito |
| **d~** | definido → nunca usado | `z=42` sem referência | Suspeito |
| **ud** | usado → definido | ciclo normal | Normal |
| **uk** | usado → morto (escopo) | fim de bloco | Normal |
