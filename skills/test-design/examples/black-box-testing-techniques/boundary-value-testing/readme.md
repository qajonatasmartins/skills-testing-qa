# Exemplos de Boundary Value Testing

Cada exemplo abaixo mantém a descrição do cenário e acrescenta o **método sob teste** e os **testes Jest** derivados pela técnica (três pontos por limite: logo abaixo, sobre, logo acima). Os testes seguem **AAA estrito**: ARRANGE → ACT (uma chamada) → ASSERT (um `expect`). Cada teste é um `it()` separado, com nome que **começa por um verbo no infinitivo** (`rejeitar`, `retornar`, `aprovar`, `validar`…) e em seguida descreve o comportamento validado e o ponto de limite exercitado.

---

## 1. Regras de Contratação Baseadas em Idade

Um sistema que decide o status de contratação baseado na idade: 0–15 (Não contrata), 16–17 (Meio período), 18–54 (Tempo integral) e 55–99 (Não contrata).

**Limites e testes gerados:** Os valores limites interessantes são avaliados em {-1, 0, 1}, {15, 16, 17}, {17, 18, 19}, {54, 55, 56} e {98, 99, 100}.

Valores adicionais (ex: letras ou números extremos) poderiam entrar apenas se as pré-condições do design defensivo do sistema exigissem.

### Método

``` typescript
type StatusContratacao = "nao-contrata" | "meio-periodo" | "tempo-integral";

function classificarContratacao(idade: number): StatusContratacao {
  if (idade < 0 || idade > 99) return "nao-contrata";
  if (idade <= 15) return "nao-contrata";
  if (idade <= 17) return "meio-periodo";
  if (idade <= 54) return "tempo-integral";
  return "nao-contrata";
}
```

### Testes gerados

``` typescript
describe("classificarContratacao", () => {
  // limite inferior da faixa 0–15
  it("rejeitar contratação quando idade é -1, abaixo do limite mínimo da faixa válida", () => {
    const resultado = classificarContratacao(-1);

    expect(resultado).toBe("nao-contrata");
  });

  it("retornar não contrata quando idade é 0, no limite inferior da faixa 0–15", () => {
    const resultado = classificarContratacao(0);

    expect(resultado).toBe("nao-contrata");
  });

  it("retornar não contrata quando idade é 1, logo acima do limite inferior 0–15", () => {
    const resultado = classificarContratacao(1);

    expect(resultado).toBe("nao-contrata");
  });

  // limite entre 0–15 e 16–17
  it("retornar não contrata quando idade é 15, no último valor da faixa 0–15", () => {
    const resultado = classificarContratacao(15);

    expect(resultado).toBe("nao-contrata");
  });

  it("retornar meio período quando idade é 16, no primeiro valor da faixa 16–17", () => {
    const resultado = classificarContratacao(16);

    expect(resultado).toBe("meio-periodo");
  });

  it("retornar meio período quando idade é 17, no último valor da faixa 16–17", () => {
    const resultado = classificarContratacao(17);

    expect(resultado).toBe("meio-periodo");
  });

  // limite entre 16–17 e 18–54
  it("retornar tempo integral quando idade é 18, no primeiro valor da faixa 18–54", () => {
    const resultado = classificarContratacao(18);

    expect(resultado).toBe("tempo-integral");
  });

  it("retornar tempo integral quando idade é 19, logo acima do limite 16–17", () => {
    const resultado = classificarContratacao(19);

    expect(resultado).toBe("tempo-integral");
  });

  // limite entre 18–54 e 55–99
  it("retornar tempo integral quando idade é 54, no último valor da faixa 18–54", () => {
    const resultado = classificarContratacao(54);

    expect(resultado).toBe("tempo-integral");
  });

  it("retornar não contrata quando idade é 55, no primeiro valor da faixa 55–99", () => {
    const resultado = classificarContratacao(55);

    expect(resultado).toBe("nao-contrata");
  });

  it("retornar não contrata quando idade é 56, logo acima do limite 18–54", () => {
    const resultado = classificarContratacao(56);

    expect(resultado).toBe("nao-contrata");
  });

  // limite superior da faixa 55–99
  it("retornar não contrata quando idade é 98, no penúltimo valor da faixa 55–99", () => {
    const resultado = classificarContratacao(98);

    expect(resultado).toBe("nao-contrata");
  });

  it("retornar não contrata quando idade é 99, no limite superior da faixa válida", () => {
    const resultado = classificarContratacao(99);

    expect(resultado).toBe("nao-contrata");
  });

  it("rejeitar contratação quando idade é 100, acima do limite máximo da faixa válida", () => {
    const resultado = classificarContratacao(100);

    expect(resultado).toBe("nao-contrata");
  });
});
```

