# Incremental Learning on Streaming-Like Data

## Project goal

This project is a small proof of concept for incremental learning.

The goal is to show how a machine learning model can be updated as new batches of data arrive, instead of being trained once and then kept fixed forever. The current setup compares:

- a **static model** trained once on the first batch
- an **incremental model** trained on the first batch and then updated batch by batch using `partial_fit()`

## Why this matters

Traditional batch learning assumes that all training data is available upfront. In many real-world settings, this is not true. New data may arrive over time, and retraining from scratch every time may be inefficient or impractical.

This PoC explores whether a model can adapt to newly introduced data through incremental updates.

## Current experiment setup

The experiment uses a binary classification task on a synthetic dataset.

The workflow is:

1. Generate or prepare the dataset
2. Split training data into sequential batches
3. Train both models on batch 1 using `fit()`
4. Keep the static model frozen
5. Update the incremental model on later batches using `partial_fit()`
6. Evaluate both models on the same held-out test set after each batch

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

## Current findings

At the current stage, the experiment shows that:

- the static model remains unchanged after batch 1, as expected
- the incremental model changes as new batches are introduced
- after aligning both models to start from the same batch-1 training point, the incremental model performs better than the frozen static baseline on later batches in this setup
- performance does **not** improve monotonically with every batch, so the results should not be interpreted as “the model always gets better”

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

For this project these datasets were used:
- [Machine Failure Prediction using Sensor data](https://www.kaggle.com/datasets/umerrtx/machine-failure-prediction-using-sensor-data/data)

## Next steps

Possible next improvements:

- introduce concept drift
- test on a more realistic dataset
- compare additional incremental models
- improve project structure and reproducibility
- add a clearer experimental summary for supervisor review

## Project structure

```text
aizu-poc/
  README.md
  requirements.txt
  notebooks/
    incremental_learning_baseline.ipynb
  data/
    data.csv
