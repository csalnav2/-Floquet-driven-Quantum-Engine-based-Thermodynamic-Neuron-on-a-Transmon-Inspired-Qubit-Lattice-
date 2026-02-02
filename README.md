# Floquet-based Transmon-like Lattice: Thermodynamic Neuron

This interactive model simulates a four-qubit, **transmon-like lattice** under thermally pulsed, **Floquet driving** (two-level approximation). It functions as a **Boolean linear classifier**, passing NOT/NOR and three-input majority tests, and provides live diagnostics with optional daemon-driven control.

## 🌀 Dynamical Core
The system evolves a full $16 \times 16$ global density matrix $\rho(t)$, allowing for genuine multi-qubit entanglement. The dynamics are open-system and time-dependent: each qubit is subjected to a periodic, pulsed square-wave bath schedule (default period $\approx 4s$), modulating effective bath temperature and dissipation. 

As a **Floquet engine**, the system exhibits stroboscopic behavior where the one-period map generates repeating quasi-modes and constructive interference patterns across multiple diagnostics.

### Lattice Interactions:
* **$J_{\text{cap}}$:** Exchange-like coupling ($\sim XX + YY$).
* **$J_{\text{ind}}$:** Ising-like coupling ($\sim ZZ$).

---

## 📊 Diagnostic Suite

### 1. State Distance & Thermodynamic Length
* **Bures Geometry:** We track the Bures angle to a reference reduced state to measure qubit drift.
* **Memory Hotspots:** Using lagged Bures distance $D_{\text{lag}}(t)$, we define a **memory-loss current** $J_{\text{sep}} = \max(0, \frac{d}{dt} D_{\text{lag}})$. Spikes in $J_{\text{sep}}$ represent points where the system "separates from its own past," visualized as purple overlays on Bloch trajectories.

### 2. Information & Complexity
* **OSEE (Operator-Space Entanglement Entropy):** Tracked across the $2|2$ cut as a proxy for operator complexity and scrambling behavior.
* **QFI (Quantum Fisher Information):** Computed as a sensitivity measure on reduced states.
* **Entanglement:** Monitored via pairwise **Log-Negativity** ($E_N$) and **Concurrence** for all 6 qubit pairs.
* **Mutual Information:** Captures total (classical + quantum) correlations across the $(01)|(23)$ bipartition.

### 3. Geometric & Physical Probes
* **Geometric Activity:** Proxies for Berry-rate, Quantum Geometric Tensor (QGT) metric, and path curvature computed from Bloch trajectories.
* **Leakage Proxy:** A risk indicator $(\Omega/|\alpha|)^2$ monitors the drive-to-anharmonicity ratio, flagging potential leakage into non-computational states ($|2\rangle, |3\rangle$).

---

## 🧠 Thermodynamic Neuron (Boolean Logic)
The lattice serves as the dynamical substrate for a **Boolean linear separator** (perceptron-equivalent). It successfully implements:
* **NOT / NOR** gates.
* **3-Input Majority** tests.
The lattice observables are mapped into a readout layer that demonstrates these gates passing their respective truth tables under the perceptron rule.

---

## 📚 References

### Quantum Thermodynamics & Logic
* **Thermodynamic Computing:** Lipka‑Bartosik et al., [Science Advances 10(36), 2024](https://doi.org/10.1126/sciadv.adm8792)
* **Quantum Perceptrons:** Schuld et al., [Phys. Lett. A 378, 21 (2014)](https://doi.org/10.1016/j.physleta.2014.08.024)

### Information Geometry & Bures Metric
* **Bures Distance:** Uhlmann, [Rep. Math. Phys. 9, 273 (1976)](https://doi.org/10.1016/0034-4877(76)90060-4)
* **Thermodynamic Length:** Crooks, [Phys. Rev. Lett. 99, 100602 (2007)](https://doi.org/10.1103/PhysRevLett.99.100602)
* **Quantum Fisher Information:** Braunstein & Caves, [Phys. Rev. Lett. 72, 3439 (1994)](https://doi.org/10.1103/PhysRevLett.72.3439)

### Complexity & Entanglement
* **OSEE:** Prosen & Pižorn, [Phys. Rev. A 76, 032316 (2007)](https://doi.org/10.1103/PhysRevA.76.032316)
* **Log-Negativity:** Vidal & Werner, [Phys. Rev. A 65, 032314 (2002)](https://doi.org/10.1103/PhysRevA.65.032314)
* **Mutual Information in Lattices:** Wolf et al., [Phys. Rev. Lett. 100, 070502 (2008)](https://doi.org/10.1103/PhysRevLett.100.070502)

### Transmon & Floquet Physics
* **Transmon Theory:** Koch et al., [Phys. Rev. A 76, 042319 (2007)](https://doi.org/10.1103/PhysRevA.76.042319)
* **Floquet Dynamics:** Shirley, [Phys. Rev. 138, B979 (1965)](https://doi.org/10.1103/PhysRev.138.B979)
* **Quantum Geometric Tensor:** Provost & Vallee, [Commun. Math. Phys. 76, 289 (1980)](https://doi.org/10.1007/BF01197703)

---

> **Note:** This simulation utilizes the two-level approximation. The leakage proxy is a diagnostic tool for experimental feasibility and does not simulate a multi-level Hilbert space.
