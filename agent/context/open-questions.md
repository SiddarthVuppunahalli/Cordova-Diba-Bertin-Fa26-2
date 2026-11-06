# Shared Open Questions

These affect multiple verticals and must not be silently resolved by one developer's implementation.

1. How should a claim's lifecycle account for repository convergence after the claimed work is integrated?
   - Should an integrated claim remain active until every contributor is on a repository state that contains it, or should integration and workspace convergence be tracked separately?
   - Is convergence measured across repository members, all registered workspaces, only active workspaces, or only workspaces affected by the claim?
   - What deterministic evidence proves that a workspace has incorporated the relevant mainline change?
   - How do inactive or offline workspaces, newly registered workspaces, multiple workspaces owned by one member, partial integrations, squash or cherry-pick integration, and later reverts affect convergence?
   - How does convergence interact with supersession, withdrawal, expiration, shared-context retrieval, and notification eligibility?
2. What should the final form and responsibility boundary of Shared Context be?
   - Should it remain a deterministic in-process module, become a separately deployed service, or include any narrowly scoped agent capability?
   - Should it permanently own post-admission lifecycle, repository-scoped access, context retrieval, and provenance, or should some of those responsibilities belong to Orchestration or another vertical?
   - Which evidence-backed events may change lifecycle state, and which component verifies and publishes those events?
   - What public commands, queries, and durable handoffs must its port expose without coupling consumers to its deployment form?
3. Should the Orchestration vertical own intake and scheduling of `ChangeManifest` requests from Workspace agents before they reach Verification?
   - Would Orchestration provide durable queuing, concurrency limits, backpressure, fairness, prioritization, retry scheduling, and traffic shaping when many Workspace agents submit simultaneously?
   - Which responsibilities remain with the Workspace producer, transport, and Verification consumer so that Orchestration does not become an unnecessary bottleneck or a single component containing every vertical's policy?
   - Does Orchestration sit in the data path for every manifest, or act as a control plane that configures and observes independently queued Verification workers?
   - Where are deduplication and idempotency enforced when a manifest is retried or delivered more than once?
4. Should cross-vertical communication use a publish-subscribe model, point-to-point request delivery, or a deliberate combination of both?
   - Which interactions are events that may have multiple subscribers, and which require a direct request and correlated response?
   - What ordering, delivery, acknowledgement, retry, backpressure, dead-letter, and replay semantics are required for each contract?
   - Who owns topic or queue names, message-envelope schemas, contract versions, correlation identifiers, and subscriber authorization?
   - Can a transport be replaced without changing vertical domain logic or shared business contracts?
   - How does the transport choice interact with Orchestration ownership without requiring Orchestration to relay every message itself?
5. What is the first supported repository language and parser boundary?
6. Which notification adapter is the first team-wide integration target?
7. Will permissioned source excerpts be supported, and what audit record is required?
8. What limits apply to semantic-model checks across verticals?
9. What deployment configuration guarantees that local stubs cannot be selected in a production composition?

Questions specific to one developer's current vertical belong under `.agent-local/`.
