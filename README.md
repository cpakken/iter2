
## Install
```bash
pnpm add from-iter
yarn add from-iter
npm install from-iter
```
### Size
| Package   | Size      | GZipped |
| --------- | --------- | ------- |
| from-iter | 1.52 KB 😎 | 673 B 😲 |

A Tiny Fully-Typed tree shakable lazy iterable library for JavaScript / TypeScript.
Combine and iterate over any iterable object or array with a simple chainable API.

Internally uses `for...of` for iterables and `for...in` for iterable objects to pipe values through a chain of functions.

Unlike chaining arrays, this library does not create intermediate arrays for each operation, instead, it iterates over the values and passes them through the chain of functions.

## Quick Start

```ts
import { fromIter } from 'from-iter'

// Create an IterList from anything that implements the Iterable protocol
// AsyncIterableIterator<any>  WIP 

//Iterable List (IterObject) uses for...of loop
//Arrays
const iterList = fromIter([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

//Map
const map = new Map([['foo', 'bar'], ['baz', 'qux'], ['hello', 'world']])
const iterList = fromIter(map) // same as fromIter(map.entries())
const iterList = fromIter(map.entries())
const iterList = fromIter(map.keys())
const iterList = fromIter(map.values())

//Set
const iterList = fromIter(new Set([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]))

//Generators
function* generator() {
  for (let i = 0; i < 10; i++) {
    yield i
  }
}
const iterList = fromIter(generator())


//Iterable Object (IterObject) uses for...in loop
const iterObj = fromIter({ foo: 'bar', baz: 'qux', hello: 'world' })
  .map((value, key, index) => key.concat(value))

//Combinations
const hybridIter = fromIter([1, 2, 3], [4, 5, 6], new Map(...), { foo: 'bar' })

//Chain fn
/**
 * @type Value - inferred from result of previous chain
 * @type Key - IterList -> <number>, IterObject -> <keyof<OBJECT_INPUT>>
 *           - index / key matching the original iterable
 * @type Index - <number> index of the output iterable
 */
const chainFn = (value: Value, key: Key, index: Index) => Result

//Chain operations (lazy evaluation - not executed until terminal operation is called)
const iterList = fromIter([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
  .map((value, key, index) => value * 2)
  .filter((value, key, index) => value % 2 === 0)
  .mapReduce((prev, value, key, index) => prev + value, initialValue)
  .until((value, key, index) => value > 10) 
//  .flatMap((value, key, index) => [value, value * 2]) TODO


//** Terminal operations **//

const reduced = iter.reduce((acc, value) => acc + value, 0)

//to Collection

const array = iter.toArray()
const object = iter.toObject()
const set = iter.toSet()
const map = iter.toMap()
const iterator = iter.values()

//Optional Mapper Functions
const array = iter.toArray((value, key, index) => value * 2)
const set = iter.toSet((value, key, index) => value * 2)
const iterator = iter.values((value, key, index) => value * 2)

const object = iter.toObject({
  key: (value, key, index) => key,
  value: (value, key, index) => value * 2
})

const map = iter.toMap({
  key: (value, key, index) => key,
  value: (value, key, index) => value * 2
})

// .to(<utilityReducer>())
const grouped = iter.to(groupBy((x) => (x % 2 === 0 ? 'even' : 'odd')))


```

## Roadmap
- [ ] Async Iterators `fromIterAsync()` 
- [ ] `createPipe` / `fromIter(iterator).pipe(...)`
- [ ] flatMap -> use `fromIter()` for child iterator items
- [ ] Benchmarks
- [ ] More tests
- [ ] Common chain functions
  - [ ] take, groupBy, takeWhile, drop, dropWhile, dropLast, dropLastWhile, skip, skipWhile, skipLast, skipLastWhile, skipRepeats, skipRepeatsWith, skipRepeatsBy, skipRepeatsWithKey, skipRepeatsBykey, skipWhileWith, skipWhileWithKey, 