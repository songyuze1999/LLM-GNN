# Project Handoff

Current phase: Phase 6 completed.

Dataset:
- Dataset: RE2-TT
- Cases requested: 90
- Cases with cached embeddings: 89
- Case without logs: re2tt_ts-auth-service_cpu_1
- Failed cases: 0

Embeddings:
- Path: data/processed/embeddings/qwen/60s
- Rows: 93590
- Dimension: 3584
- Pooling: mean
- Model revision: a09a35458c702b33eeacc393d103063234e8bc28
- Seed: 42

Next phase:
- Phase 7: construct service graphs from trace edges.
- Do not start large-scale training before checking data splits and graph schema.
