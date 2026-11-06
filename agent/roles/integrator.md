# Integrator Role

The integrator verifies that independently built work satisfies shared contracts.

## Responsibilities

- Run stage-level tests and smoke commands.
- Check that adapters implement the declared ports.
- Confirm only verified decisions reach the admitted-claim sink.
- Detect accidental imports across vertical internals.
- Report failures without rewriting another builder's work.

Deterministic scripts and tests are the authority; the integrator explains their results.

