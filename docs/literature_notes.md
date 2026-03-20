# Literature Notes

## 1. Communication-Efficient Learning of Deep Networks from Decentralized Data (McMahan et al.)

### Core problem
How can a global model be trained across decentralized clients without centralizing raw data, while keeping communication costs manageable?

### Main idea
The paper introduces Federated Averaging (FedAvg). Instead of sending raw data to a central server, clients perform local training and periodically send model updates. The server aggregates these updates, usually by weighted averaging. The paper argues that communication is a primary bottleneck in federated settings and shows that increasing local client computation can reduce the number of communication rounds needed.

### Why it matters for this project
This is the main baseline reference for collaborative decentralized learning. It gives a clean starting point for any comparison involving communication-efficient collaborative updates.

### Useful points to cite
- Communication is often more expensive than local computation in federated settings.
- FedAvg extends the basic federated SGD idea by allowing multiple local update steps before averaging.
- The method is designed with unbalanced and non-IID client data in mind.

### Similarity to my project
My project also focuses on collaborative learning under communication constraints.

### Difference from my project
FedAvg assumes collaborative model exchange but does not focus on partial neural-network transfer as the main research question. It is also not specifically framed around continual or sequential adaptation.

---

## 2. Split Learning for Health: Distributed Deep Learning without Sharing Raw Patient Data (SplitNN)

### Core problem
How can multiple institutions collaborate in deep learning without sharing raw data, while reducing client-side resource demands?

### Main idea
The paper uses split learning, where clients train only the first part of a neural network up to a cut layer and send activations to a server. The server completes the forward pass and sends gradients back to the client for the backward pass. This allows collaborative training without sharing raw input data. The paper also presents several configurations for different practical settings.

### Why it matters for this project
This is the strongest reference for selective or partial collaborative computation. It shows that useful collaborative learning does not always require every participant to exchange or process the full model.

### Useful points to cite
- Networks can be split at a cut layer.
- Activations and gradients can be exchanged instead of raw data.
- Client-side computation can be dramatically reduced.
- Communication trade-offs depend on the number of clients and the chosen setup.

### Similarity to my project
My project is also interested in whether collaboration can remain effective when only part of a network is involved in transfer or shared computation.

### Difference from my project
SplitNN focuses more on split execution and privacy-preserving collaboration, while my project is centered on adaptation under sequentially arriving data and communication-efficient transfer strategies.

---

## 3. Neurosurgeon: Collaborative Intelligence Between the Cloud and Mobile Edge

### Core problem
How should deep neural network computation be partitioned between edge devices and the cloud in order to improve efficiency?

### Main idea
Neurosurgeon analyzes the compute and transfer costs associated with different layers of a neural network and chooses partition points dynamically. The key insight is that different layers have very different cost profiles, so splitting the network at the right point can reduce latency or communication cost.

### Why it matters for this project
This paper gives conceptual support for the idea that not all parts of a network are equally expensive or equally sensible to transfer. That makes it useful as inspiration for selective neural-network transfer.

### Useful points to cite
- Layer choice matters.
- Compute cost and communication cost should be considered together.
- Partitioning decisions should be guided by system trade-offs, not treated as arbitrary.

### Similarity to my project
Both directions depend on the idea that partial network handling can be more efficient than treating the full network as a single indivisible block.

### Difference from my project
Neurosurgeon is not a federated or continual learning paper. It is mainly about inference partitioning between device and cloud, so it should be used as inspiration rather than as a direct baseline.

---

## 4. Federated Continual Learning: Concepts, Challenges, and Solutions

### Core problem
How can collaborative learning systems continue adapting when data is both distributed across clients and changing over time?

### Main idea
This survey brings together federated learning and continual learning. It presents Federated Continual Learning (FCL) as the setting where data is decentralized and non-stationary. It reviews challenges such as heterogeneity, model stability, communication overhead, privacy preservation, concept drift, and catastrophic forgetting.

### Why it matters for this project
This is the closest overall framing for my proposal. My project is also about collaborative learning under sequential data arrival, where both adaptation and communication matter.

### Useful points to cite
- FCL combines distributed collaboration with continual adaptation.
- Key challenges include heterogeneity, communication overhead, model stability, and catastrophic forgetting.
- Static-data assumptions from standard federated learning are not enough for streaming or evolving environments.

### Similarity to my project
My proposal fits naturally into the FCL problem space because it studies collaborative learning under sequential and potentially non-stationary data conditions.

### Difference from my project
The survey gives a broad taxonomy and challenge overview rather than proposing my exact selective-transfer method. My work would be one concrete strategy inside that broader problem area.

---

## Short synthesis for proposal use

Together, these four papers support the proposal from different angles:

- **McMahan et al. / FedAvg** gives the collaborative decentralized baseline.
- **SplitNN** shows that only part of a model may need to be involved in collaborative computation.
- **Neurosurgeon** supports the intuition that partition choice matters because layers differ in cost.
- **Federated Continual Learning survey** gives the closest high-level framing for distributed learning under sequential or non-stationary data.

## Honest positioning statement
My current repository does not yet implement a full federated continual learning system. What it does provide is preliminary sequential evidence that different transfer strategies can be compared in terms of both predictive performance and transfer cost. The literature above supports extending that work into a collaborative multi-client setup.
