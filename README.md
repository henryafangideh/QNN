# Iterative-Alpha Pauli Correlation Encoding for Optimized Quantum Neural Networks (IαPCE-QNN)

This notebook implements **Iterative-Alpha Pauli Correlation Encoding (IαPCE)**, a quantum feature encoding and readout framework designed to improve the state-preparation and measurement stages of hybrid Quantum Neural Networks (QNNs).

## Overview

Most variational quantum models assume a one-to-one mapping between classical features and qubits, which limits scalability as input dimensions grow. **Pauli Correlation Encoding (PCE)** addresses this challenge by representing classical variables through *k-body Pauli strings*, enabling polynomial compression of qubit requirements.

This project extends PCE to Quantum Machine Learning (QML) and introduces an **iterative-alpha training strategy** that improves optimization in highly nonlinear decoding regimes.

---

## Key Contributions

### 1. PCE-Guided State Preparation
Efficiently embeds **m classical features** into

\[
n = \mathcal{O}(m^{1/k})
\]

qubits using parameterized Pauli-string rotations.

### 2. PCE-Based Readout
Extracts hidden representations from the same Pauli observables:

\[
z_i = \tanh(\alpha \langle \Pi_i \rangle)
\]

allowing feature compression and correlation-aware measurement.

### 3. Iterative-Alpha Training (IαPCE)
Instead of training with a fixed decoder sharpness parameter, the model progressively increases

\[
\alpha_1 < \alpha_2 < \cdots < \alpha_T
\]

while warm-starting each optimization stage from the previous solution. This curriculum-style approach improves trainability and convergence.

---

## Results

### Qubit Compression

The proposed encoding significantly reduces qubit requirements.

Example:

- 12 classical features
- 4 qubits required
- Approximately **3× compression** compared to one-feature-per-qubit encoding

The compression advantage grows polynomially with increasing feature dimensions.

### Improved Trainability

Training with a large fixed alpha leads to a highly saturated loss landscape and poor optimization performance.

The iterative-alpha schedule successfully navigates this regime and achieves substantially better results:

| Method | Mean Test Accuracy |
|----------|----------|
| Fixed Large Alpha | ~0.82 |
| IαPCE | ~0.95 |

### Hardware-Friendly Architecture

The approach uses a standard **Hardware-Efficient Ansatz (HEA)** and is independent of the specific learning task, making it suitable for noisy intermediate-scale quantum (NISQ) devices.

---

## Workflow

```text
Input Features x ∈ ℝᵐ
        │
        ▼
Assign Pauli Strings Π₁ ... Πₘ
over n = O(m^(1/k)) qubits
        │
        ▼
State Preparation
|ψ(x,θ)⟩ = HEA(θ) · ∏ᵢ exp(-i xᵢ β Πᵢ) · |init⟩
        │
        ▼
Readout
zᵢ = tanh(α⟨ψ|Πᵢ|ψ⟩)
        │
        ▼
Classical Head
ŷ = σ(w·z + b)
        │
        ▼
Iterative-Alpha Optimization
α₁ < α₂ < ... < αₜ
(warm-started training)
```

---

## Future Work

Potential directions for further development include:

- Shot-based estimation of Pauli correlations
- Multi-class PCE readout architectures
- Higher-order encodings (k = 3 and beyond)
- Large-scale, high-dimensional datasets
- Deployment and validation on real quantum hardware
- Robustness analysis under realistic noise models

---

## Citation

If you use this implementation in your research, please cite the associated paper or repository. 
##[LinkedIn](https://www.linkedin.com/in/aniekanafangideh/) 
##[X](https://x.com/aniekan_Q)
