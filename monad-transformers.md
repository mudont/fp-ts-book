# Monad transformers

## Senario 1

Suppose we have defined the following APIs:

```ts
import { head } from 'fp-ts/lib/Array'
import { Option } from 'fp-ts/lib/Option'
import { pipe } from 'fp-ts/lib/pipeable'
import { map, Task, task } from 'fp-ts/lib/Task'

function fetchUserComments(
  id: string
): Task<Array<string>> {
  return task.of(['comment1', 'comment2'])
}

function fetchFirstComment(
  id: string
): Task<Option<string>> {
  return pipe(
    fetchUserComments(id),
    map(head)
  )
}
```

The type of the range of the `fetchFirstComment` function is `Task <Option <string>>` which is a nested data structure.

Is it possible to associate a monad instance?

## Scenario 2

To model an AJAX call, the type constructor `Task` is not sufficient since it represents a computation
asynchronous which can never fail, how can we also model the possible errors returned by the call (`403`,` 500`, etc ...)?

More generally, suppose we have a computation with the following properties:

- asynchronous
- can fail

How can we shape it?

We know that these two effects can be respectively encoded by the following types:

- `Task <A>` (asynchronicity)
- `Either <E, A>` (possible failure)

and that both have an instance of monad.

How can I combine these two effects?

In two ways:

- `Task <Either <L, A >>` represents an asynchronous computation that can fail
- `Either <L, Task <A>>` represents a computation that can fail or that returns an asynchronous computation

Let's say I'm interested in the first of two ways:

`` '' ts
interface TaskEither <L, A> extends Task <Either <L, A >> {}
``

Is it possible to define a monad instance for `TaskEither`?

## Monads do not compose

In general, monads do not compose, i.e. given two instances of monads, one for `M <A>` and one for `N <A>`,
then `M <N <A>>` may not still admit an instance of a monad.

**Quiz**. Because? Try defining the `flatten` function.

That they do not compose in general, however, does not mean that there are no particular cases where this happens.

Let's see some examples, if `M` admits a monad instance then the following types admit a monad instance:

- `OptionT <M, A> = M <Option <A>>`
- `EitherT <M, L, A> = M <Either <L, A >>`

More generally, ** monad transformers ** are a list of specific "recipes" that show how a monad instance can be associated with `M <N <A>>` when `M` and` N` admit an instance of monad.

To operate comfortably we need operations that allow to immerse the computations that run in the monads constituting the target monad: the _lifting_ functions.