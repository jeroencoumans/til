We want to sort a list of strings that have numbers in them, and we want to sort them by natural order.

```js
let floors = ['Floor 31', 'Floor 32', 'Floor 1', 'Floor 2', 'Floor 3', 'Floor 0', 'Floor 10', 'Floor 11', 'Floor 30']
```

Unfortunately, the default Javascript sorting doesn't work here, since it relies on lexicographic (alphabetical) ordering. 


```js
floors.sort()
// ['Floor 0', 'Floor 1', 'Floor 10', 'Floor 11', 'Floor 2', 'Floor 3', 'Floor 30', 'Floor 31', 'Floor 32']
```

Of course, we'd like it if Floor 2 and 3 follow Floor 1 instead of 11. This is also true for an array of pure numbers:

```js
let numbers = [0, -1, 21, 10, 1, 6, 7, 8, 20]
numbers.sort()
// [-1, 0, 1, 10, 20, 21, 6, 7, 8]
```

It's why we need to always use a comparator function:

```js
let ascending = (a, b) => a -b
numbers.sort(ascending)
//  [-1, 0, 1, 6, 7, 8, 10, 20, 21]
```

By default, values are [converted into strings](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort), so it's basically the same as specifying `floors.sort((a, b)=> a.localeCompare(b))`. 
It turns out that the natural sorting we're after is also built into this `localeCompare` method:

```js
let ascendingNumeric = (a, b)=> a.localeCompare(b, undefined, {numeric: true })
floors.sort(ascendingNumeric)
// ['Floor 0', 'Floor 1', 'Floor 2', 'Floor 3', 'Floor 10', 'Floor 11', 'Floor 30', 'Floor 31', 'Floor 32']
```

You can define additional options, for example to treat letters with different accents or cases the same, use `{ numeric: true, sensitivity: 'base' }`. For large arrays, use the following:

```js
let collator = new Intl.Collator(undefined, { numeric: true, sensitivity: 'base' })
let naturalSort = collator.compare
```

This is so simple and elegant, it should be in your default Javascript toolbelt! 
