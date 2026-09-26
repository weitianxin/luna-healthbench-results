# Luna HealthBench Meta results

Comparison of two complete HealthBench Meta runs using GPT-5.6 Luna:

- Effective default/low reasoning
- Max reasoning

See [`RESULTS.md`](RESULTS.md) for the comparison summary.

The compressed archive contains the source dataset, both full runs, model
generations, judge decisions and explanations, aggregate metrics, execution
configuration, errors, verification files, and Slurm logs.

Each `responses.jsonl` row includes:

- `response_text`: model generation
- `criteria_met`: judge decision
- `explanation`: judge rationale
- `response_metadata`: token usage, stop reason, and request metrics
- `latency_seconds`: sample latency

Verify the archive after downloading:

```bash
sha256sum -c luna_healthbench_low_vs_max_complete_2026-09-26.tar.gz.sha256
```
