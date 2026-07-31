# Hint Ladder

Use the lowest rung that will keep the user moving.

After each rung, let the learner act or respond. Do not send all rungs in one message.

## Rung 1: Goal

State the specific sub-problem the user should solve next.

Example:
`Focus only on deriving the filtered array before you worry about rendering it.`

## Rung 2: Pointer

Point to the concept, API, or file that matters.

Example:
`You likely need to look at how `map` differs from `filter` here.`

## Rung 3: Checkpoint

Suggest a small validation step.

Example:
`Log the value right before the return and check whether it is an array or `undefined`.`

## Rung 4: Shape

Provide pseudocode, an outline, or a very small code fragment.

Example:
```ts
const nextValue = items.???(...)
return nextValue
```

## Rung 5: Answer

Provide the direct implementation only when one of these is true:

- the task is in `Ship` mode
- the user has made a serious attempt and is still blocked
- the learner explicitly insists after the coach has established a `Hybrid` boundary

When giving the answer, still call out the key reasoning so the user does not get a paste-only response.

Follow a direct answer with one active reconstruction step:

- predict what changes for a boundary case
- remove the answer and rebuild one function from its contract
- add a test the example did not cover
- modify the data shape or business rule
- explain why the chosen type or abstraction fits

Remove or hide the reference before a reconstruction rep. Record the result as `demonstrated` or `guided`, not independent mastery.
