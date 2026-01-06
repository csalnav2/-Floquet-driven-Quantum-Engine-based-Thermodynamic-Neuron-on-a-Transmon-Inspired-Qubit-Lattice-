# Floquet-based-transmon-like-lattice-based-Thermodynamic-neuron
This interactive model simulates a four‑qubit, transmon‑like lattice under thermally pulsed, Floquet driving (two‑level approximation). It functions as a Boolean linear classifier, passing NOT/NOR and three‑input majority tests, and provides live diagnostics with optional daemon‑driven control.
 This system is modeled as a simulation of a 4‑qubit, transmon‑inspired lattice model in the two‑level (qubit) approximation, evolving a full 16×16 global density matrix (so the system can genuinely entangle, not just “fake it” with independent single‑qubit states). The dynamics are open‑system and time‑dependent: each qubit is subjected to a periodic, pulsed square‑wave bath schedule (default period ≈ 4 seconds), which modulates the effective bath temperature and dissipation parameters. Because the drive/noise environment is periodic in time, the engine exhibits Floquet‑like stroboscopic behavior (i.e., the one‑period map can generate repeating structures and “quasi‑modes” in observables), and those periodic structures can show up as constructive/destructive patterns across multiple diagnostics.

The lattice itself includes two interaction channels:

J_cap (exchange‑like coupling ∼ XX + YY)

J_ind (Ising‑like coupling ∼ ZZ)
These are applied at the global Hamiltonian level, so the evolution can generate multi‑qubit correlations and pairwise entanglement inside the same 16×16 state.

To quantify and track what the system is doing, the dashboard monitors several families of signals:

State distance / “thermodynamic length” style diagnostics:
We track Bures‑geometry quantities in two ways:

a per‑qubit “TL” style trace based on Bures angle to a reference reduced state (so it behaves like “how far did this qubit drift from the reference?”), and

a global Bures‑speed style diagnostic (Bures distance step‑to‑step), plus a lagged Bures distance D_lag(t) to probe “how much the present differs from the past.”

Memory loss / hotspot logic (Bures‑lag separation current):
Using the lagged Bures distance D_lag(t), we define a “memory‑loss current” J_sep = max(0, d/dt D_lag). When J_sep spikes, the system is “separating from its own past” faster than usual, and we visualize those events as hotspots (including purple overlays on Bloch trajectories).

Coherence / QFI / purity / echo / T1–T2:
Coherence is tracked via |ρ01| on reduced qubits; QFI is computed as a sensitivity measure (generator‑based) on reduced states; purity is Tr(ρ²). The dashboard also tracks T1 (energy relaxation / spin‑lattice analogue) and T2 (dephasing / spin‑spin analogue) as instantaneous values derived from the local noise parameters. Echo is tracked via a fidelity‑like overlap with a reference reduced state.

Entanglement and correlations (computed from the global 16×16 state):
Entanglement is diagnosed by taking partial traces down to two‑qubit 4×4 reduced states for all 6 qubit pairs, then computing pairwise log‑negativity (LN12, LN13, LN14, LN23, LN24, LN34). We also compute concurrence on those same reduced 4×4 pair states as a second entanglement monotone.
For broader correlation structure, we additionally compute mutual information across the (01)|(23) bipartition, which captures total correlation (classical + quantum) between the two halves of the lattice.

Operator complexity / entanglement‑in‑operator‑space:
We track OSEE (Operator‑Space Entanglement Entropy) across the 2|2 cut as a proxy for “operator complexity / scrambling‑ish behavior” in the density‑operator viewed as a bipartite object.

Geometry probes (clearly labeled as proxies):
We include Berry‑rate, QGT‑metric, and path curvature proxies computed from Bloch‑trajectory geometry (not the fully rigorous mixed‑state/Uhlmann geometry). These are used as “geometric activity indicators” rather than strict geometric invariants.

Finally, this engine is intended as the “Soma” (body) of a Thermodynamic Neuron: a Boolean linear separator layer (perceptron‑equivalent) that can implement NOT, NOR, and 3‑MAJORITY. The code includes logic self‑tests showing those gates pass their truth tables under the perceptron rule; the lattice engine is the dynamical substrate whose observables can be mapped into that readout layer.

One more diagnostic worth mentioning: the transmon leakage proxy (Ω/|α|)² shown alongside entanglement traces is a risk indicator for leakage into |2⟩, |3⟩… in a real weakly anharmonic transmon. In this simulation the Hilbert space is still two‑level, so it’s not “measuring actual |2⟩ population”—it’s a “how hard are we driving relative to anharmonicity?” warning light.
