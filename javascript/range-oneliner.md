You can use a utility library from lodash or write it yourself. The traditional way uses a for-loop:

```js
function range (start, end) {
  if (!end) {
    end = start
    start = 0
  }
  let range = []
  for (let i = start; i < end; i++) {
    range.push(i)
  }
  return range;
}
```

Of course, this is a very simple function that doesn't take into account invalid inputs, `Infinity`, etc., so it's only useful when you have control over its parameters. Nevertheless, it's a function I find myself copying and pasting into various projects.

There's a shorter way to achieve the same thing though, using the

```js
let range1 = (total) => Array.from({ length: end }, (v, i) => i + 1)
let range2 = (start, end) => Array.from({ length: end - start }, (v, i) => i + start)
let range3 = (start, stop, step) => Array.from({ length: (stop - start) / step + 1 }, (_, i) => start + i * step);

// Generate a sequence of numbers
// Since the array is initialized with `undefined` on each position,
// the value of `v` below will be `undefined`
range(5);
// [0, 1, 2, 3, 4]

range2(10, 10);
// [10, 11, 12, 13, 14, 15, 16, 17, 18, 19]

// Generate numbers range 0..4
range3(0, 4, 1);
// [0, 1, 2, 3, 4]

// Generate numbers range 1..10 with step of 2
range3(1, 10, 2);
// [1, 3, 5, 7, 9]

// Generate the alphabet using Array.from making use of it being ordered as a sequence
range3("A".charCodeAt(0), "Z".charCodeAt(0), 1).map((x) =>
  String.fromCharCode(x),
);
// ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z"]
```

For more information, see the MDN article on [`Array.from`](hhttps://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from#using_arrow_functions_and_array.from)