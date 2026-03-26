# Communication-Efficient Collaborative Learning Under Sequential Data Arrival via Partial Neural-Network Transfer

## Background
Machine learning systems are often trained under the assumption that data is collected centrally and remains reasonably stable during training. In many realistic settings, however, data is distributed across multiple clients and arrives over time rather than being available all at once. This creates two linked challenges. First, collaborative or decentralized learning must cope with communication costs between participants. Second, the learning system must keep adapting as new data arrives without discarding useful earlier knowledge.

Federated learning addresses collaborative training under decentralized data by keeping raw data on clients and exchanging model information instead. A well-known baseline is Federated Averaging (FedAvg), where clients train locally and a server averages updates. This line of work highlights communication rounds as a major bottleneck and shows that increasing local computation can reduce the amount of communication needed. Continual learning, meanwhile, focuses on adaptation under non-stationary or sequential data, where concept drift and catastrophic forgetting become central concerns. Recent Federated Continual Learning (FCL) work combines these two perspectives and frames the combined problem as one of distributed, non-stationary, resource-constrained learning.

## Problem Statement
Many collaborative learning methods rely on repeated transfer of full model updates. While effective, this can be communication-heavy, especially when clients operate under bandwidth or resource constraints. At the same time, if data arrives sequentially and may change over time, the learning process must remain adaptive rather than treating training as a one-shot procedure.

This project investigates whether the amount of transferred model information can be reduced without giving up too much adaptation performance. In particular, it asks whether transferring only selected parts of a neural network can preserve a meaningful portion of the benefit of collaborative adaptation while lowering communication cost.

## Research Question
Can partial neural-network transfer in a collaborative learning setting reduce communication cost while preserving adaptation performance under sequentially arriving data?

## Sub-Questions
- How does partial transfer compare with local-only or static learning under sequential data arrival?
- How does partial transfer compare with full-model transfer in terms of predictive performance?
- What is the trade-off between predictive performance and communication cost across transfer strategies?
- How robust are different transfer strategies under heterogeneous or changing data distributions?

## Hypothesis
Selective transfer of only part of a neural network can preserve much of the adaptation benefit of collaborative updating while reducing communication cost compared with full-model exchange.

## Preliminary Work
The repository already contains a preliminary collaborative learning proof of concept that supports the overall direction of this proposal. The project began with simpler sequential-learning experiments and then progressed toward neural-network transfer experiments with explicit communication-cost tracking. The current strongest stage uses a deeper two-client collaborative setup in which multiple transfer strategies can be compared directly.

At present, the implemented setup includes four transfer modes: `local_only`, `last_layer_only`, `last_two_layers`, and `full_transfer`. The experiments are evaluated using predictive metrics together with a communication-cost proxy based on transferred parameter counts. This allows the project to examine not only whether collaboration helps, but also whether intermediate transfer strategies can preserve useful performance while reducing communication cost.

The current results are still small-scale and should be interpreted cautiously. However, they already provide more than a conceptual starting point: they show that the proposed research question can be explored empirically in a controlled collaborative setting.

A useful preliminary result is that `full_transfer` achieved the strongest mean final F1 score in the current multi-seed summary, while `last_two_layers` performed very closely at substantially lower communication cost. This does not establish a universal rule, but it does support the central research direction of studying richer partial-transfer strategies as a compromise between local-only learning and full-model exchange.

## Proposed Method
The project will be developed in stages, but the early collaborative proof of concept has already been established. The next steps therefore focus less on introducing collaboration for the first time and more on extending, stress-testing, and refining the current setup.

### Phase 1: Consolidation of the Current Collaborative Baseline
The existing two-client collaborative neural-network setup will be treated as the baseline experimental environment. This phase will focus on verifying reproducibility, clarifying evaluation procedures, and ensuring that the comparison between `local_only`, `last_layer_only`, `last_two_layers`, and `full_transfer` is reported consistently.

### Phase 2: Extension to More Challenging Data Conditions
The collaborative setting will be extended to more realistic conditions, such as stronger client heterogeneity, uneven data availability, non-IID splits, or more explicit sequential change in the incoming data. The goal of this phase is to evaluate whether the observed trade-offs remain meaningful when the environment becomes less controlled.

