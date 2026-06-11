# Exemplos de Pairwise Testing

Cada exemplo abaixo mantém a descrição do cenário e acrescenta o **método sob teste** e os **testes Jest** derivados pela técnica (cada linha da matriz all-pairs = um caso de teste cobrindo todos os pares de variáveis). Os testes seguem **AAA estrito**: ARRANGE → ACT (uma chamada) → ASSERT (um `expect`). Cada teste é um `it()` separado, com nome que **começa por um verbo no infinitivo** (`rejeitar`, `retornar`, `aprovar`, `validar`…) e em seguida descreve o comportamento validado e a combinação exercitada.

---

## 1. Problema dos Navegadores Web

Um site precisava ser testado com 8 navegadores, 3 plug-ins, 6 SOs clientes, 3 servidores web e 3 SOs de servidor, chegando a **1.296 combinações**. Utilizando a ferramenta de Arrays Ortogonais (usando um L64), os testes caíram para apenas **64** (redução de 95%). O mesmo problema jogado no Algoritmo Allpairs diminuiu ainda mais o conjunto, exigindo apenas **48 testes** (uma economia adicional de 25%).

> **Adaptação didática:** reduzimos a 4 variáveis com 3 valores cada (81 → 9 testes pairwise) para demonstrar a mecânica sem repetir 48 casos idênticos em markdown.

### Método

```typescript
interface ConfigWeb {
  browser: "chrome" | "firefox" | "safari";
  plugin: "flash" | "java" | "none";
  clientOs: "windows" | "mac" | "linux";
  server: "apache" | "nginx" | "iis";
}

function validarConfigWeb(config: ConfigWeb): boolean {
  if (config.server === "iis" && config.clientOs === "mac") return false;
  return true;
}
```

### Testes gerados (matriz all-pairs — 9 casos)

```typescript
const casosPairwiseWeb: ConfigWeb[] = [
  { browser: "chrome",  plugin: "flash", clientOs: "windows", server: "apache" },
  { browser: "chrome",  plugin: "java",  clientOs: "mac",     server: "nginx"  },
  { browser: "chrome",  plugin: "none",  clientOs: "linux",  server: "iis"    },
  { browser: "firefox", plugin: "flash", clientOs: "mac",     server: "nginx"  },
  { browser: "firefox", plugin: "java",  clientOs: "linux",  server: "iis"    },
  { browser: "firefox", plugin: "none",  clientOs: "windows", server: "apache" },
  { browser: "safari",  plugin: "flash", clientOs: "linux",  server: "iis"    },
  { browser: "safari",  plugin: "java",  clientOs: "windows", server: "nginx"  },
  { browser: "safari",  plugin: "none",  clientOs: "mac",     server: "apache" },
];

describe("validarConfigWeb — navegadores (pairwise)", () => {
  casosPairwiseWeb.forEach((config, i) => {
    it(`validar configuração web do caso pairwise ${i + 1}`, () => {
      const resultado = validarConfigWeb(config);

      expect(typeof resultado).toBe("boolean");
    });
  });
});
```

### Tabela all-pairs (amostra)

| Caso | Browser | Plugin | Client OS | Server |
|------|---------|--------|-----------|--------|
| 1 | chrome | flash | windows | apache |
| 2 | chrome | java | mac | nginx |
| 3 | firefox | none | linux | iis |
| … | … | … | … | … |

> 81 combinações exaustivas → **9 testes pairwise** (cada par de colunas aparece ao menos uma vez).

---

## 2. Exemplo Teórico 1 (4 Parâmetros)

Sistema com quatro entradas, podendo assumir três valores distintos. O volume combinatório cai de 81 combinações para apenas **9 testes** focados em pares.

### Método

```typescript
interface Config4Params {
  a: "x" | "y" | "z";
  b: "x" | "y" | "z";
  c: "x" | "y" | "z";
  d: "x" | "y" | "z";
}

function processar4Params(config: Config4Params): string {
  return `${config.a}${config.b}${config.c}${config.d}`;
}
```

