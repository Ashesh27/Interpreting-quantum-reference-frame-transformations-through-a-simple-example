# Quantum reference frames: the Z₂ example, and a few extensions

Numerical notes on the three-qubit example in

> E. Castro-Ruiz, T. D. Galley and L. Loveridge, *Interpreting quantum-reference-frame transformations through a simple example*, Phys. Rev. A **114**, 032201 (2026), DOI 10.1103/czr8-dv1l.

The notebook `QRF_Z2_example_final.ipynb` reproduces the paper's calculations, implements the maps the paper only describes, and explores a few extensions. Every claim in the text is backed by a recorded numerical check (76 in total, summarised at the end). The paper itself is **not** included here; please read it at the link above.

**Status.** This is a learning and preparation project by Ashesh Kumar Gupta. The extensions are exploratory. 

---

## The idea in plain language

Imagine three qubits, A, B and C, and no outside reference: all that exists is how the qubits relate to each other. Alice uses qubit A as her reference frame and describes B and C relative to A. Bob uses B and describes A and C relative to B.

The paper builds two global states, ψ₀ and ψ₁, that behave differently:

- **Relative to Alice they look identical.** [No measurement she can make (using A as her frame) distinguishes them.]
- **Relative to Bob they are orthogonal.** [He can tell them apart perfectly.]

So if Alice knows only her own description, she cannot work out Bob's. What, then, can "changing from Alice's frame to Bob's" mean? The paper compares three answers from the literature:

| Approach | Its answer |
|---|---|
| **Perspective-neutral** | Only allow global states in one symmetry sector (H₀). Then Alice's description fixes the global state, and the change of frame is a unitary. |
| **Extra-particle** | Keep both sectors, but give Alice an extra system that records which sector the global state is in (the global charge). With it, the change of frame is again a unitary. |
| **Operational** | Alice and Bob may use only what their frames can measure. States they cannot tell apart are identified, so information is discarded and the map is only defined on equivalence classes. |

The global charge G = X⊗X⊗X is what separates ψ₀ (+1) from ψ₁ (−1). It is invariant under the symmetry, but it is not an observable relative to Alice. Whether it counts as accessible is exactly where the three approaches differ.

---

## What the notebook shows

Section labels: **[paper]** reproduces a result stated in the PRA paper; **[implemented]** builds a map the paper only describes; **[exploratory]** my own extension; **[preprint]** reproduces a result from a different paper.

| § | Content | Status |
|---|---|---|
| 0–1 | Setup, the two global states, reduction maps R₀|A and R₀|B (Eqs. 12–17): identical states for Alice, orthogonal for Bob; R₀|A is invertible only inside one charge sector | [paper] |
| 2 | Relative observables Φ^A, Φ^B (Appendix A): none relative to Alice separates ψ₀ from ψ₁, some relative to Bob does | [paper] |
| 3 | The charge observable is not a relative observable; coherent vs incoherent group averaging; Alice's "equal probability" mixture (Eq. 31) is the incoherent twirl of a superposition | [paper] + illustration |
| 4 | Extra-particle change of frame built explicitly: disentanglers T^A, T^B, controlled unitaries U±, S_{A→B} (Eqs. 24–27); S_{A→B} = SWAP·T^B(T^A)†; tested on 50 random mixed states; U₊ equals the perspective-neutral map | [implemented] |
| 5 | Operational approach: framed observables, the map Q of Eq. (B12) is its own inverse on framed states (100 random states) | [paper] |
| 6 | Algebra dimensions that make Table I concrete: invariant algebra 32, Alice's relative algebra 16, with the charge 32, restricted to H₀ 16; Appendix C shared observables 8 | [implemented] |
| 7 | Z_N generalisation: all N charge sectors give the same Alice state; Bob's states differ by a phase twist; ⟨Ψ_p^B\|Ψ_q^B⟩ = Tr(ρ_A Z^{p−q}), so they are orthogonal exactly when ρ_A has a uniform diagonal | [exploratory] |
| 8 | Non-ideal U(1) frames: generated-algebra dimensions 6 (>4) and 7 (<9); a relative phase invisible to single relational operators but visible in products | [preprint] (Garmier, Hausmann, Castro-Ruiz, arXiv:2512.19343, Example 5) |
| 9 | State discrimination: Alice's posterior equals her prior; Helstrom success probability max(p₀,p₁) for Alice and 1 for Bob. Mostly a restatement of Eq. (31) | [exploratory] |
| 10 | Numerical probe of whether a non-ideal frame can resolve different κ-sectors. Result: mixed | [exploratory] |
| 11 | An exact criterion: a single relational measurement sees an effective state in K(r) = span{\|s⟩ : s ∈ κ(r)}. Two charges are always distinguishable if their κ-sets are disjoint and can be indistinguishable if they overlap | [exploratory] |

