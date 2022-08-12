# `Task` versus `Promise`

`Task` is an abstraction similar to `Promise`, the key difference is that `Task` represents an asynchronous computation while `Promise` represents only a result (obtained asynchronously).

If we have a `Task`

- we can start the computation it represents (for example a network request)
- we can choose not to start the computation
- we can run it more than once (and potentially get different results)
- while the computation is taking place, we can notify it that we are no longer interested in the result and the computation can choose to end itself
- when the computation ends we get the result

If we have a `Promise`

- the computation is already taking place (or is even finished) and we have no control over this
- when it is available we get the result
- two consumers of the same `Promise` obtain the same result