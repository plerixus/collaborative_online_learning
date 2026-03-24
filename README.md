# Partial Neural-Network Transfer for Collaborative Learning Under Sequential Data Arrival

## Project goal

This repository began as a baseline for incremental learning on sequentially arriving data.
It is now being extended toward a proof of concept for communication-efficient collaborative learning, where only parts of a neural network may be transferred between nodes instead of sending full models.

Traditional batch learning assumes that all training data is available upfront. In many real-world settings, this is not true. New data may arrive over time, and retraining from scratch every time may be inefficient or impractical.

This project explores whether a model can adapt to newly introduced data through incremental updates. It also explores whether partial neural-network transfer between collaborating nodes can preserve much of the benefit of full-model transfer while reducing communication cost.

The repository currently includes:
- Early sequential incremental-learning baselines
- A sequential MLP transfer baseline comparing full transfer against output-layer-only transfer
- A minimal two-client collaborative simulation
- A multi-seed validation of the two-client setup
- A deeper-MLP collaborative experiment with four transfer modes:
  - `local_only`
  - `last_layer_only`
  - `last_two_layers`
  - `full_transfer`

The deeper collaborative experiment is currently the strongest evidence in the repository that richer partial transfer may be competitive with full transfer while using less communication.

## Hypothesis

Selective transfer of only part of a neural network may preserve a useful portion of the performance gains of collaborative updating, while reducing communication cost compared with full-model transfer.

## Repository status

This repository is an early-stage research proof of concept. It currently provides preliminary sequential and collaborative experiments that support a larger proposed study on communication-efficient collaborative learning with partial neural-network transfer.

The repository does **not** implement a full distributed learning framework or a realistic production system. Instead, it is meant to show that:
- Incremental adaptation on sequential data is working
- Partial neural-network transfer is technically feasible in a small collaborative setup
- Communication cost can be tracked explicitly
- Richer partial-transfer strategies may retain much of the benefit of full transfer in some scenarios

## Current experiments

### Baseline A: static vs incremental learning

The current implementation uses a binary classification task on a synthetic dataset to compare:
- A static baseline model trained once on batch 1
- An incremental baseline model updated on later batches

This stage is meant to validate the mechanics of sequential updating before moving to neural-network-based collaborative experiments.

### Baseline B: sequential MLP transfer experiment

This stage introduces a small MLP-based sequential learning setup to test transfer strategies beyond the earlier linear baseline.

It compares:
- full transfer
- output-layer-only transfer

and uses transmitted parameter counts as a simple communication-cost proxy.

This stage serves as an early bridge from sequential learning toward partial neural-network transfer.

### Experiment C: minimal two-client collaborative setup

This is the first collaborative PoC used to compare predictive performance against transfer cost in a multi-client setting.

Main features:
- Shared held-out test set
- Remaining data split between two clients
- Each client receives 3 sequential local batches
- Communication cost approximated by transmitted parameter counts
- Compared across multiple random seeds for robustness
- Final-round performance summarized by mode using mean / variability across seeds

### Experiment D: deeper-MLP collaborative seed validation

This stage extends the two-client collaborative setup by introducing a slightly deeper MLP with three weighted layers, which makes intermediate transfer strategies distinct from full transfer.

It compares:
- `local_only`
- `last_layer_only`
- `last_two_layers`
- `full_transfer`

Main features:
- Same two-client structure for comparability
- Deeper MLP architecture
- Selective layer averaging by transfer mode
- Communication cost tracked using transferred parameter counts
- Multi-seed summary of final-round behavior


## Experiment progression in this repository

- Baseline A: static vs incremental learning on batches
- Baseline B: sequential neural-network transfer on sequential batches
- Experiment C: minimal two-client collaborative setup
- Experiment D: deeper-MLP collaborative seed validation with richer partial-transfer modes
- Main measurement: task performance and estimated transmitted parameter count


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

At the current stage, the experiments show that:

