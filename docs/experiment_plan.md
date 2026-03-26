# Experiment Plan

## Purpose
This file tracks completed experiment stages, the reasoning behind upcoming experiments, and the criteria for interpreting future results. Unlike the README, which summarizes the project for external readers, this document is intended as a working plan for guiding the next development steps of the proof of concept.

## Completed Experiment Stages

### 1. Incremental Learning Baseline
A sequential learning baseline was implemented to test whether a model updated batch by batch could adapt to changing data more effectively than a static model trained only once on the initial batch.

**Main takeaway:**  
Incremental updating showed that adaptation across batches was technically feasible, but the results were not strong enough on their own to support the full collaborative-learning research direction.

### 2. Initial Neural-Network Transfer Experiment
A simple MLP-based setup was used to compare local training, output-layer-only transfer, and full-model transfer in a sequential setting.

**Main takeaway:**  
Output-layer-only transfer sometimes performed competitively, but the result was unstable and too sensitive to small-scale conditions to support a strong conclusion.

### 3. Deeper Collaborative Transfer Experiment
A deeper neural-network setup was introduced in a two-client collaborative setting with four modes: `local_only`, `last_layer_only`, `last_two_layers`, and `full_transfer`.

**Main takeaway:**  
`full_transfer` achieved the strongest mean final performance, while `last_two_layers` performed very closely at substantially lower communication cost. This is the strongest current support for the proposal direction.

## Current Interpretation
The current evidence does not show that partial transfer universally outperforms full transfer. Instead, it suggests that richer partial-transfer strategies may preserve much of the collaborative benefit of full transfer while reducing communication overhead. This trade-off is the main motivation for the next experiments.

## Next Experiments

### 1. More Clients
Increase the number of participating clients beyond the current minimal setup.

**Why:**  
The current two-client configuration is useful as a proof of concept, but it is too limited to say much about collaborative behavior in more distributed settings.

**What would be interesting:**  
If `last_two_layers` remains close to `full_transfer` as the number of clients increases, that would strengthen the communication-efficiency argument.

### 2. Stronger Heterogeneity / Non-IID Splits
Create more distinct client datasets instead of relatively mild partitioning.

**Why:**  
Collaborative learning becomes more meaningful when local client data differs more substantially.

**What would be interesting:**  
If partial transfer still remains competitive under more heterogeneous conditions, that would make the proposal much stronger.

### 3. More Explicit Sequential Change
Introduce stronger drift-like or stage-based change in incoming data.

**Why:**  
The proposal is specifically about sequentially arriving data, so the experiments should eventually reflect a more demanding sequential environment.

**What would be interesting:**  
If collaborative transfer helps adaptation more clearly under stronger sequential change, that would improve the argument for the research question.

### 4. Richer Partial-Transfer Schemes
Test whether layer-based splitting can be extended to other selective transfer strategies.

**Why:**  
The current layer-based setup is useful, but still simple.

**What would be interesting:**  
If a more selective transfer strategy can match or exceed `last_two_layers` on the performance-to-cost trade-off, that could become a central part of the final research direction.

## Decision Rule for the Next Sprint
The next sprint should prioritize experiments that strengthen the main proposal claim rather than simply adding more code. In practical terms, the most valuable next step is whichever experiment best tests whether the current `last_two_layers` result remains meaningful under more realistic collaborative conditions.

## Notes
This file is meant to evolve. Completed experiments can be moved upward into the history section, while future ideas should remain focused on testing the central trade-off between predictive performance and communication cost.