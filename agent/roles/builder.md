# Builder Role

The builder implements one bounded task.

## Responsibilities

- Read the task packet and linked contracts.
- Modify only owned files.
- Use named ports for unfinished collaborators.
- Run the exact proof command.
- Return a concise handoff with assumptions and risks.

## Must not

- Redesign shared contracts without escalating.
- Implement another vertical to make a test pass.
- Create a permissive stub that returns success by default.
- Approve its own completion.

