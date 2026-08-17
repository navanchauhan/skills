# Semantic Port Lens

**Best solution:** the source's observable behavior preserved at a declared boundary in target-native code, structure beneath it free per subsystem; not transliteration, not an FFI binding, not an unconstrained rewrite.

Scale gate: before reading code, run a mechanical census (cloc or a tokenizer, no code reading) over the reachable source. Over ~20k lines, or census tokens over half a task context window, read `references/semport-protocol.md` and run its phases; the sections below are its single-context degenerate case, and its artifacts satisfy the obligations here.

## Characterize, then port
- Pin one upstream ref; tracking later starts from it.
- Before any unit ports (spikes exempt), persist skeletons as a contract amendment: boundary declaration, oracle inventory, port map, divergence policy, port units. Small or immediate-execution ports surface them as explicit assumptions; large ones may make this set the first approved milestone, agreed at the clarification gate.
- Characterization alone is a valid delivery floor when the user wants the spec, not the port; publish it as a standalone artifact. A supplied spec package is audited for clause, journey, and error-semantics coverage instead of re-derived, its provenance and audit recorded; gaps become characterization work scoped to the gaps.
- Each unit's contracts exist before it ports; characterize→port interleaves once inventory and dependency graph exist.
- The boundary declaration states whether performance and log/text output are contractual; performance defaults out (baseline it and compose the performance lens when speed motivates the port).
- Derive contracts from source behavior — tests, examples, docs, recorded execution, strongest first. A unit is characterized when its input/output, error, ordering, default, and side-effect behaviors cite evidence (plus concurrency and cancellation where declared contractual), ordering citations tagged contractual or incidental.
- Before the spec counts as done, one spec-blind agent probes the running source and diffs its observations against the draft; every finding becomes a clause or a documented intentional departure — silence is not a disposition.
- Record user journeys as root-spec artifacts: pre-conditions, then numbered steps separating user action from enumerated observable system responses. Each journey drives an end-to-end differential scenario beyond per-unit oracles; a port is not parity-complete until every journey passes on the integrated target.
- Enumerate must-diverge behaviors: source behaviors correct in the original but wrong to preserve — update feeds, phone-home and telemetry endpoints, product identity, signing keys, trademarks. For every external contract marked non-redesignable, test: imposed by a third party (keep), or the identity of the specific product instance (quarantine)? Each entry gets a divergence test proving the port does not reproduce it; reproducing one is a defect, not parity.
- Mocks outside the unit's boundary (services, clocks, networks) keep source tests strong; mocking the unit's own behavior makes them weak — characterize such units against the source under the same deterministic stubs, never the raw service, before porting.
- Parity oracles, strongest first: side-by-side execution, ported source tests, source-recorded goldens, conformance suite, property tests; spec-true subsystems rank conformance highest. Characterization evidence never counts as parity evidence.
- Declare one equivalence relation per unit — trace-modulo-normalizations | final-state | distributional | invariant-metamorphic — derived from the language-disagreement axes. "Behaves the same" without a named relation is unfalsifiable.
- Fidelity is per subsystem: idiomatic reshape by default; facade where a delegated library owns the behavior; mirror only with recorded evidence (upstream churn there, or a named dense algorithm). Mirror beyond a minority of subsystems returns the map to approval with that fact surfaced.
- Mirror never licenses statement-level translation: source-language plumbing dissolves into target idioms (decorator→option function, inheritance→composition, coroutines→goroutines/channels).
- Declare per-subsystem divergence policy: spec-true only if the port map names the intent source (docs, conformance suite, changelog), else bug-for-bug. Ledger every omission, alteration, and addition, including from delegated libraries.
- A source bug is observed behavior contradicting that intent: apply the declared policy and ledger it; escalate only where the policy is silent or a preservation obligation is crossed.
- Security defects (attacker-controlled input at the boundary): fix immediately, ledger, surface as a contract amendment in the same delivery.
- A unit is well-cut only if its scope is statable in contract terms (inputs, outputs, errors, ordering, defaults) without naming a source file; a one-file unit records why it is also a behavioral boundary. Every unit's consumer-visible boundary contract must fit one context (practically 10–20% of the smallest task window); interiors are unbounded and read hierarchically. An over-cap boundary is re-cut, never lossily summarized. First milestone: the smallest dependency-closed adoptable slice.
- Track each unit in a monotone ledger: unspecced → specced → spec-verified → ported → validated → reconciled, with terminals not-ported(evidence) and superseded-by(unit|delegation), an orthogonal blocked(on, resume-command) flag, and back-transitions only as append-only reopened(reason) events.
- Per unit: port its source tests, implement, run side by side where runnable, recording runnability and, for nondeterministic output, a normalization rule — deterministic stubs, never raw output; spec or map corrections are contract amendments.
- Commit each unit the moment it validates — contract artifacts in the first commit, implementation per unit after; never ask whether to commit. Uncommitted work does not exist: a crashed session erases it, and the unit ledger has no resume index without it.
- Scale: a CLI or single-API-surface port whose observable contract (flags, exit codes, output formats) fits one golden-file document may collapse the entire artifact set into it, contract fields inline; approval still sees it, oracle-before-implementation holds.

