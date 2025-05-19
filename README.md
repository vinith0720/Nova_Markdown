## 🔹 JavaScript Array Methods 

#

| Method           | Parameters                                    | Callback Parameters                        | Returns                  | Mutates? | Notes / Use Case                      |
|------------------|-----------------------------------------------|--------------------------------------------|---------------------------|----------|----------------------------------------|
| `map()`          | `(callback, thisArg?)`                        | `(value, index, array)`                    | New array                | ❌       | Transform each item                    |
| `filter()`       | `(callback, thisArg?)`                        | `(value, index, array)`                    | Filtered array           | ❌       | Keep matching elements                 |
| `reduce()`       | `(callback, initialValue)`                    | `(acc, curr, index, array)`                | Final single value       | ❌       | Summing, flattening, grouping          |
| `reduceRight()`  | `(callback, initialValue)`                    | Same as `reduce()`                         | Final value              | ❌       | Right-to-left reduction                |
| `forEach()`      | `(callback, thisArg?)`                        | `(value, index, array)`                    | `undefined`              | ❌       | Iterate with side effects              |
| `some()`         | `(callback, thisArg?)`                        | `(value, index, array)`                    | `true/false`             | ❌       | At least one match                     |
| `every()`        | `(callback, thisArg?)`                        | `(value, index, array)`                    | `true/false`             | ❌       | All must match                         |
| `find()`         | `(callback, thisArg?)`                        | `(value, index, array)`                    | First matching value     | ❌       | Find object or match                   |
| `findIndex()`    | `(callback, thisArg?)`                        | `(value, index, array)`                    | Index or -1              | ❌       | Index of first match                   |
| `flatMap()`      | `(callback, thisArg?)`                        | `(value, index, array)`                    | Flattened array          | ❌       | Map + flat (depth = 1)                 |
| `sort()`         | `(compareFn?)`                                | `(a, b)`                                   | Sorted array             | ✅       | In-place sort                          |
| `fill()`         | `(value, start?, end?)`                       | –                                          | Filled array             | ✅       | Replace range                          |
| `copyWithin()`   | `(target, start?, end?)`                      | –                                          | Modified array           | ✅       | Copy slice internally                  |
| `Array.from()`   | `(arrayLike, mapFn?, thisArg?)`               | `(value, index)`                           | New array                | ❌       | Convert iterable                       |
| `Array.of()`     | `(item1, item2, ...)`                         | –                                          | New array                | ❌       | Safe array creation                    |
| `at()`           | `(index)`                                     | –                                          | Single value             | ❌       | Supports negative indexing             |
| `includes()`     | `(valueToFind, fromIndex?)`                   | –                                          | `true/false`             | ❌       | Check for existence                    |
| `join()`         | `(separator?)`                                | –                                          | String                   | ❌       | Join array into string                 |
| `toSorted()`     | `(compareFn?)`                                | `(a, b)`                                   | Sorted copy              | ❌       | Immutable sort                         |
| `toSpliced()`    | `(start, deleteCount, ...items)`              | –                                          | New array                | ❌       | Immutable version of splice            |


## 📥 Callback Parameters Breakdown


### Most array methods that accept a callback use this format:

`callback(currentValue, index, array)`

- `currentValue:` The element being processed
- `index:` The index of the element
- `array:` The original array


### ✅  Mutation Behaviour



| Mutates Original Array | Methods                                                                 |
|------------------------|-------------------------------------------------------------------------|
| ❌ No (Immutable)       | `map`, `filter`, `reduce`, `reduceRight`, `flatMap`, `find`, `findIndex`, `some`,  `every`, `includes`, `join`, `at`, `Array.from`, `Array.of`, `toSorted`, `toSpliced` |
| ✅ Yes (Mutable)        | `sort`, `fill`, `copyWithin`, `splice`, `reverse`, `push`, `pop`, `shift`, `unshift` |



#

# 🔹 JavaScript Object Methods

| Method                 | Parameters                     | Returns                   | Mutates? | Notes / Use Case                            |
|------------------------|-------------------------------|----------------------------|----------|---------------------------------------------|
| `Object.keys()`        | `(obj)`                        | Array of keys             | ❌       | Iterate keys or get length                  |
| `Object.values()`      | `(obj)`                        | Array of values           | ❌       | Iterate values                              |
| `Object.entries()`     | `(obj)`                        | Array of `[key, value]`   | ❌       | For `for...of`, maps, CSV, etc.             |
| `Object.fromEntries()` | `([[key, value], ...])`        | Object                    | ❌       | Reverse of `entries()`                      |
| `Object.assign()`      | `(target, ...sources)`         | Modified target object    | ✅       | Merge or clone (shallow)                    |
| `Object.hasOwn()`      | `(obj, key)`                   | Boolean                   | ❌       | More reliable than `hasOwnProperty()`       |
| `Object.freeze()`      | `(obj)`                        | Frozen object             | ✅       | Prevent any mutation (shallow)              |
| `Object.seal()`        | `(obj)`                        | Sealed object             | ✅       | Prevent adding/removing props               |
| `Object.create()`      | `(proto, propertiesObject?)`   | New object                | ❌       | Create object with custom prototype         |
| `Object.defineProperty()` | `(obj, prop, descriptor)`  | Modified object           | ✅       | Fine control over properties                |
| `Object.defineProperties()`| `(obj, descriptors)`      | Modified object           | ✅       | Multiple properties with descriptors        |
| `Object.getOwnPropertyDescriptor()` | `(obj, prop)`   | Descriptor object         | ❌       | See config/writable/enumerable flags        |
| `Object.getPrototypeOf()`| `(obj)`                     | Prototype object          | ❌       | Introspection / inheritance                 |
| `Object.setPrototypeOf()`| `(obj, proto)`              | Modified object           | ✅       | Change prototype (not recommended)          |
| `Object.is()`          | `(value1, value2)`             | Boolean                   | ❌       | Strict equality + handles `NaN`, `-0`       |
| `Object.isFrozen()`    | `(obj)`                        | Boolean                   | ❌       | Check if frozen                             |
| `Object.isSealed()`    | `(obj)`                        | Boolean                   | ❌       | Check if sealed                             |
| `Object.preventExtensions()` | `(obj)`                 | Modified object           | ✅       | Prevent new props (existing still editable) |
| `Object.isExtensible()`| `(obj)`                        | Boolean                   | ❌       | Check if extensible                         |


## ✅ Mutation Behavior

| Mutates Object? | Methods                                                                 |
|------------------|-------------------------------------------------------------------------|
| ❌ No            | `Object.keys`, `Object.values`, `Object.entries`, `Object.fromEntries`, `Object.is`, `Object.getPrototypeOf`, `Object.getOwnPropertyDescriptor`, `Object.isFrozen`, `Object.isSealed`, `Object.isExtensible` |
| ✅ Yes           | `Object.assign`, `Object.freeze`, `Object.seal`, `Object.defineProperty`, `Object.defineProperties`, `Object.setPrototypeOf`, `Object.preventExtensions` |
