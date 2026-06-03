# Granularidade da história

Estes níveis guiam **quantos passos**, **quais atores** e **se sistemas de TI aparecem**. O utilizador deve escolher **um** nível principal por entrega (ou duas entregas sequenciais, ex.: grossa → fina).

## Grossa

- **Objetivo:** visão ponta a ponta do processo; alinhamento rápido entre áreas.
- **Atividades:** tipicamente 4–8 passos numerados; sem despacho operacional fino.
- **Atores:** só os indispensáveis ao enredo; nomes estáveis do domínio.
- **Sistemas de TI:** só se forem o “nó” central do valor; caso contrário omitir ou agregar (“plataforma de vendas” em vez de três microserviços).
- **PlantUML:** poucos `Person`/`System`; objetos de trabalho agregados (`Document(Pedido)` em vez de cinco documentos).

## Pura

- **Objetivo:** processo de negócio com **linguagem de domínio**; foco em pessoas, papéis, regras e artefatos de negócio.
- **Sistemas de TI:** por defeito **ausentes** ou **genéricos** (“sistema interno”) só se o especialista de domínio os nomear como atores da história. Não adicionar `System(CRM)` sem o utilizador pedir neste nível.
- **Atividades:** suficientes para contar a história completa sem telas nem APIs.
- **Uso:** descoberta de linguagem ubíqua e fronteiras de contexto.

## Fina

- **Objetivo:** operação do dia a dia; handoffs claros; exceções principais.
- **Atividades:** mais passos; ramificações simples (ex.: aprovado vs recusado).
- **Objetos de trabalho:** vários `Document`/`Folder`/`Email` conforme o fluxo real.
- **Sistemas:** entram quando o processo **depende** deles; nomear como o negócio chama (não só hostname técnico).

## Digitalizada

- **Objetivo:** AS-IS ou TO-BE com **TI explícito**: aplicações, integrações, filas, canais digitais.
- **Atores:** combinação de `Person`, `Group`, `System` com rótulos que o time usa (ex.: “App PDV”, “API Faturamento”).
- **Atividades:** inclui passos disparados por sistema, batches, webhooks — sempre com verbo legível para humanos (“regista”, “envia”, “consulta”).
- **PlantUML:** `System` por serviço relevante; cuidado com explosão de caixas — agrupar com `Boundary` quando fizer sentido.

## Regra de transição

Se o utilizador pedir “refinar”, gera **nova** secção ou ficheiro com sufixo de nível no título; não sobrescrevas silenciosamente a história grossa.