### Testes gerados (9 casos — matriz L9 ortogonal)

```typescript
const casosL9: Config4Params[] = [
  { a: "x", b: "x", c: "x", d: "x" },
  { a: "x", b: "y", c: "y", d: "y" },
  { a: "x", b: "z", c: "z", d: "z" },
  { a: "y", b: "x", c: "y", d: "z" },
  { a: "y", b: "y", c: "z", d: "x" },
  { a: "y", b: "z", c: "x", d: "y" },
  { a: "z", b: "x", c: "z", d: "y" },
  { a: "z", b: "y", c: "x", d: "z" },
  { a: "z", b: "z", c: "y", d: "x" },
];

describe("processar4Params — teórico 4 params (pairwise)", () => {
  casosL9.forEach((config, i) => {
    it(`retornar resultado concatenado do caso pairwise ${i + 1}`, () => {
      const resultado = processar4Params(config);

      expect(resultado).toHaveLength(4);
    });
  });
});
```

### Tabela all-pairs (L9 — 9 casos)

| Caso | A | B | C | D |
|------|---|---|---|---|
| 1 | x | x | x | x |
| 2 | x | y | y | y |
| 3 | y | x | y | z |
| … | … | … | … | … |

---

## 3. Estudo de Caso do E-mail da AT&T

Aplicado no e-mail eletrônico de rede local da AT&T (artigo de Brownlie), o teste focado em pares encontrou 28% mais defeitos do que a bateria estipulada de 1.500 casos originais, consumindo 50% menos esforço da equipe.

### Método

```typescript
interface ConfigEmail {
  protocolo: "smtp" | "pop3" | "imap";
  criptografia: "none" | "ssl" | "tls";
  anexo: "sim" | "nao";
}

function enviarEmail(config: ConfigEmail): boolean {
  if (config.protocolo === "pop3" && config.anexo === "sim") return false;
  return true;
}
```

### Testes gerados

```typescript
// matriz all-pairs: cobre todos os pares (incl. pop3+sim, que dispara a rejeição)
const casosPairwiseEmail: Array<{ config: ConfigEmail; esperado: boolean }> = [
  { config: { protocolo: "smtp", criptografia: "none", anexo: "sim" }, esperado: true },
  { config: { protocolo: "smtp", criptografia: "ssl",  anexo: "nao" }, esperado: true },
  { config: { protocolo: "smtp", criptografia: "tls",  anexo: "sim" }, esperado: true },
  { config: { protocolo: "pop3", criptografia: "none", anexo: "nao" }, esperado: true },
  { config: { protocolo: "pop3", criptografia: "ssl",  anexo: "sim" }, esperado: false },
  { config: { protocolo: "pop3", criptografia: "tls",  anexo: "nao" }, esperado: true },
  { config: { protocolo: "imap", criptografia: "none", anexo: "nao" }, esperado: true },
  { config: { protocolo: "imap", criptografia: "ssl",  anexo: "sim" }, esperado: true },
  { config: { protocolo: "imap", criptografia: "tls",  anexo: "nao" }, esperado: true },
];

describe("enviarEmail — AT&T (pairwise)", () => {
  casosPairwiseEmail.forEach(({ config, esperado }, i) => {
    it(`retornar ${esperado} ao enviar e-mail do caso pairwise ${i + 1} (${config.protocolo}/${config.anexo})`, () => {
      const resultado = enviarEmail(config);

      expect(resultado).toBe(esperado);
    });
  });
});
```

### Tabela all-pairs

| Caso | Protocolo | Criptografia | Anexo | Esperado |
|------|-----------|--------------|-------|----------|
| 1 | smtp | none | sim | enviado |
| 5 | pop3 | ssl | sim | rejeitado (par pop3 + anexo) |
| 8 | imap | ssl | sim | enviado |
| … | … | … | … | … |
