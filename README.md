# Partial Neural-Network Transfer for Collaborative Learning Under Sequential Data Arrival

## Project goal

This repository began as a baseline for incremental learning on sequentially arriving data.
It is now being extended toward a proof of concept for communication-efficient collaborative learning, where only parts of a neural network may be transferred between nodes instead of sending full models.

Traditional batch learning assumes that all training data is available upfront. In many real-world settings, this is not true. New data may arrive over time, and retraining from scratch every time may be inefficient or impractical.

This project explores whether a model can adapt to newly introduced data through incremental updates. It also explores the efficiency of partial neural-network transfer between nodes in a collaborative setting.

The repository already includes preliminary sequential experiments with an MLP, including a comparison between full transfer and output-layer-only transfer, together with a simple communication-cost proxy based on transmitted parameter counts.

The repository now also includes a minimal two-client collaborative simulation comparing local-only learning, full-model transfer, and output-layer-only transfer under sequential local data arrival.

## Hypothesis

Selective transfer of only part of a neural network may preserve a useful portion of the performance gains of collaborative updating, while reducing communication cost compared with full-model transfer.

## Repository status

This repository is an early-stage research proof of concept. It currently provides preliminary sequential experiments that support a larger proposed study on collaborative learning with partial neural-network transfer. It includes sequential baselines, sequential MLP transfer experiments and a minimal two-client collaborative prototype. It does not implement a full realistic distributed system or large-scale collaborative framework.

## Current experiments

### Baseline A: static vs incremental learning

The current implementation uses a binary classification task on a synthetic dataset to compare:
- A static baseline model trained once on batch 1
- An incremental baseline model updated on later batches

This stage is intended to validate the mechanics of sequential updating before moving to neural-network-based collaborative experiments.

### Baseline B: sequential MLP transfer experiment

This stage introduces a small MLP-based sequential learning setup to test transfer strategies beyond the earlier linear baseline. The current experiment compares full transfer against output-layer-only transfer and uses transmitted parameter counts as a simple communication-cost proxy. The purpose of this stage is to establish an early performance-versus-cost trade-off before implementing a true multi-client collaborative setting.

- MLP sequential setup
- Full transfer vs output-layer-only transfer
- Parameter-count transfer-cost proxy
- Preliminary performance vs communication trade-off

### Experiment C: minimal two-client collaborative setup

This is the first collaborative PoC used to compare predictive performance against transfer cost in a multi-client setting.
- Shared held-out test set
- Remaining data split between two clients
- Each client receives 3 sequential local batches
- Communication cost approximated by transmitted parameter counts


## Planned research progression

- Baseline A: static vs incremental learning on batches
- Baseline B: neural-network continual update on sequential batches
- Experiment C: two-client collaborative setup
- Next: richer partial-transfer strategies and repeated runs across seeds.
- Next: robustness under heterogeneous or drifting client data
- Experiment D: partial model transfer vs full model transfer
- Measure: task performance and estimated transmitted parameter count



## Evaluation metrics

The models are compared using:

- Accuracy
- Precision
- Recall
- F1 score

The main focus is on how predictive performance changes across batches and how that performance relates to estimated transfer cost.

## Research question

Can partial neural-network transfer in a collaborative learning setting reduce communication cost while preserving adaptation performance on sequentially arriving data?

## Current findings

At the current stage, the experiment shows that:

- The static model remains unchanged after batch 1, as expected
- The incremental model changes as new batches are introduced
- When both models start from the same batch-1 training point, the incrementally updated model outperforms the frozen baseline on later batches in this setup
- Performance does **not** improve monotonically with every batch, so the results should not be interpreted as “the model always gets better”
- In the MLP-based sequential experiments, different transfer strategies already show a measurable trade-off between predictive performance and estimated transfer cost. In particular, full transfer and output-layer-only transfer do not behave identically, which supports the broader research direction of selective neural-network transfer.
- Full-model transfer produced the strongest improvement in initial two-client collaborative setup
- Output-layer-only transfer remained close to local-only performance in initial two-client collaborative setup
- Output-layer-only transfer greatly reduced communication cost relative to full transfer
- This suggests a measurable performance-vs-communication trade-off, but naive head-only transfer may be too weak to recover most of the benefits of full collaboration- 
The main takeaway so far is that incremental updating works technically and produces meaningfully different behavior from a static baseline.

## Limitations

This is still a simplified baseline experiment.

Main limitations:

- The dataset is synthetic
- The data is not truly time-dependent
- There is no explicit concept drift yet
- The setup simulates streamed data through batches rather than using a real streaming pipeline
- Current collaborative experiment uses only 2 clients
- Only one small dataset (~944 rows after cleaning)
- Only one simple MLP architecture
- Only one naive partial-transfer strategy (output layer only)
- Results are based on an initial run and should be validated across multiple random seeds


Because of that, this PoC currently demonstrates the **mechanics** of incremental learning more clearly than a full real-world advantage.

## Data

Current dataset:
- [Machine Failure Prediction using Sensor data](https://www.kaggle.com/datasets/umerrtx/machine-failure-prediction-using-sensor-data/data)

## Models and components used

Current repository components include:
- `SGDClassifier(loss="log_loss")` for the early incremental baseline
- `StandardScaler` for the linear baseline pipeline
- Two-client collaborative simulation
- Shared-test evaluation protocol
- Full-model parameter averaging
- Output-layer-only averaging
- A small MLP model for sequential transfer experiments
- Parameter-count-based communication-cost proxy for transfer comparisons

## Next steps

- Repeat the two-client experiment across multiple random seeds
- Test richer partial-transfer strategies beyond output-layer-only
- Introduce stronger client heterogeneity
- Evaluate under simulated concept drift
- Compare communication/performance trade-offs more systematically
- Optionally extend from 2 clients to more clients later

## Bridge from Current Work to Collaborative Learning

The repository has now reached the first multi-client simulation stage. This collaborative stage is still simplified, it serves as a bridge between sequential transfer experiments and more realistic future collaborative learning research.

## Project structure

```text
collaborative_online_learning/
  README.md
  requirements.txt
  docs/
    research_proposal_draft.md
    literature_notes.md
    experiment_plan.md
  notebooks/
    incremental_learning_baseline.ipynb
    mlp_sequential_baseline.ipynb
    two-client_collaborative_setup.ipynb
  data/
    data.csv
  results/
    two_client_all_results.csv
    two_client_summary.csv


```
## Documentation

Additional project documentation is available in `docs/`, including:
- `research_proposal_draft.md`
- `literature_notes.md`
- `experiment_plan.md`