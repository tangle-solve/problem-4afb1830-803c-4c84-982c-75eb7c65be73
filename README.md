<!-- solve-problem:4afb1830-803c-4c84-982c-75eb7c65be73 -->
# Efficient sampling near the graph-coloring uniqueness threshold

A [Tangle Solve problem](https://solve.tangle.tools/problems/4afb1830-803c-4c84-982c-75eb7c65be73).

## What is the problem?

Design a randomized algorithm that, given as input any graph G with maximum degree Δ and a palette of q colors, outputs a proper q-coloring of G distributed within total variation distance ε of the uniform distribution over all proper q-colorings of G, in time polynomial in |V(G)| and log(1/ε). The goal is to achieve this for **every** graph with maximum degree at most Δ, for all q ≥ Δ+2 (i.e., the palette is at least one color larger than the greedy/brute-force threshold Δ+1).

Formal setting: the state space Ω(G,q) is the set of proper vertex colorings σ: V → {1,…,q}; the target measure is uniform on Ω. Quality is measured in total variation distance. The degree bound Δ is a worst-case guarantee over graphs, not an average-case model. Statistical-physics framing: this is the zero-temperature (proper-coloring) antiferromagnetic q-state Potts model on worst-case bounded-degree graphs.

Ambiguity note: the census states q ≥ Δ+2, but the physics conjecture ("uniqueness threshold") is typically stated as uniqueness of the infinite-volume Gibbs measure on the (Δ−1)-regular tree, believed to hold for q ≥ Δ+1 (up to boundary cases); some versions of the algorithmic conjecture use q ≥ Δ+2 to avoid degenerate cases at q = Δ+1 (e.g., cliques of size Δ+1). Read "Δ+2" as the safe formulation; solving q ≥ Δ+1+γ for any constant γ, or q ≥ Δ+1 for all Δ ≥ some Δ₀, counts as major progress.

## Why does it matter?

This is the flagship open problem in the "algorithmic threshold = physics threshold" program: it would confirm that Gibbs uniqueness, a condition from statistical mechanics, exactly characterizes when worst-case approximate counting/sampling becomes tractable. It would yield an FPRAS for counting q-colorings on all bounded-degree graphs down to the natural threshold, settle the complexity-theoretic dividing line (below it, sampling is NP-hard on some Δ-regular graphs), and provide the template technique likely to transfer to other antiferromagnetic spin systems (hard-core, general Potts) where the same conjecture is open.

## What's already known?

- **Uniqueness side (physics):** On the (Δ−1)-regular tree, uniqueness of the Gibbs measure for proper q-colorings is known to hold for q ≥ Δ+1 (for large enough q relative to Δ; exact boundary cases are subtle (verify)). This is the conjectural algorithmic threshold.
- **Hardness side:** For q ≤ Δ (and q = Δ+1 on some graphs, e.g., cliques/amalgamations), sampling/counting is NP-hard or the mixing is exponentially slow on worst-case instances. So the interesting window is essentially q = Δ+1 and Δ+2 only.
- **Algorithms:** Vigoda's 2000 coupling-of-chains analysis of a single-site update chain gives sampling for q ≥ (11/6)Δ, the first bound linear in Δ with constant < 2. Subsequent refinements of block dynamics / local-to-global arguments have pushed the constant down; the census cites a ~1.809Δ benchmark by 2026 (Chen–Moi–Yin-type results (verify); the census attributes the ~1.809Δ line to the 2026 literature on unconstrained colorings). All current methods leave a gap of roughly 0.8Δ colors between the algorithmic bound and the conjectured Δ+2 threshold.
- **Special cases solved:** triangle-free graphs (q ≥ (1+ε)Δ via combinatorial methods of Bhatt–Hayes–etc. (verify)), large-girth graphs (via spectral independence on random-ish instances), and certain structured families. These fall short of all graphs precisely because of local structures like cliques and dense neighborhoods that break spatial-mixing-by-induction arguments.
- **Key modern machinery:** spectral independence and the local-to-global contraction framework (Barvinok, Anari–Oveis Gharan–Rezaei; Chen–Liu–Vigoda) can in principle give fast mixing for q ≥ CΔ with C provable but > 1; the obstruction is bounding the spectral norm of the influence matrix for worst-case graphs near q ≈ Δ.

## What would count as solving it?

1. **Full solution (algorithm):** An FPTAS/FPRAS-style randomized algorithm sampling uniform q-colorings of every max-degree-Δ graph within TV distance ε in poly(n, log 1/ε) time for all q ≥ Δ+2 (all Δ ≥ 3; q = Δ+2 and beyond). Equivalently, a proof that Glauber/block dynamics mix in poly(n) time at q = Δ+2 on all such graphs.
2. **Partial but accepted progress:** pushing the algorithmic constant below 1.809Δ toward Δ (e.g., q ≥ (1+c)Δ for small c), or resolving q ≥ Δ+2 for all triangle-free / girth-girth-conditioned graphs.
3. **Lower-bound/dichotomy variant:** a proof that at q = Δ+1 (or q ≤ Δ) worst-case sampling is NP-hard, matching the threshold from below — combined with (1) this gives the full algorithmic/complexity dichotomy.
4. **Negative variant:** an explicit max-degree-Δ graph family and q ≥ Δ+2 exhibiting exponential mixing time for all local chains, refuting the conjecture.

## How to start

1. Read Vigoda (2000) to master the coupling/path-coupling analysis of the flip chain; understand exactly where the 11/6 arises (the worst-case discrepancy in color-class counts between neighbors).
2. Read the spectral-independence literature: Chen–Liu–Vigoda (FOCS 2021) and Chen–Moi–Yin's coloring analysis — this is the machinery behind the ~1.809Δ bound. The task is to bound pairwise influences μ(σ(u)=c | σ(v)=c′) for worst-case graphs; near q = Δ these influence matrices are barely contractive.
3. Identify the extremal obstructions: (Δ+1)-cliques, "amalgamated" graphs, and dense neighborhoods where uniqueness barely holds. Any successful analysis must be tight on these instances — a good sanity check for candidate techniques.
4. Natural approaches: (a) two-block / clique-block dynamics that update neighborhoods jointly to sidestep single-site bottlenecks; (b) combine spectral independence with a clever choice of field/degree reduction; (c) push the Kuchukova–Perkins–Povill fixed-color-class-size analysis (which handles constrained marginals) to the unconstrained case. Main obstacle: local-to-global arguments degrade exactly when q − Δ is a small constant.

## Key references

- Vigoda, E. (2000). "Improved bounds for sampling colorings." Journal of Mathematical Physics 41(3): 1555–1569. — Analyzes a heat-bath "flip chain" via coupling, proving O(n log n) mixing for q > (11/6)Δ, the long-standing benchmark.
- Kuchukova, Y., Perkins, W., Povill, G. (2026). "Sampling colorings with fixed color class sizes," arXiv:2603.08259 (per census; verify). — Studies sampling q-colorings under prescribed color-class sizes, a constrained variant closer to the physics uniqueness analysis.
- Chen, X., Liu, Y.-T., Vigoda, E. (2021). "Optimal mixing of Glauber dynamics: entropy factorization via high-dimensional expansion." — Establishes the spectral-independence toolkit (with Chen, Eldan, Guo, et al.) underlying the current best constants for colorings. (Verify exact attribution of the ~1.809Δ bound; likely Chen–Moi–Yin, "Sampling colorings with almost linear potential," 2021+.)
- Anari, N., Oveis Gharan, S., Rezaei, A. (2016+). Work on the geometry of distributions and log-concave polynomials — background for influence-matrix/entropy methods. (verify)

## Warnings

- The conjectured threshold is q ≥ Δ+1 in most physics statements; the census's Δ+2 sidesteps boundary subtleties. State precisely which version you attack.
- Do not conflate worst-case-all-graphs with typical/bounded-girth results: many published sampling results hold only for triangle-free or large-girth graphs and do not transfer.
- The 2026 arXiv identifier (2603.08259) and the 1.809Δ figure come from a census record and must be verified against the actual literature before relying on them.
- Path coupling with naive metrics provably cannot beat constants around 11/6; new analyses need block dynamics or spectral methods. Also beware: fast mixing of some specific chain does not automatically give an efficient sampler unless the chain's steps are implementable.

---
*`ALGO-000142` · unverified*

## Acceptance guidance

1. **Full solution (algorithm):** An FPTAS/FPRAS-style randomized algorithm sampling uniform q-colorings of every max-degree-Δ graph within TV distance ε in poly(n, log 1/ε) time for all q ≥ Δ+2 (all Δ ≥ 3; q = Δ+2 and beyond). Equivalently, a proof that Glauber/block dynamics mix in poly(n) time at q = Δ+2 on all such graphs.
2. **Partial but accepted progress:** pushing the algorithmic constant below 1.809Δ toward Δ (e.g., q ≥ (1+c)Δ for small c), or resolving q ≥ Δ+2 for all triangle-free / girth-girth-conditioned graphs.
3. **Lower-bound/dichotomy variant:** a proof that at q = Δ+1 (or q ≤ Δ) worst-case sampling is NP-hard, matching the threshold from below — combined with (1) this gives the full algorithmic/complexity dichotomy.
4. **Negative variant:** an explicit max-degree-Δ graph family and q ≥ Δ+2 exhibiting exponential mixing time for all local chains, refuting the conjecture.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/4afb1830-803c-4c84-982c-75eb7c65be73?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000142.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000142)
