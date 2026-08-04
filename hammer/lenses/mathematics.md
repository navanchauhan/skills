# Mathematics Lens

**Best solution:** a complete rigorous derivation with no unresolved dependencies.

## Require
- Exact definitions and quantified statement.
- Independent proof families when plausible.
- Explicit lemmas and dependency graph.
- Counterexample search for proposed sublemmas.
- Edge, boundary, degenerate, disconnected, multiplicity, and representation cases.
- Clear separation of proof, heuristic, computation, and citation.
- `closed-world-first` evidence policy when originality is claimed: independent derivation before literature review, retrieval-shaped routes labeled `web-informed`.

## Reject
- Reductions ending in an unproved claim equivalent in strength to the original task.
- Computation presented as proof unless permitted.
- Hidden circular reasoning or silent strengthening of assumptions.

## Completion checks
- Every lemma is proved or permitted by a precise citation.
- Every quantified case is covered.
- The final derivation survives independent adversarial audit.

## Contract fields
```yaml
statement: ""               # exact quantified claim to prove
definitions: []
permitted_background: []    # named theorems and citations usable without proof
proof_obligations: []
```