### Notes on the paper's equations

- Eq. (B19)/(B20) are printed with |1⟩⟨1|_C in both terms. My calculation gives ½(|0⟩⟨0|_A⊗|1⟩⟨1|_C + |1⟩⟨1|_A⊗|0⟩⟨0|_C), which is consistent with Eq. (31). This looks like a small typographical slip that does not affect the paper's results.
- In Eq. (26) the control on the extra particle is written with |0⟩⟨0| and |1⟩⟨1| (trivial and sign irrep); I write the extra particle's two charge eigenstates as |±⟩, as produced by T^A. Same control, different labels.

---

## Limits

- The extensions in §7, §9–11 are exploratory. §9 adds little beyond Eq. (31).
- §11 assumes pure global states of definite total charge and the rank-one, equal-weight orientation POVM used in the notebook. Mixed hypotheses and priors are not covered, and the statement about when a sector projector lies in the span of single relational operators is a **numerical observation, not a proof**.
- Checks are numerical. The extreme values in §11 follow from the analytic argument. The code checks representative constructions, not an optimisation over all states.
- Only Abelian groups (Z₂, Z_N, U(1)) are treated.
- I have not done a literature search for novelty on §7, §9 or §11.

Possible next steps: mixed hypotheses with priors (Helstrom bound for several hypotheses), a proof of the span observation in §11, and a non-Abelian example.

---

## Running it

```bash
python -m venv .venv && source .venv/bin/activate   # optional
pip install -r requirements.txt
jupyter notebook QRF_Z2_example_final.ipynb
```

Tested with Python 3.12, NumPy 2.5 and Matplotlib 3.11. "Restart & Run All" takes a few seconds and ends with `76 / 76 checks passed`. Only failures are printed, and each section ends with a one-line report. All random numbers are seeded.

### Conventions

- Qubits are ordered (A, B, C). The global action is G = X_A⊗X_B⊗X_C; Π± = (1±G)/2 project onto H₀ and H₁.
- R₀|F|ψ⟩ = √2 ⟨0|_F|ψ⟩; the two remaining qubits keep their order.
- Z_N shift S|j⟩ = |j+1⟩ and ω = e^{2πi/N}.

---

## References

1. E. Castro-Ruiz, T. D. Galley, L. Loveridge, Phys. Rev. A **114**, 032201 (2026).
2. S. Garmier, L. Hausmann, E. Castro-Ruiz, *The Perspectives of Non-Ideal Quantum Reference Frames*, arXiv:2512.19343.
3. E. Castro-Ruiz, O. Oreshkov, *Relative subsystems and quantum reference frame transformations*, Commun. Phys. **8**, 187 (2025).
4. T. Carette, J. Głowacki, L. Loveridge, *Operational quantum reference frame transformations*, Quantum **9**, 1680 (2025).
5. A.-C. de la Hamette, V. Kabel, C. Brukner, *Accessibility of global properties from internal quantum reference frame perspectives*, arXiv:2510.09100.
6. G. Doat, A. Vanrietvelde, *What can we do in a symmetry-constrained perspective?*, Quantum **10**, 2126.

## License

Code: MIT (add a `LICENSE` file before publishing). The cited papers remain under their own licences.
