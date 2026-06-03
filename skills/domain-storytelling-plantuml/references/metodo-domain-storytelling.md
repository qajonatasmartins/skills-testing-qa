# Método Domain Storytelling (resumo operacional)

Baseado no método público descrito em https://domainstorytelling.org/. Esta skill não substitui o livro nem facilitação presencial.

## Pictografia (conceitos)

- **Ator pessoa:** quem executa trabalho com intenção.
- **Ator grupo:** equipa ou papel coletivo quando a história não precisa de indivíduos.
- **Ator sistema:** software ou hardware autónomo que participa do fluxo (níveis fina/digitalizada).
- **Objeto de trabalho:** algo que passa de mão em mão — pedido, fatura, lista, conversa, pasta.
- **Atividade:** seta numerada com **verbo** no domínio (não “clica”, salvo que o utilizador defina essa granularidade).
- **Fronteira (`Boundary` no PlantUML):** opcional; agrupa o que pertence ao mesmo contexto ou mesmo “mundo” da história.

## Estrutura típica de frase

Em prosa PT-BR, cada passo deve ser legível como:

> **[Ator]** **[verbo no domínio]** **[objeto de trabalho]** (**opcional:** para / com / em **[outro ator ou lugar]**).

Exemplo: “O **Recepcionista** **regista** a **chegada do hóspede** no **livro de estadias**.”

## Regras de qualidade

- Uma história, um fio narrativo principal; paralelismo só quando o negócio afirma simultaneidade.
- Nomes de atores e objetos: **termos do negócio**, não jargão interno de equipa sem glossário.
- Evitar “fantasma”: ator que aparece uma vez sem papel nas atividades seguintes.
- Se houver AS-IS e TO-BE, **dois blocos** com títulos explícitos.

## Checklist antes de fechar

- [ ] Cada número de passo tem sujeito e verbo claros.
- [ ] Objetos de trabalho aparecem quando são criados, transformados ou entregues.
- [ ] Linguagem consistente (um termo por conceito).
- [ ] Granularidade respeitada (ver `granularidade-niveis.md`).
- [ ] Premissas documentadas se algo foi inferido.
