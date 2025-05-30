<p align="center">
<b>iterable-run</b> is a tiny utility that collects all the <b>yielded</b> values as well as the <b>return</b> result<br>from an iterable or an async iterable.
</p>

# iterable-run

## Features

- Supports both **iterables** and **async iterables**
- Preserves the final return value (`return`)
- Fully typed
- Well tested

## Usage

### reading sync iterable

```ts
import { iterableRun } from "iterable-run"

function* deepThought() {
  yield "ultimate"
  yield "answer"
  return 42
}

const { items, result } = iterableRun(deepThought())

console.log(items) // ["ultimate",  "answer"]
console.log(result) // 42
```

### reading async iterable

```ts
import { asyncIterableRun } from "iterable-run"

async function* range(n: number) {
  for (let i = 0; i < n; i++) {
    yield await Promise.resolve(i)
  }
  return await Promise.resolve("finished")
}

const { items, result } = await asyncIterableRun(range(5))

console.log(items) // [0, 1, 2, 3, 4]
console.log(result) // "finished"
```

## Related

If you **don't need the return value** from the iterable, consider using the standard:

- [`Array.from(iterable)`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/from) for synchronous iterables
- [`Array.fromAsync(asyncIterable)`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/fromAsync) for asynchronous iterables

These native methods collect yielded values into arrays but **discard the final `return` result**. That makes them simpler and potentially more appropriate if you only care about the items.
