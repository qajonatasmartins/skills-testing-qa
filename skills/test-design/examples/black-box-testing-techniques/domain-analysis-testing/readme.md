# Exemplos de Domain Analysis Testing

Cada exemplo abaixo mantém a descrição do cenário e acrescenta o **método sob teste** e os **testes Jest** derivados pela técnica 1x1 (on point / off point por fronteira, mantendo as demais variáveis em in point). Os testes seguem **AAA estrito**: ARRANGE → ACT (uma chamada) → ASSERT (um `expect`). Cada teste é um `it()` separado, com nome que **começa por um verbo no infinitivo** (`rejeitar`, `retornar`, `aprovar`, `validar`…) e em seguida descreve o comportamento validado e o ponto de domínio exercitado.

Abaixo um estudo de caso principal para explicar o uso dessa técnica, acompanhado de um exercício teórico.

---

## 1. O Exemplo da Admissão na Stateless University

A universidade determina a aprovação de candidatos baseada no GPA (notas da escola variando de 0 a 4.0) e em uma prova ACT (cuja nota máxima é 36). A regra de admissão gera um domínio de três equações lineares de fronteira:

- ACT <= 36.
- GPA <= 4.0.
- 10 * GPA + ACT >= 71.

Ao invés de testar exaustivamente todas as notas, a técnica 1x1 gera apenas 6 casos de teste eficientes:

- **TC1 & TC2 testam a regra `GPA <= 4.0`:**
  - TC1: GPA=4.0 (**on point**) / ACT=34 (típico) -> Resultado: Admissão.
  - TC2: GPA=4.1 (**off point**) / ACT=33 (típico) -> Resultado: Rejeição.
- **TC3 & TC4 testam a regra `ACT <= 36`:**
  - TC3: ACT=36 (**on point**) / GPA=3.7 (típico) -> Resultado: Admissão.
  - TC4: ACT=37 (**off point**) / GPA=3.8 (típico) -> Resultado: Rejeição.
- **TC5 & TC6 testam a interação `10*GPA + ACT >= 71`:**
  - TC5: GPA=3.7 e ACT=34 (**on point** em ambos) -> Resultado: Admissão.
  - TC6: GPA=3.8 e ACT=32 (**off point** da interação das duas) -> Resultado: Rejeição.

### Método

```typescript
interface Candidato {
  gpa: number;
  act: number;
}

function avaliarAdmissao(candidato: Candidato): boolean {
  const { gpa, act } = candidato;
  if (gpa > 4.0) return false;
  if (act > 36) return false;
  if (10 * gpa + act < 71) return false;
  return true;
}
```

### Testes gerados

```typescript
describe("avaliarAdmissao — Stateless University", () => {
  it("aprovar candidato quando GPA=4.0 on point e ACT=34 in point, TC1 fronteira GPA <= 4.0", () => {
    const resultado = avaliarAdmissao({ gpa: 4.0, act: 34 });

    expect(resultado).toBe(true);
  });

  it("rejeitar candidato quando GPA=4.1 off point e ACT=33 in point, TC2 fronteira GPA <= 4.0", () => {
    const resultado = avaliarAdmissao({ gpa: 4.1, act: 33 });

    expect(resultado).toBe(false);
  });

  it("aprovar candidato quando ACT=36 on point e GPA=3.7 in point, TC3 fronteira ACT <= 36", () => {
    const resultado = avaliarAdmissao({ gpa: 3.7, act: 36 });

    expect(resultado).toBe(true);
  });

  it("rejeitar candidato quando ACT=37 off point e GPA=3.8 in point, TC4 fronteira ACT <= 36", () => {
    const resultado = avaliarAdmissao({ gpa: 3.8, act: 37 });

    expect(resultado).toBe(false);
  });

  it("aprovar candidato quando GPA=3.7 e ACT=34 on point da interação 10*GPA+ACT >= 71, TC5", () => {
    const resultado = avaliarAdmissao({ gpa: 3.7, act: 34 });

    expect(resultado).toBe(true);
  });

  it("rejeitar candidato quando GPA=3.8 e ACT=32 off point da interação 10*GPA+ACT >= 71, TC6", () => {
    const resultado = avaliarAdmissao({ gpa: 3.8, act: 32 });

    expect(resultado).toBe(false);
  });
});
```

### Matriz de teste de domínio

