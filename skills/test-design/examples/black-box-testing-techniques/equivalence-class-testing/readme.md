# Exemplos de Equivalence Class Testing

Cada exemplo abaixo mantém a descrição do cenário e acrescenta o **método sob teste** e os **testes Jest** derivados pela técnica (um representante por classe de equivalência válida ou inválida). Os testes seguem **AAA estrito**: ARRANGE → ACT (uma chamada) → ASSERT (um `expect`). Cada teste é um `it()` separado, com nome que **começa por um verbo no infinitivo** (`rejeitar`, `retornar`, `aprovar`, `validar`…) e em seguida descreve o comportamento validado e a classe exercitada.

---

## 1. Goofy Mortgage Company (GMC)

A GMC é uma empresa fictícia de hipotecas que o autor usa para mostrar como particionar diferentes categorias de dados:

- **Intervalo Contínuo (*Continuous range*):** A empresa só aceita dar hipotecas para pessoas com renda mensal entre $1.000 e $83.333.
  - A classe válida abrange qualquer valor dentro desse intervalo (como $1.342).
  - As classes inválidas são os valores abaixo do mínimo (ex: $123) e os valores acima do máximo (ex: $90.000).
- **Valores Discretos (*Discrete values*):** A GMC aprova hipotecas para um número inteiro de 1 a 5 casas.
  - A classe válida requer um número inteiro (ex: 2).
  - As classes inválidas incluem números negativos, zeros (ex: -2 ou 0), números acima do limite (ex: 8) ou números fracionados/decimais (ex: 2,5), que também não fariam sentido.
- **Seleção Única (*Single selection*):** A empresa só atende clientes do tipo "Pessoa" (Person).
  - Logo, a classe de valores válidos só aceita "Pessoa"
  - Qualquer outra entidade jurídica, como "Corporação", "Truste" ou "Parceria", formam as classes de equivalência inválidas.
- **Seleção Múltipla (*Multiple selection*):** Eles aprovam imóveis dos tipos "Condomínio", "Sobrado" (*Townhouse*) ou "Casa Unifamiliar".
  - A classe inválida envolve a inserção de qualquer imóvel que não esteja nessa lista, como "Duplex", "Casa móvel" ou "Casa na árvore".

### Método

```typescript
type TipoCliente = "person" | "corporation" | "trust" | "partnership";
type TipoImovel = "condominio" | "townhouse" | "casa-unifamiliar";

interface PedidoHipoteca {
  rendaMensal: number;
  numCasas: number;
  tipoCliente: TipoCliente;
  tiposImovel: string[];
}

function validarPedidoHipoteca(pedido: PedidoHipoteca): boolean {
  const rendaValida = pedido.rendaMensal >= 1000 && pedido.rendaMensal <= 83333;
  const casasValidas =
    Number.isInteger(pedido.numCasas) && pedido.numCasas >= 1 && pedido.numCasas <= 5;
  const clienteValido = pedido.tipoCliente === "person";
  const tiposPermitidos: TipoImovel[] = ["condominio", "townhouse", "casa-unifamiliar"];
  const imoveisValidos =
    pedido.tiposImovel.length > 0 &&
    pedido.tiposImovel.every((t) => tiposPermitidos.includes(t as TipoImovel));

  return rendaValida && casasValidas && clienteValido && imoveisValidos;
}
```

### Testes gerados

