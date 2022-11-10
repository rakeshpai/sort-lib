# sort-lib

A small set of utilities for writing more readable sort functions.

* Tiny - almost fits in a tweet, tree-shakable.
* Typescript
* ES modules

If you're like me, you probably always need to look up how to sort an array in JS. ("How do you sort strings again?", "Is it first argument minus second argument for descending, or the other way around?") This module is meant to solve that problem, while making the code more readable in the process.

## Installation

```
npm install sort-lib
```

## Usage

```js
import { desc, byNum } from 'sort-lib';

[
  { age: 25 },
  { age: 10 },
  { age: 42 }
]
  // Sort descending by number `.age`
  .sort(desc(byNum(value => value.age)))

/*
[
  { age: 42 },
  { age: 25 },
  { age: 10 }
]
*/
```

## Functions

### `asc` / `desc`

Wrap the sort function with `asc` or `desc` to set the sort direction. If you do not use this function, sorting will always happen in the ascending direction.

### `byNum(numberGetter)`

Creates a sort function that can be used to sort a number. You have to pass in a function that returns a number given a value in the array - this number is what the sorting is performed on. `byNum` returns a sort function.

### `byDate(dateGetter)`

Like `byNum`, but for date objects. Pass in a function that returns a date given a value in the array. Returns a sort function.

Internally, compares dates by calling `.getTime()` on the date objects.

```js
[
  { dateOfBirth: new Date('2000-01-01') },
  { dateOfBirth: new Date('2020-01-01') },
  { dateOfBirth: new Date('1970-01-01') }
]
  // Sort ascending by date `.dateOfBirth`
  .sort(asc(byDate(x => x.dateOfBirth)))

/*
[
  { dateOfBirth: new Date('1970-01-01') },
  { dateOfBirth: new Date('2000-01-01') },
  { dateOfBirth: new Date('2020-01-01') }
]
*/
```

### `byString(stringGetter)`

Like `byNum` and `byDate` above, but for strings. Pass in a function that returns a string given a value in the array. Returns a sort function.

Internally, compares strings using the `.localeCompare` method.

```js
[
  { name: 'Bob' },
  { name: 'Alice' },
  { name: 'Charlie' }
]
  // Sort descending by string `.name`
  .sort(desc(byString(x => x.name)))

/*
[
  { name: 'Charlie' },
  { name: 'Bob' },
  { name: 'Alice' }
]
*/
```

## Tips

* Define a function called `prop`, or use one from a library like [ramda](https://ramdajs.com/docs/#prop) for even more point-free code:

  ```ts
  const prop = <T>(key: keyof T) => (obj: T) => obj[key];

  [{ age: 42 }, { age: 10 }]
    .sort(desc(byNum(prop('age'))))
  ```
* If your array only contains primitives, you might want to define a function called `identity` (or use the one from [ramda](https://ramdajs.com/docs/#identity))
  ```ts
  const identity = <T>(x: T) => x;

  ['Alice', 'Bob'].sort(desc(byString(identity)))
  ```