| TC | Fronteira testada | GPA | ACT | Ponto | Esperado |
|----|-------------------|-----|-----|-------|----------|
| TC1 | GPA <= 4.0 | 4.0 (on) | 34 (in) | on/in | aprovar |
| TC2 | GPA <= 4.0 | 4.1 (off) | 33 (in) | off/in | rejeitar |
| TC3 | ACT <= 36 | 3.7 (in) | 36 (on) | in/on | aprovar |
| TC4 | ACT <= 36 | 3.8 (in) | 37 (off) | in/off | rejeitar |
| TC5 | 10*GPA+ACT >= 71 | 3.7 | 34 | on/on | aprovar |
| TC6 | 10*GPA+ACT >= 71 | 3.8 | 32 | off/off | rejeitar |

---

## 2. Exemplo Proposto das Aulas de Educação Geral

Outro exemplo é uma análise de domínio baseada nos créditos de graduação exigidos pela universidade. As regras que restringem simultaneamente as variáveis determinam que os estudantes devem cursar:

- Entre 4 a 16 horas de Ciências Sociais.
- Entre 4 a 16 horas de Ciências Físicas.
- Contudo, a soma das horas de Ciências Sociais e Ciências Físicas não pode ultrapassar 24 horas.

Podemos chegar à solução exata aplicando a técnica de **Análise de Domínio 1x1 (1x1 Domain Analysis)** ensinada para encontrar os casos de teste corretos.

Aqui está a resolução passo a passo de como a resposta é construída com base na metodologia do livro:

### Identificando as Variáveis e Restrições (Fronteiras)

Temos duas variáveis principais:

- **S:** Horas de Ciências Sociais
- **P:** Horas de Ciências Físicas

As regras criam um domínio com **5 equações lineares de fronteira (todas fechadas, ou seja, usam >= ou <=)**:

- **S >= 4** (Mínimo de Sociais)
- **S <= 16** (Máximo de Sociais)
- **P >= 4** (Mínimo de Físicas)
- **P <= 16** (Máximo de Físicas)
- **S + P <= 24** (Soma máxima permitida)

### Aplicando a Técnica 1x1

A regra 1x1 diz que, para cada condição relacional (>=, <=), devemos criar dois casos de teste: um **on point** (exatamente sobre a fronteira) e um **off point** (logo fora do domínio aceitável). Enquanto testamos os limites de uma variável, mantemos as outras variáveis em um ponto típico e seguro (*in point*).

Abaixo está a **Matriz de Teste de Domínio**:

#### Testando a Fronteira 1: Ciências Sociais >= 4

- **TC1 (*On point* / Válido):** S = 4 horas | P = 10 horas (típico) -> *Resultado: Aceito (Soma = 14).*
- **TC2 (*Off point* / Inválido):** S = 3 horas | P = 10 horas (típico) -> *Resultado: Rejeitado (Sociais abaixo do mínimo).*

#### Testando a Fronteira 2: Ciências Sociais <= 16

- **TC3 (*On point* / Válido):** S = 16 horas | P = 6 horas (típico) -> *Resultado: Aceito (Soma = 22).*
- **TC4 (*Off point* / Inválido):** S = 17 horas | P = 6 horas (típico) -> *Resultado: Rejeitado (Sociais acima do máximo).*

#### Testando a Fronteira 3: Ciências Físicas >= 4

- **TC5 (*On point* / Válido):** P = 4 horas | S = 10 horas (típico) -> *Resultado: Aceito (Soma = 14).*
- **TC6 (*Off point* / Inválido):** P = 3 horas | S = 10 horas (típico) -> *Resultado: Rejeitado (Físicas abaixo do mínimo).*

#### Testando a Fronteira 4: Ciências Físicas <= 16

- **TC7 (*On point* / Válido):** P = 16 horas | S = 6 horas (típico) -> *Resultado: Aceito (Soma = 22).*
- **TC8 (*Off point* / Inválido):** P = 17 horas | S = 6 horas (típico) -> *Resultado: Rejeitado (Físicas acima do máximo).*

#### Testando a Fronteira 5: Interação (Soma <= 24)

Para testar essa interação, escolhemos valores válidos individualmente para S e P, mas que somados atinjam o limite exato (*on*) ou o ultrapassem por 1 (*off*).

- **TC9 (*On point* / Válido):** S = 12 horas e P = 12 horas -> *Resultado: Aceito (Soma exata = 24).*
- **TC10 (*Off point* / Inválido):** S = 13 horas e P = 12 horas -> *Resultado: Rejeitado (Soma = 25, viola o limite de horas combinadas).*

