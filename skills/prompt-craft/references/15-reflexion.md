# Reflexion

## O que é
Framework onde o modelo reflete sobre suas próprias respostas e erros para
melhorá-las iterativamente. Após gerar uma resposta, o agente recebe feedback
(de um avaliador, teste ou auto-avaliação), produz uma "reflexão" verbal
sobre o que deu errado e armazena essa reflexão em memória para guiar
a próxima tentativa. Diferente de métodos que apenas sinalizam "certo/errado",
o Reflexion converte feedback em insights acionáveis. Melhora performance
em tarefas de decisão sequencial, codificação e raciocínio.

## Quando usar
- Geração de código onde testes automatizados podem validar a saída
- Tarefas iterativas onde a resposta pode ser avaliada objetivamente
- Resolução de problemas onde a primeira tentativa frequentemente falha
- Agentes que precisam aprender com erros sem fine-tuning
- Quando se pode executar múltiplas tentativas com feedback entre elas
- Escrita e revisão de documentos/artefatos

## Quando NÃO usar
- Tarefas que precisam de resposta instantânea (sem margem para iteração)
- Quando não há mecanismo de feedback/avaliação disponível
- Tarefas subjetivas sem critérios claros de sucesso
- Se o custo de múltiplas iterações é proibitivo
- Quando a primeira resposta já é consistentemente boa

## Template / Estrutura

### Ciclo de Reflexion

```
TENTATIVA {{ N }}:

Tarefa: {{ descrição_da_tarefa }}

{{ memória de reflexões anteriores, se houver }}

Resposta:
{{ resposta_gerada }}

---
Avaliação: {{ resultado do teste / feedback }}
---

Reflexão: Analise o que deu errado e por quê.
O que devo mudar na próxima tentativa?
{{ reflexão gerada }}

---
(Próxima tentativa usa as reflexões acumuladas como contexto)
```

### Template prático

```
Tarefa: {{ tarefa }}

Reflexões anteriores (lições aprendidas):
{{ reflexão_1 }}
{{ reflexão_2 }}
...

Com base nas lições acima, gere uma nova resposta evitando os erros anteriores.

Resposta:
```

## Exemplo prático

### Exemplo — Geração de código com reflexão

**Tentativa 1:**
```
Tarefa: Escreva uma função Python que receba uma lista de preços e retorne
o desconto progressivo: 5% para compras > R$ 100, 10% para > R$ 500,
15% para > R$ 1000. Aplique sobre o total.
```

**Resposta 1:**
```python
def calcular_desconto(precos):
    total = sum(precos)
    if total > 1000:
        return total * 0.85
    elif total > 500:
        return total * 0.90
    elif total > 100:
        return total * 0.95
    return total
```

**Avaliação:**
```
FALHOU: test_exato_100 - input [100] esperava 100.00, recebeu 100.00 ✓
FALHOU: test_exato_500 - input [500] esperava 475.00, recebeu 475.00 ✓
FALHOU: test_lista_vazia - input [] esperava 0, recebeu 0 ✓
FALHOU: test_negativos - input [-50, 200] levantou erro, esperava tratamento
FALHOU: test_retorno_tipo - esperava dict com {total, desconto, final}, recebeu float
```

**Reflexão 1:**
```
Erros identificados:
1. A função retorna apenas o valor final, mas os testes esperam um dict
   com total original, valor do desconto e valor final.
2. Não trata valores negativos na lista de preços.
Próxima tentativa: retornar dicionário e validar entradas.
```

**Tentativa 2 (com reflexão):**
```
Tarefa: [mesma tarefa]

Reflexões anteriores:
- Retornar dict com {total, desconto, valor_final}, não apenas float
- Tratar valores negativos (ignorar ou levantar erro)

Gere a função corrigida:
```

**Resposta 2:**
```python
def calcular_desconto(precos):
    precos_validos = [p for p in precos if p > 0]
    total = sum(precos_validos)

    if total > 1000:
        percentual = 0.15
    elif total > 500:
        percentual = 0.10
    elif total > 100:
        percentual = 0.05
    else:
        percentual = 0

    desconto = total * percentual
    return {
        "total": total,
        "desconto": desconto,
        "valor_final": total - desconto
    }
```

## Componentes do Reflexion
1. **Actor**: gera a resposta/ação
2. **Evaluator**: avalia a qualidade (testes, heurísticas, outro LLM)
3. **Self-Reflection**: modelo gera insight verbal sobre o erro
4. **Memory**: armazena reflexões para próximas tentativas

## Dicas
- Funciona melhor quando o feedback é específico (não apenas "errado")
- Limite a 3-5 iterações para evitar loops improdutivos
- Reflexões devem ser acionáveis ("mudar X") e não vagas ("tentar melhor")
- Ideal para pair programming com IA: rode testes → reflita → corrija
- Combine com testes automatizados para feedback objetivo

## Fonte
https://www.promptingguide.ai/pt/techniques/reflexion