- The static model remains unchanged after batch 1, as expected
- The incremental model changes as new batches are introduced
- When both models start from the same batch-1 training point, the incrementally updated model outperforms the frozen baseline on later batches in this setup
- Performance does **not** improve monotonically with every batch, so the results should not be interpreted as “the model always gets better”

In the MLP-based sequential experiments, different transfer strategies already show a measurable trade-off between predictive performance and estimated transfer cost. In particular, full transfer and output-layer-only transfer do not behave identically, which supports the broader research direction of selective neural-network transfer.

In the initial two-client collaborative experiments:
- Full-model transfer produced the strongest result in the initial collaborative run
- After repeating the setup across multiple random seeds, full transfer still achieved the best mean final F1, but its advantage over local-only training was small and not fully stable
- Output-layer-only transfer remained much cheaper in communication cost, but did not show a stable advantage

The deeper-MLP collaborative experiment produced the strongest current result in the repository:
- `full_transfer` achieved the highest mean final F1 (0.8778)
- `last_two_layers` performed very close to `full_transfer` (0.8767 mean final F1)
- `last_two_layers` did so at substantially lower transfer cost than `full_transfer` (870 vs 1830 mean cumulative transfer cost)
- `last_layer_only` remained the cheapest communication-based transfer mode, but was generally weaker than `last_two_layers`

The most defensible conclusion at this stage is not that one transfer rule is universally best, but that partial neural-network transfer can produce a meaningful performance-versus-communication trade-off that is worth studying further.


## Limitations

This is still a simplified baseline research PoC.

Main limitations:

- The dataset is synthetic
- The data is not truly time-dependent
- There is no explicit concept drift yet
- The setup simulates streamed data through batches rather than using a real streaming pipeline
- The collaborative experiment currently uses only 2 clients
- Only one small dataset (~944 rows after cleaning)
- Only one deeper MLP architecture has been tested in the richer partial-transfer setup
- Only a small number of partial-transfer strategies have been tested so far
- Results have been checked across multiple random seeds, but the ranking between strategies is still somewhat sensitive to seed
- The current collaborative evidence is still based on a small setup (2 clients, one dataset, one main architecture family)

Because of that, this PoC currently demonstrates the mechanics and plausibility of communication-efficient partial transfer more clearly than a full real-world advantage.

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
- Selective layer averaging (`last_layer_only`, `last_two_layers`)
- Small MLP models for sequential and collaborative experiments
- Parameter-count-based communication-cost proxy for transfer comparisons
- Multi-seed collaborative evaluation

## Research directions

Natural extensions of the current proof of concept include:
- Testing on larger datasets
- Increasing the number of clients
- Introducing stronger client heterogeneity
- Evaluating under simulated concept drift
- Studying whether intermediate transfer strategies remain competitive under broader conditions

## Bridge from Current Work to Collaborative Learning

The repository has moved beyond the first collaborative simulation stage and now includes a richer partial-transfer experiment using a deeper MLP.

This matters because the earlier collaborative setup could only compare:
- `local_only`
- A very small partial-transfer rule
- Full transfer

The deeper-MLP experiment adds a real intermediate condition (`last_two_layers`), which makes the project more aligned with the actual research question: whether transferring only part of a neural network can preserve much of the benefit of collaboration while reducing communication cost.

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
    two-client_collaborative_seed_validation.ipynb
    two-client_collaborative_seed_validation_richer_strategy.ipynb
  data/
    data.csv
  results/
    two_client_all_results.csv
    two_client_summary.csv
    two_client_seed_validation_all_rounds.csv
    two_client_seed_validation_final_results.csv
    two_client_seed_validation_summary.csv
    two_client_seed_validation_all_rounds_deeper_nn.csv
    two_client_seed_validation_final_results_deeper_nn.csv
    two_client_seed_validation_summary_deeper_nn.csv

```
## Documentation

Additional project documentation is available in `docs/`, including:
- `research_proposal_draft.md`
- `literature_notes.md`
- `experiment_plan.md`