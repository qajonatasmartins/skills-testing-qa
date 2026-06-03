# PlantUML — biblioteca DomainStory

Fonte canónica: README do repositório https://github.com/johthor/DomainStory-PlantUML

## Include (escolhe um padrão por ficheiro)

```puml
!include <DomainStory/domainStory>
```

Requer PlantUML **≥ 1.2022.5** (stdlib). Para versão “sempre atual” do repositório:

```puml
!include https://raw.githubusercontent.com/johthor/DomainStory-PlantUML/main/domainStory.puml
```

Ou cópia local: `!include path/to/domainStory.puml`

## Mapeamento pictográfico → macros

| Papel no método | Macro típica |
|-----------------|--------------|
| Pessoa | `Person(nome)` |
| Grupo | `Group(nome)` |
| Sistema | `System(nome)` |
| Documento | `Document(nome)` |
| Pasta | `Folder(nome)` |
| Chamada | `Call(nome)` |
| Email | `Email(nome)` |
| Conversa | `Conversation(nome)` |
| Informação | `Info(nome)` |
| Fronteira / contexto | `Boundary(Nome) { ... }` |

Objeto dinâmico na atividade: `Conversation: clima` (prefixo `Tipo:`).

## Atividade

Forma geral:

```puml
activity($step, $subject, $predicate, $object[, $post][, $target] ...)
```

- Passo `_` : sequencial com auto-numeração.
- Passo `|` : paralelo (não incrementa — ver README da lib).
- Layout global: `!$Story_Layout = "left-to-right"` (ou `landscape`, `top-to-bottom`, `portrait` conforme documentação da lib).

## Tema (opcional)

```puml
@startuml
!theme bluegray
!include <DomainStory/domainStory>
...
@enduml
```

## Versões (resumo do upstream)

A matriz DomainStory ↔ PlantUML muda entre releases; consulta a tabela no README do repositório (ex.: lib empacotada desde 1.2022.5; versões mais novas da lib com PlantUML 1.2024.8+). Indica sempre ao utilizador qual include usaste e que deve renderizar com PlantUML compatível.

## Boas práticas para esta skill

1. Declarar atores e objetos **dentro** do `Boundary` quando usas objetos dinâmicos na `activity`.
2. Predicados e rótulos em **PT-BR** nas partes visíveis ao negócio.
3. Após gerar, indicar ao utilizador comando ou extensão IDE para renderizar (ex.: `plantuml arquivo.puml`).

## Erros comuns

- Esquecer o `!include` da DomainStory.
- Usar `sequenceDiagram` Mermaid em vez da entrega `.puml` desta skill.
- `System` em história “pura” sem confirmação do utilizador.
