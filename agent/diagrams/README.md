# Diagram Policy

Diagram images are views, not source of truth.

Each maintained diagram has:

1. Mermaid source in `src/`.
2. A semantic Markdown companion describing nodes, edges, invariants, and provenance.
3. An optional generated SVG or PNG for human viewing.

## Change protocol

- Preserve stable node IDs.
- Update source and semantic companion together.
- Record ambiguous source material as an open question.
- Validate that every edge has a documented payload or control meaning.
- Do not add the removed evaluation layer.

An agent should read the semantic companion first and Mermaid only when it needs topology. Personal milestone diagrams and implementation experiments belong under `.agent-local/`.
