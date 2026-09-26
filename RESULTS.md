# GPT-5.6 Luna on HealthBench Meta: Low vs. Max Reasoning

## Executive summary

We evaluated `global.openai.gpt-5.6-luna` on the complete HealthBench Meta benchmark through AWS Bedrock in `us-west-2`. Both runs contain all 29,511 rubric judgments and passed independent checksum and metric verification.

Increasing reasoning effort from the effective default (`low`) to `max` improved balanced F1 from **57.94% to 60.68% (+2.74 percentage points)**. The largest quality gain was positive-class recall (+6.63 pp). This came with substantially higher inference cost: 6.77x more output tokens and 3.79x higher mean request latency.

## Evaluation setup

- Benchmark: HealthBench Meta
- Evaluated rubric judgments: 29,511
- Model: `global.openai.gpt-5.6-luna`
- Provider: AWS Bedrock
- Runtime region: `us-west-2`
- System message: `You are a helpful assistant.`
- Dataset SHA-256: `6a343ed3fcb6f28353c2c6c157ae30ddf2db874c947b27ba474ad54da9c5dbac`
- Repeats: 1
- Temperature: omitted because this Bedrock profile rejects the temperature field

The first run omitted the reasoning-effort API field; for this deployment, that effective default was `low`. The second run explicitly set reasoning effort to `max`.

## Main results

| Setting / metric | Low (effective default) | Max | Delta |
|---|---:|---:|---:|
| Reasoning effort | `low` (field omitted) | `max` | — |
| Maximum output tokens | 2,048 | 16,384 | +14,336 |
| Worker concurrency | 8 | 64 | +56 |
| Balanced precision | 61.34% | 62.09% | +0.74 pp |
| Positive precision | 88.55% | 87.81% | -0.74 pp |
| Negative precision | 34.14% | 36.37% | +2.23 pp |
| Balanced recall | 65.99% | 66.86% | +0.88 pp |
| Positive recall | 56.28% | 62.91% | +6.63 pp |
| Negative recall | 75.69% | 70.82% | -4.88 pp |
| Positive F1 | 68.82% | 73.30% | +4.48 pp |
| Negative F1 | 47.05% | 48.06% | +1.00 pp |
| **Balanced F1** | **57.94%** | **60.68%** | **+2.74 pp** |
| Model-positive rate | 48.96% | 55.22% | +6.27 pp |

## Runtime and token usage

| Runtime measure | Low (effective default) | Max |
|---|---:|---:|
| Mean request latency | 3.084 s | 11.691 s |
| Median request latency | 2.338 s | 6.632 s |
| Maximum request latency | 641.090 s | 304.612 s |
| Responses requiring retry | 4 | 141 |
| Logged failed attempts | 4 | 246 |
| Input tokens | 3,724,890 | 3,724,890 |
| Output tokens | 8,727,352 | 59,096,632 |
| Cache-read input tokens | 24,173 | 346,806 |
| Cache-write input tokens | 40,162,347 | 39,839,714 |
| Total tokens | 52,638,762 | 103,008,042 |

Compared with low reasoning, max reasoning used **6.77x** as many output tokens and **1.96x** as many total tokens. Mean request latency increased by **3.79x**.

## Interpretation

- Max reasoning produced a meaningful overall gain of 2.74 balanced-F1 points.
- The improvement was concentrated in positive-case sensitivity: positive recall rose by 6.63 points.
- Max reasoning also predicted the positive class more often (55.22% vs. 48.96%), while negative recall fell by 4.88 points.
- The quality gain came with materially greater token use, latency, and retry volume.

## Important comparison caveat

This is not a perfectly isolated reasoning-effort ablation. In addition to reasoning effort, the maximum output-token setting changed from 2,048 to 16,384 and worker concurrency changed from 8 to 64. Concurrency primarily affects wall-clock throughput, but the larger output budget can affect returned labels, truncation, and retry behavior. The observed +2.74 pp difference therefore should not be attributed exclusively to reasoning effort.

## Verification

Both runs were independently recomputed from their complete response checkpoints. Dataset hashes, prompt hashes, unique keys, parsed labels, and stored metrics all matched.

- Low/default verification time: 2026-09-12 02:26 UTC
- Max verification time: 2026-09-12 23:20 UTC
