# Evals — prisma-risk-testing

## Testes funcionais

- `evals.json` — 3 prompts de QA (planejamento PIX, MR auth rápido, squeeze release).
- Workspace de iteração: `prisma-risk-testing-workspace/iteration-1/`
- Viewer estático (PT-BR): `prisma-risk-testing-workspace/iteration-1/review.html`

Regenerar o viewer em português:

```bash
python3 prisma-risk-testing-workspace/eval-viewer/generate_review_pt.py \
  prisma-risk-testing-workspace/iteration-1 \
  --skill-name "prisma-risk-testing" \
  --benchmark prisma-risk-testing-workspace/iteration-1/benchmark.json \
  --static prisma-risk-testing-workspace/iteration-1/review.html
```

## Otimização de triggering (opcional)

Arquivo `trigger_eval.json` com 20 queries (10 should-trigger / 10 should-not-trigger).

A partir da **raiz do repositório**, defina as variáveis e execute:

```bash
REPO_ROOT="$(git rev-parse --show-toplevel)"
SKILL_CREATOR="$HOME/.claude/plugins/cache/claude-plugins-official/skill-creator/unknown/skills/skill-creator"

cd "$SKILL_CREATOR"

python -m scripts.run_loop \
  --eval-set "$REPO_ROOT/.claude/skills/prisma-risk-testing/evals/trigger_eval.json" \
  --skill-path "$REPO_ROOT/.claude/skills/prisma-risk-testing" \
  --model <model-id-da-sessao> \
  --max-iterations 5 \
  --verbose
```

Requer `claude` CLI e credenciais Anthropic configuradas.
