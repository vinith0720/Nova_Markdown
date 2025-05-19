## 🔹 Advanced JavaScript Array Methods Cheat Sheet

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


---

### 🔁 Mutation Summary Table (Markdown)

```markdown
## 🔁 Does This Method Mutate the Original Array?

| Mutates Original Array | Methods                                                                 |
|------------------------|-------------------------------------------------------------------------|
| ❌ No (Immutable)       | `map`, `filter`, `reduce`, `reduceRight`, `flatMap`, `find`, `findIndex`, `some`, `every`, `includes`, `join`, `at`, `Array.from`, `Array.of`, `toSorted`, `toSpliced` |
| ✅ Yes (Mutable)        | `sort`, `fill`, `copyWithin`, `splice`, `reverse`, `push`, `pop`, `shift`, `unshift` |
