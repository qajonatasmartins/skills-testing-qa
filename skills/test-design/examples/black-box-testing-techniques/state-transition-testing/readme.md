# Exemplos de State-Transition Testing

Cada exemplo abaixo mantém a descrição do cenário e acrescenta o **método sob teste** (FSM em TypeScript) e os **testes Jest** derivados pela técnica (transições válidas e sneak paths — eventos inválidos rejeitados). Os testes seguem **AAA estrito**: ARRANGE → ACT (uma chamada) → ASSERT (um `expect`). Cada teste é um `it()` separado, com nome que **começa por um verbo no infinitivo** (`rejeitar`, `transitar`, `retornar`, `validar`…) e em seguida descreve o comportamento validado e a transição exercitada.

Abaixo exemplos que ilustram testes e máquinas de estado de transição:

---

## 1. Catraca/Roleta (Turnstile)

Um sistema simples com os estados "Travado" (Locked) e "Destravado" (Unlocked), ativados pelos eventos de inserir moeda ou tentar passar.

### Método

```typescript
type EstadoCatraca = "locked" | "unlocked";
type EventoCatraca = "coin" | "pass";

function transitarCatraca(estado: EstadoCatraca, evento: EventoCatraca): EstadoCatraca | null {
  if (estado === "locked" && evento === "coin") return "unlocked";
  if (estado === "unlocked" && evento === "pass") return "locked";
  return null;
}
```

### Testes gerados

```typescript
describe("transitarCatraca", () => {
  it("transitar para unlocked quando locked recebe coin, transição válida", () => {
    const resultado = transitarCatraca("locked", "coin");

    expect(resultado).toBe("unlocked");
  });

  it("transitar para locked quando unlocked recebe pass, transição válida", () => {
    const resultado = transitarCatraca("unlocked", "pass");

    expect(resultado).toBe("locked");
  });

  it("rejeitar transição quando locked recebe pass, sneak path inválido", () => {
    const resultado = transitarCatraca("locked", "pass");

    expect(resultado).toBeNull();
  });

  it("rejeitar transição quando unlocked recebe coin, sneak path inválido", () => {
    const resultado = transitarCatraca("unlocked", "coin");

    expect(resultado).toBeNull();
  });
});
```

### Tabela de transição de estados

| Estado atual | Evento | Próximo estado | Válido? |
|--------------|--------|----------------|---------|
| locked | coin | unlocked | Sim |
| unlocked | pass | locked | Sim |
| locked | pass | — | Não (sneak) |
| unlocked | coin | — | Não (sneak) |

---

## 2. Sistema de Reservas de Companhia Aérea

Uma reserva tramitando pelos estados "Feita", "Paga", "Emitida" (Ticketed), "Usada", "Cancelada".

### Método

```typescript
type EstadoReserva = "made" | "paid" | "ticketed" | "used" | "cancelled";
type EventoReserva = "payMoney" | "payTimerExpires" | "issueTicket" | "useTicket" | "cancel";

function transitarReserva(estado: EstadoReserva, evento: EventoReserva): EstadoReserva | null {
  if (estado === "made" && evento === "payMoney") return "paid";
  if (estado === "made" && evento === "payTimerExpires") return "cancelled";
  if (estado === "paid" && evento === "issueTicket") return "ticketed";
  if (estado === "ticketed" && evento === "useTicket") return "used";
  if (estado === "made" && evento === "cancel") return "cancelled";
  return null;
}
```

### Testes gerados

```typescript
describe("transitarReserva", () => {
  it("transitar para paid quando made recebe payMoney, transição válida", () => {
    const resultado = transitarReserva("made", "payMoney");

    expect(resultado).toBe("paid");
  });

  it("transitar para cancelled quando made recebe payTimerExpires, transição válida", () => {
    const resultado = transitarReserva("made", "payTimerExpires");

    expect(resultado).toBe("cancelled");
  });

  it("transitar para ticketed quando paid recebe issueTicket, transição válida", () => {
    const resultado = transitarReserva("paid", "issueTicket");

    expect(resultado).toBe("ticketed");
  });

  it("transitar para used quando ticketed recebe useTicket, transição válida", () => {
    const resultado = transitarReserva("ticketed", "useTicket");

    expect(resultado).toBe("used");
  });

  it("rejeitar transição quando made recebe issueTicket, sneak path inválido", () => {
    const resultado = transitarReserva("made", "issueTicket");

    expect(resultado).toBeNull();
  });

  it("rejeitar transição quando cancelled recebe payMoney, sneak path inválido", () => {
    const resultado = transitarReserva("cancelled", "payMoney");

    expect(resultado).toBeNull();
  });
});
```

### Tabela de transição de estados

| Estado | Evento | Próximo | Válido? |
|--------|--------|---------|---------|
| made | payMoney | paid | Sim |
| made | payTimerExpires | cancelled | Sim |
| paid | issueTicket | ticketed | Sim |
| ticketed | useTicket | used | Sim |
| made | issueTicket | — | Não |
| cancelled | payMoney | — | Não |

---

## 3. Estrutura de Dados Pilha (Stack)

Estados Inicial, Vazia, Contendo dados, Cheia e Final; e suas interações com as funções de criar, adicionar, deletar e destruir.

### Método

```typescript
type EstadoPilha = "initial" | "empty" | "holding" | "full" | "final";
type EventoPilha = "create" | "push" | "pop" | "destroy";

interface ContextoPilha {
  count: number;
  capacity: number;
}

function transitarPilha(
  estado: EstadoPilha,
  evento: EventoPilha,
  ctx: ContextoPilha
): EstadoPilha | null {
  if (estado === "initial" && evento === "create") return "empty";
  if (estado === "empty" && evento === "push") return ctx.count + 1 >= ctx.capacity ? "full" : "holding";
  if (estado === "holding" && evento === "push") return ctx.count + 1 >= ctx.capacity ? "full" : "holding";
  if (estado === "holding" && evento === "pop") return ctx.count - 1 <= 0 ? "empty" : "holding";
  if (estado === "full" && evento === "push") return null;
  if ((estado === "empty" || estado === "holding" || estado === "full") && evento === "destroy") return "final";
  return null;
}
```

### Testes gerados

```typescript
describe("transitarPilha", () => {
  it("transitar para empty quando initial recebe create, transição válida", () => {
    const resultado = transitarPilha("initial", "create", { count: 0, capacity: 3 });

    expect(resultado).toBe("empty");
  });

  it("transitar para holding quando empty recebe push, transição válida", () => {
    const resultado = transitarPilha("empty", "push", { count: 0, capacity: 3 });

    expect(resultado).toBe("holding");
  });

  it("transitar para full quando holding recebe push atingindo capacity, transição válida", () => {
    const resultado = transitarPilha("holding", "push", { count: 2, capacity: 3 });

    expect(resultado).toBe("full");
  });

  it("rejeitar transição quando full recebe push, sneak path inválido", () => {
    const resultado = transitarPilha("full", "push", { count: 3, capacity: 3 });

    expect(resultado).toBeNull();
  });
});
```

### Tabela de transição de estados

| Estado | Evento | Próximo | Válido? |
|--------|--------|---------|---------|
| initial | create | empty | Sim |
| empty | push | holding | Sim |
| holding | push (cap) | full | Sim |
| full | push | — | Não |
| * | destroy | final | Sim |
