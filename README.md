# LLM-GNN-RCA

Experiments for microservice log anomaly detection and root-cause localization using frozen Qwen2.5-7B-Instruct log representations and graph neural networks. The intended model route is:

1. Qwen encodes log-window semantics with frozen hidden states.
2. Service-level features are assembled from log statistics, embeddings, and optional metrics.
3. GCN, GAT, and GATv2 model service-call structure and anomaly propagation.
4. Anomaly detection and case-level RCA ranking are evaluated with reproducible failure-case splits.

## Repository layout

```text
configs/        Experiment configuration, including seed, windows, Qwen, and GNN defaults.
models/         Model components. `llm_encoder.py` provides frozen Qwen hidden-state pooling.
preprocessing/  Case-level windowing and trace-edge extraction utilities.
scripts/        Executable data inspection, validation, preprocessing, and embedding-cache scripts.
migration/      Environment and handoff records from the current server setup.
requirements.txt Python and PyTorch/PyG dependency pins.
AGENTS.md       Project rules for data integrity, reproducibility, and execution gates.
data/           Local-only raw data, processed windows, embeddings, and model weights (ignored by Git).
results/        Local-only checkpoints, predictions, metrics, figures, and logs (ignored by Git).
```

## Current progress

The repository currently contains the RE2-TT schema/statistics inspection, case-level preprocessing, a small Qwen embedding validation, and cached 60-second Qwen embeddings. Graph construction, failure-case splits, baselines, GNN models, training, and evaluation are next phases. See [`migration/HANDOFF.md`](migration/HANDOFF.md) for the current handoff status.

The source dataset, cached embeddings, Qwen model files, and generated results are deliberately not committed here. They belong in the separate data/model repository and must be preserved locally according to `AGENTS.md`.

## Configuration

The primary configuration is [`configs/default.yaml`](configs/default.yaml). It fixes seed 42, RE2-TT as the current dataset, 60-second windows (with 30 and 120 seconds supported), the Qwen revision and pooling method, and the default GNN dimensions.

## Existing entry points

The current data preparation and validation entry points are:

```bash
python scripts/validate_cases.py
python scripts/analyze_cases.py
python scripts/preprocess_re2tt.py --config configs/default.yaml
python scripts/validate_qwen_embeddings.py --config configs/default.yaml
python scripts/cache_qwen_embeddings.py --config configs/default.yaml
```

The planned training and evaluation entry points are `train.py`, `evaluate.py`, and `scripts/run_all_experiments.py`; they will be added with the graph and experiment phases.

## Data and reproducibility rules

Do not fabricate labels, telemetry, service edges, predictions, or results. Splits must be made at failure-case level, with seed 42 recorded alongside every split and experiment. Do not begin large-scale training until the data format, statistics, split design, and graph schema have been checked. The complete rules are documented in [`AGENTS.md`](AGENTS.md).
