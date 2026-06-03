# CoT Multimodal (Multimodal Chain-of-Thought)

## O que é
Extensão do Chain-of-Thought para cenários com múltiplas modalidades de entrada,
combinando texto e imagem (ou outros formatos) no processo de raciocínio.
O modelo analisa informações visuais e textuais em conjunto, gerando passos
intermediários de raciocínio que incorporam ambas as fontes. A abordagem
típica é em dois estágios: primeiro gerar o raciocínio (rationale) com base
nas entradas multimodais, depois gerar a resposta usando o raciocínio.
Melhora significativamente a precisão em tarefas que dependem de interpretação visual.

## Quando usar
- Problemas que envolvem gráficos, diagramas ou tabelas visuais
- Análise de screenshots de interfaces (bugs visuais, UX)
- Interpretação de fluxogramas ou diagramas de arquitetura
- Questões científicas com figuras ou diagramas
- Acessibilidade: descrever e raciocinar sobre conteúdo visual
- QA sobre documentos que misturam texto e imagens

## Quando NÃO usar
- Tarefas puramente textuais (use CoT padrão)
- Quando o modelo não suporta entrada multimodal
- Imagens de baixa qualidade onde informação visual é ilegível
- Tarefas onde a imagem é decorativa e não contém informação relevante

## Template / Estrutura

### Estágio 1 — Raciocínio multimodal

```
Analise a imagem e o texto abaixo. Descreva o que você observa na imagem
e como isso se relaciona com a pergunta.

Texto: {{ contexto_textual }}
Imagem: {{ imagem_anexada }}

Raciocínio (descreva o que vê e como isso ajuda a responder):
```

### Estágio 2 — Resposta baseada no raciocínio

```
Com base no raciocínio abaixo, responda à pergunta.

Raciocínio: {{ raciocínio_gerado_estágio_1 }}

Pergunta: {{ pergunta }}
Resposta:
```

### Template unificado

```
Observe a imagem e leia o contexto abaixo.

Contexto: {{ texto }}
Imagem: {{ imagem }}

Pergunta: {{ pergunta }}

Primeiro, descreva o que você observa na imagem que é relevante para a pergunta.
Depois, raciocine passo a passo combinando informações visuais e textuais.
Por fim, apresente sua resposta.
```

## Exemplo prático

### Exemplo 1 — Análise de gráfico

**Prompt:**
```
Observe o gráfico de vendas mensais (imagem anexa) e responda.

Contexto: O gráfico mostra vendas de Jan a Dez/2024 para 3 produtos (A, B, C).

Primeiro, descreva as tendências que observa no gráfico.
Depois, responda: Qual produto teve o maior crescimento percentual
entre o primeiro e o segundo semestre?

Passo a passo:
```

**Resposta esperada:**
```
Observações do gráfico:
- Produto A: estável em ~R$ 50k/mês o ano todo
- Produto B: começa em ~R$ 20k em Jan, sobe para ~R$ 80k em Dez
- Produto C: começa em ~R$ 40k, cai para ~R$ 30k, depois volta a ~R$ 45k

Raciocínio:
- Produto A: média 1º sem ≈ 50k, 2º sem ≈ 50k → crescimento ≈ 0%
- Produto B: média 1º sem ≈ 30k, 2º sem ≈ 65k → crescimento ≈ 117%
- Produto C: média 1º sem ≈ 35k, 2º sem ≈ 40k → crescimento ≈ 14%

Resposta: Produto B, com crescimento de aproximadamente 117%.
```

### Exemplo 2 — Bug visual em interface

**Prompt:**
```
Analise o screenshot da tela de login (imagem anexa).

Contexto: O usuário reportou que "o botão de login está estranho no modo escuro".

Descreva o que você observa de errado na interface.
Liste os problemas visuais encontrados e sugira correções.
```

**Resposta esperada:**
```
Observações:
1. O texto do botão "Entrar" está em cor escura sobre fundo escuro → ilegível
2. O campo de senha não tem borda visível no modo escuro
3. O link "Esqueci a senha" está em azul escuro, quase invisível

Problemas e correções:
1. Botão: trocar cor do texto para branco (#FFFFFF) no tema escuro
2. Campo senha: adicionar borda clara (ex: #555) ou fundo levemente diferente
3. Link: usar cor mais clara (ex: #66B2FF) para contraste adequado
```

## Dicas
- Peça sempre que o modelo DESCREVA o que vê antes de raciocinar
- A descrição verbal da imagem funciona como "âncora" para o raciocínio
- Para gráficos/tabelas, peça extração de dados numéricos específicos
- Combine com few-shot: forneça exemplo de como analisar imagem + texto
- Verifique se o modelo realmente "viu" os detalhes relevantes da imagem

## Fonte
https://www.promptingguide.ai/pt/techniques/multimodalcot
