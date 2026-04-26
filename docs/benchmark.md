# Token Saving Benchmark

This page measures visible prompt-token estimates for non-English Codex prompts before and after translation to English.

The numbers are intentionally conservative and reproducible: they use the same rough estimator as `codex-ts`. They are not exact billing numbers, and they do not include hidden reasoning tokens. Treat them as a visible-prompt benchmark, not an invoice.

## Medium Coding Task

Task: inspect why repository tests fail and modify only the necessary files.

| Language | Task | Original visible tokens | English visible tokens | Estimated saving |
|---|---|---:|---:|---:|
| Chinese | medium | 35 | 24 | 31.4% |
| Japanese | medium | 49 | 24 | 51.0% |
| Korean | medium | 41 | 24 | 41.5% |
| Thai | medium | 89 | 24 | 73.0% |
| Hindi | medium | 93 | 24 | 74.2% |
| Arabic | medium | 70 | 24 | 65.7% |
| Spanish | medium | 26 | 24 | 7.7% |
| French | medium | 26 | 24 | 7.7% |

## What This Means

The biggest visible-token wins appear for scripts that are expensive under many LLM tokenizers: Thai, Hindi, Arabic, Japanese, Korean, and Chinese. Latin-script languages often save less, but `codex-ts` still keeps the final-answer language experience consistent.

This benchmark is useful because Codex Token Saver optimizes the prompt that the main Codex run sees. It cannot inspect or reduce hidden reasoning tokens after the main model starts working.

## Reproduce

```bash
python3 scripts/benchmark_visible_tokens.py
```

For a real end-to-end check, run:

```bash
codex-ts doctor
codex-ts exec "请帮我检查这个仓库为什么测试失败，并尽量只修改必要的文件。"
```

You should see a report similar to:

```text
codex-ts: estimated prompt tokens 35 -> 24 (-11); language=Chinese; elapsed=...
```

## Benchmark Roadmap

- Add long-task prompts with refactor, tests, and explanation requests.
- Add tokenizer-specific measurements for OpenAI-compatible tokenizers.
- Add real-world cost comparisons when Codex exposes stable usage accounting.
- Add benchmark snapshots for releases.