```typescript
const pedidoValidoBase = {
  rendaMensal: 5000,
  numCasas: 2,
  tipoCliente: "person" as const,
  tiposImovel: ["condominio"],
};

describe("validarPedidoHipoteca — GMC", () => {
  describe("intervalo contínuo (renda mensal)", () => {
    it("aprovar pedido quando renda é $1.342, representante da classe válida de renda", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, rendaMensal: 1342 });

      expect(resultado).toBe(true);
    });

    it("rejeitar pedido quando renda é $123, representante da classe inválida abaixo do mínimo", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, rendaMensal: 123 });

      expect(resultado).toBe(false);
    });

    it("rejeitar pedido quando renda é $90.000, representante da classe inválida acima do máximo", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, rendaMensal: 90000 });

      expect(resultado).toBe(false);
    });
  });

  describe("valores discretos (número de casas)", () => {
    it("aprovar pedido quando numCasas é 2, representante da classe válida de casas inteiras", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, numCasas: 2 });

      expect(resultado).toBe(true);
    });

    it("rejeitar pedido quando numCasas é -2, representante da classe inválida negativa", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, numCasas: -2 });

      expect(resultado).toBe(false);
    });

    it("rejeitar pedido quando numCasas é 0, representante da classe inválida zero", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, numCasas: 0 });

      expect(resultado).toBe(false);
    });

    it("rejeitar pedido quando numCasas é 8, representante da classe inválida acima do limite", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, numCasas: 8 });

      expect(resultado).toBe(false);
    });

    it("rejeitar pedido quando numCasas é 2.5, representante da classe inválida fracionária", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, numCasas: 2.5 });

      expect(resultado).toBe(false);
    });
  });

  describe("seleção única (tipo de cliente)", () => {
    it("aprovar pedido quando tipoCliente é person, única classe válida de cliente", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, tipoCliente: "person" });

      expect(resultado).toBe(true);
    });

    it("rejeitar pedido quando tipoCliente é corporation, representante da classe inválida de pessoa jurídica", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, tipoCliente: "corporation" });

      expect(resultado).toBe(false);
    });

    it("rejeitar pedido quando tipoCliente é trust, representante da classe inválida de truste", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, tipoCliente: "trust" });

      expect(resultado).toBe(false);
    });

    it("rejeitar pedido quando tipoCliente é partnership, representante da classe inválida de parceria", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, tipoCliente: "partnership" });

      expect(resultado).toBe(false);
    });
  });

  describe("seleção múltipla (tipos de imóvel)", () => {
    it("aprovar pedido quando tiposImovel contém townhouse, representante da classe válida de imóvel", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, tiposImovel: ["townhouse"] });

      expect(resultado).toBe(true);
    });

    it("rejeitar pedido quando tiposImovel contém duplex, representante da classe inválida de imóvel não permitido", () => {
      const resultado = validarPedidoHipoteca({ ...pedidoValidoBase, tiposImovel: ["duplex"] });

      expect(resultado).toBe(false);
    });
  });
});
```

### Mapa de classes de equivalência

| Variável | Classe | Representante | Esperado |
|----------|--------|---------------|----------|
| Renda mensal | Válida ($1.000–$83.333) | $1.342 | aprovar |
| Renda mensal | Inválida (abaixo) | $123 | rejeitar |
| Renda mensal | Inválida (acima) | $90.000 | rejeitar |
| Nº de casas | Válida (1–5 inteiros) | 2 | aprovar |
| Nº de casas | Inválida (negativo) | -2 | rejeitar |
| Nº de casas | Inválida (zero) | 0 | rejeitar |
| Nº de casas | Inválida (acima) | 8 | rejeitar |
| Nº de casas | Inválida (fracionário) | 2.5 | rejeitar |
| Tipo cliente | Válida | person | aprovar |
| Tipo cliente | Inválida | corporation / trust / partnership | rejeitar |
| Tipo imóvel | Válida | townhouse | aprovar |
| Tipo imóvel | Inválida | duplex | rejeitar |

---

## 2. Corretora Fictícia Brown & Donaldson (B&D)

Página de compras e vendas de ações da corretora (*Trade Web page*) para demonstrar a técnica em elementos de interface de usuário (GUI):

- **Tipo de Ordem (Botões de rádio / *Radio Buttons*):** O usuário deve escolher entre "Comprar" (*Buy*) ou "Vender" (*Sell*). Como é um botão de seleção fechado, o autor destaca que o desenvolvedor já ajudou o testador: não há como digitar valores da classe inválida (como "Trocar" ou "Pular"). Logo, os testes ficam restritos à classe válida (Comprar e Vender).
- **Quantidade de Ações:** O campo de quantidade aceita valores numéricos entre 1 e 9999 (1 a 4 dígitos).
  - A classe válida testa números dentro do intervalo (ex: 1, 22, 4444).
  - Para classes inválidas, devem ser testados tanto os limites numéricos incorretos (ex: -42, 0, 12345) quanto dados textuais que quebram a validação estrutural do campo (ex: letras ou caracteres como "#$@").
- **Símbolo da Ação (*Ticker Symbol*):** O usuário deve inserir o código da ação (ex: AA, AABC).
  - A classe de dados válida abrange qualquer código pertencente à lista da bolsa de valores.
  - A classe inválida seria qualquer combinação de caracteres não cadastrada no sistema.