### Phase 3: Richer Partial-Transfer Strategies
Beyond simple layer-based transfer, the project may explore more selective transfer schemes, such as transferring chosen blocks or other architecture-dependent subsets of parameters. The purpose of this phase is to test whether the communication-performance balance can be improved further beyond the current fixed layer splits.

### Phase 4: Comparative Evaluation
All strategies will be compared using predictive metrics together with communication-aware metrics. The central objective is not only to maximize predictive performance, but to evaluate which transfer strategies provide the most useful trade-off between adaptation and communication cost under sequential data arrival.

## Evaluation Plan
The proposed approach will be evaluated using:

- Accuracy
- Precision
- Recall
- F1 score
- Transfer or communication cost proxy
- Possibly update time or runtime-related indicators

The main comparison will be between:

1. local-only or static learning,
2. full-model collaborative transfer,
3. partial neural-network transfer.

Additional experiments may examine robustness under non-IID client data, concept drift, or uneven client participation.

## Bridge from Current Work to Collaborative Learning
The current repository already goes beyond a purely sequential transfer baseline. It now includes a minimal collaborative learning setup with two clients, a shared evaluation procedure, explicit communication-cost tracking, and multiple transfer conditions ranging from local-only learning to full transfer.

This means the project no longer needs to justify collaboration only in theoretical terms. Instead, the current proof of concept acts as an initial collaborative baseline on top of which more realistic and more demanding experiments can be built. The main limitation is therefore not the absence of collaboration, but the small scale and simplified nature of the present setup.

The next research step is to extend this collaborative baseline rather than replace it. In particular, future work should test whether the current performance-versus-communication trade-offs remain stable under more heterogeneous client data, more clients, and more difficult sequential conditions.

## Related Work
Federated learning provides a framework for collaborative model training without centralizing raw data. McMahan et al. introduced Federated Averaging (FedAvg), showing that communication-efficient decentralized training can be achieved through local client updates followed by server-side averaging. This work is relevant because it establishes communication cost and client heterogeneity as central issues in collaborative learning.

Split learning extends collaborative training by partitioning neural networks across participants. In SplitNN, clients and servers train different parts of the same model and exchange intermediate activations and gradients instead of raw data. This is relevant to the present project because it shows that selective sharing of model computation can reduce client burden while preserving collaborative learning capability.

Neurosurgeon studies intelligent partitioning of deep neural network execution between mobile devices and the cloud. Its core contribution is showing that different layers have different compute and communication profiles, and that choosing partition points carefully can improve efficiency. While Neurosurgeon focuses on inference offloading rather than collaborative continual training, it provides useful inspiration for selective neural-network transfer strategies.

Federated Continual Learning combines collaborative decentralized learning with continual adaptation to sequentially arriving, non-stationary data. Recent survey work highlights key challenges including heterogeneity, model stability, communication overhead, privacy preservation, concept drift, and catastrophic forgetting. This provides the closest overall framing for the present project.

## Expected Contribution
This work aims to contribute:

1. A collaborative learning framework for studying partial neural-network transfer under sequential data arrival,
2. A prototype experimental setup with explicit communication-cost tracking across multiple transfer strategies,
3. An empirical analysis of the trade-off between predictive performance and communication cost in a small-scale collaborative setting,
4. A research direction showing that intermediate transfer strategies may preserve much of the benefit of full-model collaboration while requiring less communication,
5. A proposal for future work that connects preliminary experimental evidence with broader questions in communication-efficient collaborative and continual learning.

## Current Scope and Limitations
This project is currently at the proposal and proof-of-concept stage. The existing repository already contains a real collaborative neural-network experiment, but the setup remains small-scale and simplified. In particular, the current evidence is based on a limited number of clients, a limited dataset, and a communication-cost proxy based on transferred parameter counts rather than real system-level bandwidth measurements.

For that reason, the current results should be treated as preliminary support for the feasibility of the research direction rather than as definitive evidence. The immediate goal is not to claim a final solution, but to show that the proposed question is technically plausible, aligned with relevant literature, and supported by early empirical results.
