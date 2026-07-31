# Capability Progression System

Use this reference for learning routes, multi-session coaching, mastery decisions, and exercise selection.

## Define capability as observable behavior

Replace topic-only goals such as “learn React hooks” with behavior:

- explain why state belongs in one component rather than another
- predict when an effect runs and what cleanup does
- implement a controlled form from a short requirement
- debug stale state or dependency problems
- choose between local state, context, and server state and defend the tradeoff

Knowledge supports capability, but consuming explanations is not evidence that the learner can perform.

## Capability ladder

Use the lowest level that lacks evidence:

| Level | Observable ability | Typical rep |
| --- | --- | --- |
| `L0 Recognize` | Identify syntax, concepts, and execution flow in existing code | annotate or trace a small example |
| `L1 Explain/Predict` | Explain why code is shaped this way and predict behavior before running it | output prediction, data-flow trace |
| `L2 Modify` | Make a constrained change with documentation or hints | add validation, change a data shape |
| `L3 Implement` | Build the target behavior from a short spec without a copyable solution | blank-file or TODO-boundary task |
| `L4 Debug/Test` | Form a hypothesis, isolate a fault, fix it, and prevent regression | broken-code diagnosis plus test |
| `L5 Design/Transfer` | Choose an approach in an unfamiliar variation and explain tradeoffs | design a nearby feature or abstraction |

Do not force every topic to L5. Set the target level from the user's real goal.

## Evidence rules

Prefer evidence in this order:

1. The learner implements or debugs the behavior.
2. Automated tests or observable runtime behavior verify it.
3. The learner explains the data flow and one tradeoff.
4. A transfer rep succeeds with less help.

Record the amount of help:

- `independent`
- `docs-only`
- `hinted`
- `guided`
- `demonstrated`

Only `independent` or `docs-only` transfer evidence normally supports promotion to the next level. A demonstrated solution creates a future practice item, not mastery.

## Select the next rep

Choose a rep at the edge of current ability:

- If the learner cannot trace the code, use recognition and prediction.
- If they can explain but cannot change it, use a constrained modification.
- If they can modify examples, remove the example and give a short specification.
- If they can implement happy paths, introduce a bug, boundary case, or test requirement.
- If they can debug familiar code, change the context and ask for a design choice.

Change one main difficulty dimension at a time:

- less scaffolding
- less familiar API
- more ambiguous requirement
- more interacting state
- stricter correctness or testing
- unfamiliar project location

## Space and interleave

Use three kinds of repetition:

- `Immediate`: a nearby transfer rep in the same session.
- `Delayed`: retrieve and implement again after at least one other topic or session.
- `Interleaved`: mix the skill with an older one, such as React state plus TypeScript unions.

Do not repeat identical prompts. Preserve the capability while changing surface details.

## Route construction

Build routes as thin vertical slices that produce working behavior:

1. language/runtime mental model
2. read and trace existing project code
3. make a safe local change
4. implement a small feature boundary
5. add tests and debug a seeded fault
6. refactor with an explicit reason
7. design and deliver a small independent slice

For work inside an existing repository:

1. inspect the feature's current component/module boundary and data ownership
2. run the available baseline test, type-check, or smallest runtime check
3. identify agent-owned setup and learner-owned target behavior
4. keep the learner's first diff small enough to review
5. verify the diff, then give a no-copy transfer rep

For React/TypeScript, prefer project slices such as:

- render typed server data
- add controlled filtering
- model loading/error/success states with a discriminated union
- extract a reusable hook only after duplication appears
- test interaction and one failure case
- diagnose an effect or stale-state bug
- choose state ownership for a new feature

Treat filtered, sorted, or otherwise computable values as derived data by default; do not mirror them into React state with an effect merely to practice effects. Introduce effects for synchronization with systems outside React, such as network requests, subscriptions, timers, browser APIs, or imperative widgets. Require dependency and cleanup reasoning when relevant.

Do not teach APIs detached from the data flow that makes them necessary.

## Session close

End substantial coaching with:

- `Evidence`: what the learner actually demonstrated and with how much help
- `Weak point`: the narrow unresolved gap
- `Next rep`: one executable task that reduces scaffolding or adds transfer

Use “What you learned” only for a concise mental-model summary; do not substitute it for evidence.