**A Resposta Final:**

Resulta em **10 Casos de Teste (TC1 ao TC10)**. Testando essas 10 combinações específicas, garante-se uma cobertura otimizada de todas as regras de admissão e a detecção de defeitos em limites lógicos sem precisar testar exaustivamente todas as combinações matemáticas possíveis.

### Método

```typescript
interface CreditosGraduacao {
  horasSociais: number;
  horasFisicas: number;
}

function validarCreditosGraduacao(creditos: CreditosGraduacao): boolean {
  const { horasSociais: s, horasFisicas: p } = creditos;
  if (s < 4 || s > 16) return false;
  if (p < 4 || p > 16) return false;
  if (s + p > 24) return false;
  return true;
}
```

### Testes gerados

```typescript
describe("validarCreditosGraduacao — Educação Geral", () => {
  it("aceitar créditos quando S=4 on point e P=10 in point, TC1 fronteira S >= 4", () => {
    const resultado = validarCreditosGraduacao({ horasSociais: 4, horasFisicas: 10 });

    expect(resultado).toBe(true);
  });

  it("rejeitar créditos quando S=3 off point e P=10 in point, TC2 fronteira S >= 4", () => {
    const resultado = validarCreditosGraduacao({ horasSociais: 3, horasFisicas: 10 });

    expect(resultado).toBe(false);
  });

  it("aceitar créditos quando S=16 on point e P=6 in point, TC3 fronteira S <= 16", () => {
    const resultado = validarCreditosGraduacao({ horasSociais: 16, horasFisicas: 6 });

    expect(resultado).toBe(true);
  });

  it("rejeitar créditos quando S=17 off point e P=6 in point, TC4 fronteira S <= 16", () => {
    const resultado = validarCreditosGraduacao({ horasSociais: 17, horasFisicas: 6 });

    expect(resultado).toBe(false);
  });

  it("aceitar créditos quando P=4 on point e S=10 in point, TC5 fronteira P >= 4", () => {
    const resultado = validarCreditosGraduacao({ horasSociais: 10, horasFisicas: 4 });

    expect(resultado).toBe(true);
  });

  it("rejeitar créditos quando P=3 off point e S=10 in point, TC6 fronteira P >= 4", () => {
    const resultado = validarCreditosGraduacao({ horasSociais: 10, horasFisicas: 3 });

    expect(resultado).toBe(false);
  });

  it("aceitar créditos quando P=16 on point e S=6 in point, TC7 fronteira P <= 16", () => {
    const resultado = validarCreditosGraduacao({ horasSociais: 6, horasFisicas: 16 });

    expect(resultado).toBe(true);
  });

  it("rejeitar créditos quando P=17 off point e S=6 in point, TC8 fronteira P <= 16", () => {
    const resultado = validarCreditosGraduacao({ horasSociais: 6, horasFisicas: 17 });

    expect(resultado).toBe(false);
  });

  it("aceitar créditos quando S=12 e P=12 on point da interação S+P <= 24, TC9", () => {
    const resultado = validarCreditosGraduacao({ horasSociais: 12, horasFisicas: 12 });

    expect(resultado).toBe(true);
  });

  it("rejeitar créditos quando S=13 e P=12 off point da interação S+P <= 24, TC10", () => {
    const resultado = validarCreditosGraduacao({ horasSociais: 13, horasFisicas: 12 });

    expect(resultado).toBe(false);
  });
});
```

### Matriz de teste de domínio

| TC | Fronteira | S | P | Ponto S | Ponto P | Esperado |
|----|-----------|---|---|---------|---------|----------|
| TC1 | S >= 4 | 4 | 10 | on | in | aceitar |
| TC2 | S >= 4 | 3 | 10 | off | in | rejeitar |
| TC3 | S <= 16 | 16 | 6 | on | in | aceitar |
| TC4 | S <= 16 | 17 | 6 | off | in | rejeitar |
| TC5 | P >= 4 | 10 | 4 | in | on | aceitar |
| TC6 | P >= 4 | 10 | 3 | in | off | rejeitar |
| TC7 | P <= 16 | 6 | 16 | in | on | aceitar |
| TC8 | P <= 16 | 6 | 17 | in | off | rejeitar |
| TC9 | S+P <= 24 | 12 | 12 | on | on | aceitar |
| TC10 | S+P <= 24 | 13 | 12 | off | on | rejeitar |