### Método

```typescript
type TipoOrdem = "buy" | "sell";

interface OrdemTrade {
  tipoOrdem: TipoOrdem;
  quantidade: string;
  simbolo: string;
}

const SIMBOLOS_VALIDOS = new Set(["AA", "AAPL", "GOOG", "MSFT"]);

function validarOrdemTrade(ordem: OrdemTrade): boolean {
  const ordemValida = ordem.tipoOrdem === "buy" || ordem.tipoOrdem === "sell";
  if (!ordemValida) return false;

  if (!/^\d+$/.test(ordem.quantidade)) return false;
  const qtd = Number(ordem.quantidade);
  const quantidadeValida = qtd >= 1 && qtd <= 9999;

  const simboloValido = SIMBOLOS_VALIDOS.has(ordem.simbolo.toUpperCase());

  return quantidadeValida && simboloValido;
}
```

### Testes gerados

```typescript
describe("validarOrdemTrade — B&D", () => {
  describe("tipo de ordem (radio button — só classes válidas)", () => {
    it("validar ordem quando tipoOrdem é buy, representante da classe válida de compra", () => {
      const resultado = validarOrdemTrade({ tipoOrdem: "buy", quantidade: "22", simbolo: "AA" });

      expect(resultado).toBe(true);
    });

    it("validar ordem quando tipoOrdem é sell, representante da classe válida de venda", () => {
      const resultado = validarOrdemTrade({ tipoOrdem: "sell", quantidade: "22", simbolo: "AA" });

      expect(resultado).toBe(true);
    });
  });

  describe("quantidade de ações", () => {
    it("validar ordem quando quantidade é 4444, representante da classe válida numérica", () => {
      const resultado = validarOrdemTrade({ tipoOrdem: "buy", quantidade: "4444", simbolo: "AA" });

      expect(resultado).toBe(true);
    });

    it("rejeitar ordem quando quantidade é -42, representante da classe inválida negativa", () => {
      const resultado = validarOrdemTrade({ tipoOrdem: "buy", quantidade: "-42", simbolo: "AA" });

      expect(resultado).toBe(false);
    });

    it("rejeitar ordem quando quantidade é 0, representante da classe inválida zero", () => {
      const resultado = validarOrdemTrade({ tipoOrdem: "buy", quantidade: "0", simbolo: "AA" });

      expect(resultado).toBe(false);
    });

    it("rejeitar ordem quando quantidade é 12345, representante da classe inválida acima do máximo", () => {
      const resultado = validarOrdemTrade({ tipoOrdem: "buy", quantidade: "12345", simbolo: "AA" });

      expect(resultado).toBe(false);
    });

    it("rejeitar ordem quando quantidade é #$@, representante da classe inválida não numérica", () => {
      const resultado = validarOrdemTrade({ tipoOrdem: "buy", quantidade: "#$@", simbolo: "AA" });

      expect(resultado).toBe(false);
    });
  });

  describe("símbolo da ação (ticker)", () => {
    it("validar ordem quando simbolo é AA, representante da classe válida cadastrada", () => {
      const resultado = validarOrdemTrade({ tipoOrdem: "buy", quantidade: "1", simbolo: "AA" });

      expect(resultado).toBe(true);
    });

    it("rejeitar ordem quando simbolo é ZZZZ, representante da classe inválida não cadastrada", () => {
      const resultado = validarOrdemTrade({ tipoOrdem: "buy", quantidade: "1", simbolo: "ZZZZ" });

      expect(resultado).toBe(false);
    });
  });
});
```

### Mapa de classes de equivalência

| Campo | Classe | Representante | Esperado |
|-------|--------|---------------|----------|
| Tipo ordem | Válida (buy) | buy | validar |
| Tipo ordem | Válida (sell) | sell | validar |
| Quantidade | Válida (1–9999) | 4444 | validar |
| Quantidade | Inválida (negativa) | -42 | rejeitar |
| Quantidade | Inválida (zero) | 0 | rejeitar |
| Quantidade | Inválida (acima) | 12345 | rejeitar |
| Quantidade | Inválida (texto) | #$@ | rejeitar |
| Símbolo | Válida (cadastrado) | AA | validar |
| Símbolo | Inválida (não cadastrado) | ZZZZ | rejeitar |