### Mapa de limites

| Limite | Abaixo | Sobre | Acima | Resultado esperado |
|--------|--------|-------|-------|--------------------|
| Início da faixa 0–15 | -1 | 0 | 1 | nao-contrata |
| Entre 0–15 e 16–17 | 15 | 16 | 17 | nao-contrata / meio-periodo |
| Entre 16–17 e 18–54 | 17 | 18 | 19 | meio-periodo / tempo-integral |
| Entre 18–54 e 55–99 | 54 | 55 | 56 | tempo-integral / nao-contrata |
| Fim da faixa 55–99 | 98 | 99 | 100 | nao-contrata |

---

## 2. Brown & Donaldson (Página Web de Trade — Estrutura/Comprimento)

Na página de negócios (Trade), o campo "Quantidade" exige entre 1 e 4 caracteres numéricos (0 a 9).

**Limites e testes gerados:** Focando no atributo de comprimento do dado, os casos de teste selecionam entradas com as extensões de {0, 1, 4, 5} caracteres numéricos.

### Método

``` typescript
function quantidadeTemComprimentoValido(quantidade: string): boolean {
  if (!/^\d*$/.test(quantidade)) return false;
  const comprimento = quantidade.length;
  return comprimento >= 1 && comprimento <= 4;
}
```

### Testes gerados

``` typescript
describe("quantidadeTemComprimentoValido", () => {
  it("rejeitar quantidade quando entrada está vazia com 0 caracteres, abaixo do mínimo de 1", () => {
    const resultado = quantidadeTemComprimentoValido("");

    expect(resultado).toBe(false);
  });

  it("validar quantidade quando entrada tem 1 caractere numérico, no limite inferior válido", () => {
    const resultado = quantidadeTemComprimentoValido("1");

    expect(resultado).toBe(true);
  });

  it("validar quantidade quando entrada tem 4 caracteres numéricos, no limite superior válido", () => {
    const resultado = quantidadeTemComprimentoValido("1234");

    expect(resultado).toBe(true);
  });

  it("rejeitar quantidade quando entrada tem 5 caracteres numéricos, acima do máximo de 4", () => {
    const resultado = quantidadeTemComprimentoValido("12345");

    expect(resultado).toBe(false);
  });
});
```

### Mapa de limites

| Limite | Abaixo (0) | Sobre (1) | — | Sobre (4) | Acima (5) |
|--------|------------|-----------|---|-----------|-----------|
| Comprimento | inválido | válido | … | válido | inválido |

---

## 3. Brown & Donaldson (Página Web de Trade — Limites de Valores Baseados no Contexto)

Analisando o mesmo campo de "Quantidade", mas agora focando nos limites de **valor**. O limite mínimo é sempre 1 ação, testando-se {0, 1, 2}. No entanto, o limite máximo varia com o contexto:

- **Se a ordem for Vender (Sell):** O limite superior é exatamente a quantidade atual de ações que o cliente possui (`sharesOwned`). Os testes seriam {sharesOwned-1, sharesOwned, sharesOwned+1}.
- **Se a ordem for Comprar (Buy):** O limite superior depende do saldo do usuário menos comissões, dividido pelo preço da ação (`shares = (accountBalance - commission) / sharePrice`). Os testes devem avaliar a compra de {shares-1, shares, shares+1}.