## Track upstream
- Process upstream changes in first-parent topological order by functional effect, not text alone; batch acknowledge-class commits (docs-only, CI-only, source-language-only).
- Route each change through the port map — port the translation, not the diff; unmapped concepts characterize-then-map first.
- The ledger lives in the product tree so it survives without Hammer: shortsha|date|disposition(new→implemented|acknowledged|out-of-scope)|reason|port-commit, committed atomically with its change (acknowledge-class batches: ledger-only commits).
- Implemented dispositions name parity evidence; on resume, reconcile dispositions newer than the newest port commit with executed, not merely recorded, parity evidence.

## Avoid
- Statement-level mirroring anywhere; idiomatic reshaping of a subsystem the map marked mirrored.
- Porting private internals invisible at the boundary; re-implementing behavior a delegated library owns.
- Silent divergence: omission, alteration, addition (same-named feature, different semantics). Ledgered must-diverge entries are not silent.
- Oracle-induced overengineering: widening internal or public API surfaces to satisfy testability pressure. Test seams stay internal; the oracle adapts to the architecture, never the reverse.
- Claiming parity from compilation, the pre-existing target suite, or matched file counts (record under insufficient_outcomes).

## Completion checks
- Every ported unit traces to characterization artifacts; the symbol-level API map marks each public symbol ported, renamed, omitted, or added.
- Each unit's parity evidence meets its declared parity rank; side-by-side skips and disagreements carry reasons.
- The ported unit exposes no interface the port map does not warrant; complexity added only to feed the oracle has been removed.
- License obligations met: attribution and notices for ported code and tests, or behavior-only reimplementation where licenses conflict.
- Attack parity where languages disagree: boundary inputs, error paths, contractual ordering, defaults, side effects (filesystem, network, environment, events), nil/empty, unicode, integer width, cancellation, concurrency model. Each axis resolves in the unit's declared equivalence relation as a witnessed normalization or an order-is-contract assertion.

## Contract fields
```yaml
upstream: ""                # repo, pinned ref, read-only checkout, licenses
mode: ""                    # characterize-port | track
semantic_spec: ""           # boundary declaration + behavioral contracts + journeys
port_map: ""                # concept/symbol map, fidelity, mirror evidence
divergence_ledger: ""       # policy; omissions, alterations, additions
must_diverge: []            # source behaviors that must NOT survive the port, each with a divergence test
port_units: []              # dependency order, first adoptable
equivalence_relations: {}   # unit -> trace-modulo-normalizations | final-state | distributional | invariant-metamorphic
unit_ledger: ""             # per-unit lifecycle (unspecced -> ... -> reconciled); same product-tree-or-state-branch choice as ledger_path
ledger_path: ""             # commit dispositions; tracking starts at the pinned ref
delegation_boundaries: []   # library-owned behavior vs replaced spec
parity_evidence: []         # per-unit rank, runnability, normalization
```
