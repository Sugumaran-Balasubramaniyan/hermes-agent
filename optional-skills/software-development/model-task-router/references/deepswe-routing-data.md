# DeepSWE Routing Data — Why Model Routing Matters

> **Updated 2026-06-10**: Cost figures corrected based on [DeepSWE issue #21](https://github.com/datacurve-ai/deep-swe/issues/21) — DeepSeek's cache-hit pricing was not applied, inflating costs 4-14×. The solve-rate data (8%) is unaffected. The routing recommendation stands, but the justification shifts from cost to reliability.

## The Evidence

DeepSWE ([deepswe.datacurve.ai](https://deepswe.datacurve.ai)) is the only contamination-free, real-complexity coding benchmark. Tasks are written from scratch, require 5.5× more code than SWE-bench Pro, and span 91 repositories across 5 languages.

## Key Finding: Models Collapse on Real Engineering Tasks

| Model | SWE-bench Pro | DeepSWE | Collapse |
|-------|:------------:|:-------:|:--------:|
| GPT-5.5 (xhigh) | 58.6% | **70%** | +11 — improves |
| GPT-5.4 (xhigh) | 57.7% | **56%** | −2 — holds |
| Claude Opus 4.7 | 64.3% | 54% | −10 |
| MiniMax M3 | 59.0% | 20% | **−39** |
| DeepSeek V4-Pro | 55.4% | **8%** | **−47** |

## Cost Analysis (Cache-Adjusted, June 2026)

DeepSeek V4-Pro's actual pricing includes cache-hit rates at $0.0036/M (99.2% off cache-miss). DeepSWE's reported costs billed all tokens at the miss rate. Corrected:

| Model | DeepSWE | Cost/Task | Cost/Solve | Attempts/Solve |
|-------|:-------:|:---------:|:----------:|:--------------:|
| GPT-5.5 | **70%** | $6.61 | $9.44 | 1.4 |
| GPT-5.4 | **56%** | $4.38 | **$7.82** | 1.8 |
| GPT-5.4-Mini | 24% | $2.08 | $8.67 | 4.2 |
| DeepSeek V4-Pro | **8%** | **$0.30** | **$3.75** | 12.5 |

**Key insight:** V4-Pro costs the least per eventual solve ($3.75), but requires **12.5× more attempts** than GPT-5.4 (1.8). The routing decision isn't about cost — it's about reliability. A task routed to V4-Pro fails 92% of the time on first attempt.

### Additional Concerns (from DeepSWE Issue #21)

- **No effort tuning**: V4-Pro was run with `reasoning_effort: null` while all other models had tuned effort levels (xhigh, max, medium). Its 8% score may improve with proper tuning.
- **OpenRouter privacy guardrail**: OpenRouter blocks DeepSeek by default (data-training concern). Benchmark tests may have failed from 404 errors, not model capability.

## Terminal-Bench (Agentic CLI)

| Model | Score | $/1M Output |
|-------|:-----:|:-----------:|
| GPT-5.5 | 82.7% | $30.00 |
| DeepSeek V4-Pro | 67.9% | $0.87 |

## The Routing Decision

| Task Type | Route to | Why |
|-----------|----------|-----|
| Code Generation | GPT-5.4 | 56% success, 1.8 attempts — gets it done |
| Architecture | GPT-5.5 | 70% success, 1.4 attempts — best-in-class |
| Orchestration | V4-Pro | 67.9% Terminal-Bench, cheap, reliable at tools |
| Mechanical | GPT-5.4-Mini | 24% success, delegated, fast |

V4-Pro should not code because it fails 92% of real coding tasks — not because it costs more per solve.

## Sources

- DeepSWE Leaderboard: https://deepswe.datacurve.ai/
- DeepSWE Issue #21 (cost correction): https://github.com/datacurve-ai/deep-swe/issues/21
- DeepSWE Blog: https://deepswe.datacurve.ai/blog
- DeepSeek V4 Technical Report: https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro
- Kilo Leaderboard: https://kilo.ai/leaderboard