### Método

``` typescript
type TipoOrdem = "buy" | "sell";

interface ContextoTrade {
  sharesOwned: number;
  accountBalance: number;
  commission: number;
  sharePrice: number;
}

function quantidadeValidaPorContexto(
  quantidade: number,
  ordem: TipoOrdem,
  ctx: ContextoTrade
): boolean {
  if (!Number.isInteger(quantidade) || quantidade < 1) return false;

  if (ordem === "sell") {
    return quantidade <= ctx.sharesOwned;
  }

  const maxCompra = Math.floor((ctx.accountBalance - ctx.commission) / ctx.sharePrice);
  return quantidade <= maxCompra;
}
```

### Testes gerados

``` typescript
describe("quantidadeValidaPorContexto", () => {
  const ctxPadrao = { sharesOwned: 100, accountBalance: 10000, commission: 10, sharePrice: 50 };

  describe("limite mínimo (sempre 1 ação)", () => {
    it("rejeitar ordem quando quantidade é 0, abaixo do mínimo de 1 ação", () => {
      const resultado = quantidadeValidaPorContexto(0, "sell", ctxPadrao);

      expect(resultado).toBe(false);
    });

    it("validar ordem quando quantidade é 1, no limite inferior válido", () => {
      const resultado = quantidadeValidaPorContexto(1, "sell", ctxPadrao);

      expect(resultado).toBe(true);
    });

    it("validar ordem quando quantidade é 2, logo acima do limite inferior de 1 ação", () => {
      const resultado = quantidadeValidaPorContexto(2, "sell", ctxPadrao);

      expect(resultado).toBe(true);
    });
  });

  describe("limite superior — ordem Sell", () => {
    const ctx = { sharesOwned: 50, accountBalance: 10000, commission: 10, sharePrice: 50 };

    it("validar venda quando quantidade é 49 ações, um abaixo de sharesOwned", () => {
      const resultado = quantidadeValidaPorContexto(49, "sell", ctx);

      expect(resultado).toBe(true);
    });

    it("validar venda quando quantidade é 50 ações, exatamente sharesOwned", () => {
      const resultado = quantidadeValidaPorContexto(50, "sell", ctx);

      expect(resultado).toBe(true);
    });

    it("rejeitar venda quando quantidade é 51 ações, um acima de sharesOwned", () => {
      const resultado = quantidadeValidaPorContexto(51, "sell", ctx);

      expect(resultado).toBe(false);
    });
  });

  describe("limite superior — ordem Buy", () => {
    // maxCompra = floor((10000 - 10) / 50) = 199
    const ctx = { sharesOwned: 0, accountBalance: 10000, commission: 10, sharePrice: 50 };

    it("validar compra quando quantidade é 198 ações, um abaixo de maxCompra", () => {
      const resultado = quantidadeValidaPorContexto(198, "buy", ctx);

      expect(resultado).toBe(true);
    });

    it("validar compra quando quantidade é 199 ações, exatamente maxCompra", () => {
      const resultado = quantidadeValidaPorContexto(199, "buy", ctx);

      expect(resultado).toBe(true);
    });

    it("rejeitar compra quando quantidade é 200 ações, um acima de maxCompra", () => {
      const resultado = quantidadeValidaPorContexto(200, "buy", ctx);

      expect(resultado).toBe(false);
    });
  });
});
```

### Mapa de limites

| Contexto | Limite | Abaixo | Sobre | Acima |
|----------|--------|--------|-------|-------|
| Qualquer ordem | Mínimo (1 ação) | 0 | 1 | 2 |
| Sell | Máximo (`sharesOwned`) | sharesOwned - 1 | sharesOwned | sharesOwned + 1 |
| Buy | Máximo (`(balance - commission) / price`) | max - 1 | max | max + 1 |
