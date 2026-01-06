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

Pushback / “meet you in the middle” calibration
1) The temperature units in your writeup are the biggest physics pothole

You wrote “11 to 20 Kelvin.” For transmon‑type superconducting qubits, that would be… the qubit funeral.
What your code actually uses by default is 110–200 mK (0.110–0.200 K), controlled by:

--T_bath_base_mK 110

--T_bath_hot_mK 200

If you meant 11–20 mK, that’s also a realistic fridge regime, but it’s not what the current defaults are. So the safest fix is: say 110–200 mK (or 0.11–0.20 K) unless you change the CLI defaults.

2) “GKSL master equation” vs what the code literally does

Your simulation is GKSL‑like / Lindblad‑inspired, but you are not explicitly integrating a continuous‑time master equation in differential form. You’re doing a stroboscopic map each step:

coherent unitary from H(t)

local CPTP channels (dephasing + amp damping + excitation) applied via Kraus form

That’s totally legit as an approximation, but the clean language is:

“Trotterized unitary + local Kraus channels (Markovian / GKSL‑inspired).”

3) “Floquet modes” is defensible if you say “Floquet‑like (open system)”

Floquet theory is cleanest for unitary periodic Hamiltonians. You have periodic dissipation and drive, so it’s more accurate to say:

“Floquet‑like stroboscopic dynamics / periodic CPTP map / Floquet‑Lindblad‑style behavior.”

4) Geometry: your code uses proxies (good!) — your text should say “proxies” too

Your docstring already admits the truth: mixed‑state Berry/Uhlmann/QGT is subtle. So in the writeup, keep the ambition, but add the guardrails:

“Berry‑rate / QGT / curvature proxies derived from Bloch‑trajectory geometry.”

5) Leakage proxy ≠ leakage population (in a 2‑level sim)

You can absolutely say it’s a transmon leakage risk proxy.
You can’t say it “shows population in |2⟩” because the model literally cannot populate |2⟩.

6) “Mutual information between pairs” vs what you compute

In the dashboard you compute MI(01 : 23) (a bipartition), not MI for every pair. Also: MI measures total correlation, not purely entanglement.

7) “Thermodynamic neuron passes gates”

The code does include the perceptron truth‑table self‑tests.
But it does not (yet) prove that the transmon lattice dynamics are producing the correct inputs/thresholding to implement those gates end‑to‑end. So the honest middle ground is:

“The readout layer (perceptron) passes NOT/NOR/3‑MAJORITY tests; integrating lattice observables into the readout is the intended thermodynamic‑neuron pipeline.”

Command line recipes (daemon + parameter modulation)

Use your actual filename:

python quantum_unified_revised_v68.py --mode lattice --bath_enable

Fast run (no Wigner, fewer frames, quicker output)
python quantum_unified_revised_v68.py \
  --mode lattice --bath_enable \
  --no_wigner \
  --render_skip 4 \
  --q_tmax 10 --dt 0.02 \
  --outdir outputs

Enable the daemon (SPSA “GRAPE‑ish” pulse shaping)
python quantum_unified_revised_v68.py \
  --mode lattice --bath_enable \
  --daemon \
  --daemon_iters 10 \
  --daemon_pulses 20 \
  --daemon_seed 0

Tune the bath temperature window (IMPORTANT: these are milliKelvin)

Example: 20–120 mK:

python quantum_unified_revised_v68.py \
  --mode lattice --bath_enable \
  --T_bath_base_mK 20 \
  --T_bath_hot_mK 120

Tune couplings (J_cap / J_ind) and entanglement visibility

Your code has an ent_boost knob that scales couplings inside H to keep LN visible under dissipation:

python quantum_unified_revised_v68.py \
  --mode lattice --bath_enable \
  --J_cap 0.10 --J_ind 0.06 \
  --ent_boost 12


Optional periodic entanglement pumping via bath waveform:

python quantum_unified_revised_v68.py \
  --mode lattice --bath_enable \
  --ent_boost 10 \
  --ent_pulse_amp 0.4

Transmon proxy knobs
python quantum_unified_revised_v68.py \
  --mode lattice --bath_enable \
  --transmon_EC 0.20 \
  --transmon_flux_amp 0.12


Disable bath‑driven EJ modulation (still computes diagnostics):

python quantum_unified_revised_v68.py \
  --mode lattice --bath_enable \
  --transmon_flux_no_bath

Quick sanity test: entanglement tools unit tests
python quantum_unified_revised_v68.py --ent_tests

“Single” mode (prints final scalar summaries, no coupling)
python quantum_unified_revised_v68.py --mode single --bath_enable

Output / rendering control

Disable MP4 conversion:

python quantum_unified_revised_v68.py --mode lattice --bath_enable --no_mp4


Render stability (respect Ctrl+C instead of ignoring SIGINT):

python quantum_unified_revised_v68.py --mode lattice --bath_enable --respect_sigint


Export plot‑data bundles (on by default, but explicit):

python quantum_unified_revised_v68.py --mode lattice --bath_enable --export_plot_data

“Provide GitHub” — what I can do (and a ready repo skeleton)

I can’t create the repo for you directly from here, but I can hand you a clean GitHub‑ready structure + a README you can paste, and the exact git commands to publish.

Suggested repo name

transmon-lattice-dashboard

thermodynamic-neuron-transmon-lattice

floquet-open-quantum-neuron
