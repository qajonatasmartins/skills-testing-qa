# Template de saída (obrigatório)

O relatório final deve seguir esta estrutura em Markdown.

## Título

`# Domain Story — <nome curto do processo>`

## Metadados

- **Granularidade:** grossa | pura | fina | digitalizada
- **Tipo:** AS-IS | TO-BE | exploração
- **Data / autores** (se o utilizador forneceu)

## História narrada (PT-BR)

Lista numerada; cada linha = uma frase-canção do método.

## Premissas e dúvidas em aberto

Bullets; vazio se não houver.

## PlantUML

Quando o entregável for mantido no repositório, grava também um ficheiro **`.puml`** com o mesmo conteúdo do bloco abaixo (facilita pré-visualização na IDE com a extensão PlantUML).

Código completo:

```puml
@startuml
...
@enduml
```

## Como renderizar

- Indicar comando ou extensão IDE.
- Mencionar versão mínima PlantUML e qual `!include` foi usado (`<DomainStory/domainStory>` vs URL raw vs path local).

## Checklist de qualidade

Copiar bullets checkáveis de `metodo-domain-storytelling.md` (secção Checklist) e marcar `[x]` / `[ ]` conforme aplicável.
