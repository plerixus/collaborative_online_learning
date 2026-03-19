# Incremental Learning on Streaming-Like Data

## Project goal

This repository began as a baseline for incremental learning on sequentially arriving data.
It is now being extended toward a proof of concept for communication-efficient collaborative learning, where only parts of a neural network may be transferred between nodes instead of sending full models.

Traditional batch learning assumes that all training data is available upfront. In many real-world settings, this is not true. New data may arrive over time, and retraining from scratch every time may be inefficient or impractical.

This project explores whether a model can adapt to newly introduced data through incremental updates. It will also explore efficiency of partial neural network transfer between nodes in collaborative setting.

## Hypothesis

Selective transfer of only part of a neural network may preserve a useful portion of the performance gains of collaborative updating, while reducing communication cost compared with full-model transfer.

## Current baseline experiment

The current implementation uses a binary classification task on a synthetic dataset to compare:
- a static baseline model trained once on batch 1
- an incremental baseline model updated on later batches

This stage is intended to validate the mechanics of sequential updating before moving to neural-network-based collaborative experiments.

## Planned research progression

- Baseline A: static vs incremental learning on batches
- Baseline B: neural-network continual update on sequential batches
- Experiment C: two-client collaborative setup
- Experiment D: partial model transfer vs full model transfer
- Measure: task performance and estimated transmitted parameter count

## Models used

- `SGDClassifier(loss="log_loss")`
- `StandardScaler`

Two separate scalers are used so that the static and incremental pipelines remain independent.

## Evaluation metrics

The models are compared using:

- Accuracy
- Precision
- Recall
- F1 score

The main focus is on how performance changes across batches.

## Research question

Can partial neural-network transfer in a collaborative learning setting reduce communication cost while preserving adaptation performance on sequentially arriving data?

## Current findings

At the current stage, the experiment shows that:

- The static model remains unchanged after batch 1, as expected
- tThe incremental model changes as new batches are introduced
- When both models start from the same batch-1 training point, the incrementally updated model outperforms the frozen baseline on later batches in this setup
- Performance does **not** improve monotonically with every batch, so the results should not be interpreted as “the model always gets better”

The main takeaway so far is that incremental updating works technically and produces meaningfully different behavior from a static baseline.

## Limitations

This is still a simplified baseline experiment.

Main limitations:

- the dataset is synthetic
- the data is not truly time-dependent
- there is no explicit concept drift yet
- the setup simulates streamed data through batches rather than using a real streaming pipeline

Because of that, this PoC currently demonstrates the **mechanics** of incremental learning more clearly than a full real-world advantage.

## Data

Current dataset:
- [Machine Failure Prediction using Sensor data](https://www.kaggle.com/datasets/umerrtx/machine-failure-prediction-using-sensor-data/data)

## Next steps

- replace the linear baseline with a small neural network baseline
- simulate two collaborative clients/nodes with sequential local updates
- define a communication-cost proxy using parameter counts
- compare full-model transfer against partial-layer transfer
- evaluate the trade-off between predictive performance and communication cost

## Project structure

```text
aizu-poc/
  README.md
  requirements.txt
  notebooks/
    incremental_learning_baseline.ipynb
  data/
    data.csv
