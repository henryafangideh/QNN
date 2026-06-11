## Summary: Iterative-$\alpha$ Pauli Correlation Encoding for Optimized Quantum Neural Networks (I$\alpha$PCE-QNN)

This notebook introduces and implements an **Iterative-$\alpha$ Pauli Correlation Encoding (I$\alpha$PCE)** scheme to enhance the state-preparation and readout stages of a hybrid Quantum Neural Network (QNN).

### Problem Addressed
Most variational quantum models suffer from high qubit requirements, often assuming a one-to-one mapping between problem variables and qubits. Pauli Correlation Encoding (PCE) offers a solution by mapping classical variables to $k$-body Pauli strings, leading to polynomial compression of qubit requirements. This work extends PCE to Quantum Machine Learning (QML).

### Contributions
1.  **PCE-guided state preparation:** Efficiently injects $m$ classical features into $n = \mathcal{O}(m^{1/k})$ qubits using Pauli-string rotations.
2.  **PCE readout:** Extracts a hidden representation ($z_i = \tanh(\alpha\langle\Pi_i\rangle)$) from the same Pauli strings.
3.  **Iterative-$\alpha$ training (I$\alpha$ part):** Anneals the sharpness parameter $\alpha$ over training stages ($\alpha_1 < \alpha_2 < \dots < \alpha_T$), warm-starting each stage from the previous optimum. This helps in navigating the loss landscape effectively.

### Key Findings
*   **Qubit Compression:** The method achieves significant qubit compression. For example, 12 classical features were embedded into just 4 qubits (a 3x reduction compared to one-to-one encoding). This compression rate improves polynomially with more features.
*   **Trainability in the Sharp Regime:** The iterative-$\alpha$ schedule is crucial for training in the saturating regime (where $\alpha$ approaches the hard sign decoder). While a fixed moderate $\alpha$ performs well, fixed large $\alpha$ leads to a flat, untrainable loss landscape. I$\alpha$PCE recovers high accuracy in this challenging regime (e.g., ~0.95 vs ~0.82 mean test accuracy).
*   **Problem-Independent Circuit:** The circuit architecture (Hardware-Efficient Ansatz) is problem-independent, offering a practical advantage on noisy quantum hardware.

### Logic Flow
```
 features x \in R^m ──► assign Pauli strings \Pi_1..\Pi_m over n = O(m^{1/k}) qubits
        │
        ▼
 STATE PREPARATION:  |\psi(x,\theta)\rangle = HEA(\theta) \cdot \prod_i \exp(-i x_i \beta \Pi_i) \cdot |init\rangle
        │
        ▼
 READOUT:            z_i = tanh(\alpha \langle\psi|\Pi_i|\psi\rangle)        (m correlation features)
        │
        ▼
 CLASSICAL HEAD:     \hat{y} = \sigma(w\cdot z + b)                  (logistic layer)
        │
        ▼
 ITERATIVE-\alpha LOOP:   for \alpha in [\alpha_1 < \alpha_2 < ... < \alpha_T]: re-optimize \theta (warm start)
```

### Future Work
Future directions include shot-based estimation of correlations, multi-class PCE readouts, higher-order ($k=3$) encodings for high-dimensional data, and validation on real quantum hardware.
