<!-- solve-problem:bcaf5c67-779c-47e9-8d36-120f6357b8cf -->
# Majority outside ACC⁰

A [Tangle Solve problem](https://solve.tangle.tools/problems/bcaf5c67-779c-47e9-8d36-120f6357b8cf).

## What is the problem?

Prove that the majority function MAJ_n : {0,1}^n → {0,1} (output 1 iff at least n/2 inputs are 1) is not computable by polynomial-size ACC⁰ circuits.

**Formal setting.** An ACC⁰[s,m,d] circuit is a depth-d circuit (d = O(1)) of unbounded-fan-in AND, OR, NOT gates and MOD_m gates, where MOD_m outputs 1 iff the sum of its inputs is ≢ 0 (mod m). The modulus m and depth d are fixed constants; the size (number of gates) may grow as poly(n). **Conjecture:** for every fixed d ≥ 1, m ≥ 2, and constant c, there exists n₀ such that no depth-d ACC⁰[s,m,d] circuit with s ≤ n^c computes MAJ_n for all n ≥ n₀.

Note this is a *function-specific* lower bound, strictly weaker than separating NEXP (or even ENP) from ACC⁰, yet it has resisted proof. Smolensky's work shows MOD_q ∉ ACC⁰[m] for distinct primes q ∤ m, and gives exponential lower bounds for MAJ against AC⁰[p] (depth-2 or restricted depth), but the general ACC⁰-vs-MAJ question is open.

## Why does it matter?

Majority is the canonical "global counting" primitive; proving it lies outside ACC⁰ would cleanly separate approximate/majority-type computation from constant-depth computation with modular gates — the natural completion of the AC⁰ < ACC⁰ < TC⁰ hierarchy at the function level. It is a benchmark for lower-bound techniques: our inability to prove it exposes the limits of current methods (Smolensky-style approximation and Williams's algorithmic method both fall short here). It also bears on threshold-vs-modular gate expressiveness questions arising in neural network and circuit modeling.

## What's already known?

- **AC⁰:** MAJ requires 2^{Ω(n^{1/(d-1)})} size in depth-d AC⁰ (Furst–Saxe–Sipser; Håstad's switching lemma). Settled.
- **AC⁰[p] (prime moduli):** Smolensky proved MAJ ∉ AC⁰[p] (exponential lower bounds via low-degree polynomial approximation over finite fields). Settled for prime power moduli.
- **Composite moduli / mixed moduli:** For MOD_q vs MOD_m with q having a prime factor not dividing m, Smolensky separates. But MAJ vs general ACC⁰ (composite m, arbitrary depth, AND/OR mixed in) is open. Known partial results: lower bounds for depth-2 or symmetric-function subcircuits; results showing ACC⁰ circuits computing MAJ or MOD must have specific structural properties (verify current best depth-3 status — active area).
- **Williams (2011):** NEXP ⊄ ACC⁰, via the algorithmic method (fast ACC⁰-SAT algorithms ⇒ lower bounds). This proves a class separation but yields nothing about specific functions like MAJ — the method needs the function to encode computation, and MAJ is "too easy" to trigger it.

The gap: Smolensky's polynomial approximation technique degrades badly when AND/OR gates are interleaved with MOD_m gates for composite m; no inductive hardness measure survives the composition.

## What would count as solving it?

A proof that for all constants d, m, c, MAJ_n requires superpolynomial-size depth-d ACC⁰[m] circuits. Stronger acceptable forms: a 2^{n^ε} lower bound, or even depth-3 ACC⁰ lower bounds for MAJ with arbitrary modulus would be major (verify what's known for depth 3 before claiming novelty). Partial targets, in increasing difficulty:
1. MAJ ∉ ACC⁰[m] for some specific composite m (e.g., m = 6) at any fixed depth.
2. Superpolynomial lower bound for depth-3 ACC⁰ with mixed AND/OR/MOD_m.
3. The full statement for all fixed m, d.

A referee would accept: a rigorous size hierarchy theorem of the form "any depth-d ACC⁰[m] circuit computing MAJ_n has size ≥ f(n) with f superpolynomial," for stated ranges of m, d.

## How to start

1. **Read Smolensky's lower-bound framework first** (via the Jukna textbook, *Boolean Function Complexity*, chapters on polynomial approximation and AC⁰[p] lower bounds). Understand exactly where the argument breaks for composite moduli: MOD_6 gates require polynomial representations over Z (not a field), and degree blows up.
2. **Read Williams (2011)** to understand why the algorithmic method gives class separations but not function-specific ones; consider whether an "easy function" variant exists (e.g., combining MAJ with a hardness-amplifying gadget).
3. **Try the depth-2 and depth-3 cases explicitly.** A depth-2 ACC⁰ circuit is a SYM∘MOD or MOD∘AND-type circuit; known results (e.g., on symmetric circuits for MAJ — verify Beigel–Tarui-type results) may extend. The natural obstacle is that MAJ is well-approximated by low-degree polynomials over Z with small weights, killing degree-based measures.
4. **Investigate communication-complexity / lifting approaches**: MAJ has low randomized communication complexity in most partitions, so standard lifting fails — this itself is worth understanding as the core obstacle.

## Key references

- M. L. Furst, J. B. Saxe, M. Sipser, "Parity, Circuits, and the Polynomial-Time Hierarchy" ( Mathematical Systems Theory, 1984). First parity/AC⁰ lower bounds via random restrictions. No stable link; standard.
- J. Håstad, "Computational Limitations of Small-Depth Circuits" (MIT Press, 1987). Optimal exponential AC⁰ lower bounds via the switching lemma; implies MAJ ∉ AC⁰.
- R. Smolensky, "Algebraic Methods in the Theory of Lower Bounds for Boolean Circuit Complexity" (STOC 1987). Proves MAJ, MOD_q ∉ AC⁰[p] for q ≠ p via polynomial approximation over finite fields. https://dl.acm.org/doi/10.1145/28395.28428
- R. Williams, "Non-uniform ACC Circuit Lower Bounds" (CCC 2011). Proves NEXP ⊄ ACC⁰ via fast ACC⁰-SAT algorithms; the algorithmic method. https://dl.acm.org/doi/10.1145/2189776.2189796
- S. Jukna, *Boolean Function Complexity: Advances and Frontiers* (Springer, 2012). Textbook synthesis of AC⁰[p]/ACC⁰ lower-bound techniques. https://doi.org/10.1007/978-3-642-24508-4
- W. M. Hoza, lecture notes on circuit complexity (course notes covering ACC⁰ lower bounds; referenced in census record). (verify availability)

## Warnings

- **Do not conflate AC⁰[p] with ACC⁰.** Smolensky's bounds cover prime (and prime-power) moduli without interleaved AND/OR of arbitrary depth; claiming them for general ACC⁰ is the classic error.
- **MAJ has low-degree integer polynomial approximations** (small-weight, ~√n degree), so naive polynomial-method attacks are provably insufficient — know why before attempting one.
- Williams's theorem does NOT imply MAJ ∉ ACC⁰; NEXP separations are non-constructive about the witnessing function.
- Check recent literature (depth-3 ACC⁰ lower bounds, e.g., works of Chattopadhyay, Mande, Srinivasan and others on depth-3/4 MOD circuits) before claiming a partial result is new.

---
*`ALGO-000036` · unverified*

## Acceptance guidance

A proof that for all constants d, m, c, MAJ_n requires superpolynomial-size depth-d ACC⁰[m] circuits. Stronger acceptable forms: a 2^{n^ε} lower bound, or even depth-3 ACC⁰ lower bounds for MAJ with arbitrary modulus would be major (verify what's known for depth 3 before claiming novelty). Partial targets, in increasing difficulty:
1. MAJ ∉ ACC⁰[m] for some specific composite m (e.g., m = 6) at any fixed depth.
2. Superpolynomial lower bound for depth-3 ACC⁰ with mixed AND/OR/MOD_m.
3. The full statement for all fixed m, d.

A referee would accept: a rigorous size hierarchy theorem of the form "any depth-d ACC⁰[m] circuit computing MAJ_n has size ≥ f(n) with f superpolynomial," for stated ranges of m, d.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/bcaf5c67-779c-47e9-8d36-120f6357b8cf?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000036.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000036)
