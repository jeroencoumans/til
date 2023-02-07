I was working on puzzle 14 of the Advent Javascript challenge 2022 and I couldn't figure out a solution. Luckily, plenty of other people have solved it and I found a really neat way.

The puzzle is to find the optimal (smallest) path in an array of numbers, where you can only move diagonally:

    0
   / \
  7   4
 / \ / \
2   4   6

Which, in code, is represented as an array:

```js
[
  [0],
  [7, 4],
  [2, 4, 6]
]```

Here, the optimal path is 8 (0, 4, 4). 

A correct implementation only takes three magnificent lines:

```js
function getOptimalPath(path) {
  return path.reduceRight((previous, current) => {
    return current.map((nr, index) => {
      return nr + Math.min(previous[index], previous[index + 1]);
    });
  })[0];
}```

I learned two things from this solution:

1. I forgot about `reduceRight`. It's the same as `reduce` (which I use all the time), but I never had a good use case for `reduceRight`. It's perfect here since the solution requires a single number, so the program will figure out the smallest number from each row by comparing it with the previous row. My failed attempts were looping over the array from top to bottom, which is difficult because you need to compute each possible combination of paths that way before you can determine the optimal one. 
2. When I tried to dissect the solution, I was surprised that it had no initial value provided. Huh!? Turns out, the inital value is *optional*. If no value is provided, it will use the last element of the array as the initial value and start with the one before that. So, the first time this runs, `previous` is set to `[2, 4, 6]` and `current` is `[7, 4]`. Aha!

I asked ChatGPT to rewrite this solution with regular for...of loops and it answered with a fully working version:

"Sure! Here is how you can rewrite the getOptimalPath() function using a for...of loop:"

```js
function getOptimalPath(path) {
  let previous = path[path.length - 1];
  for (let current of path.slice(0, -1).reverse()) {
    let optimalPath = [];
    for (let [index, nr] of current.entries()) {
      optimalPath[index] = nr + Math.min(previous[index], previous[index + 1]);
    }
    previous = optimalPath;
  }
  return previous[0];
}
```

"This version of the function works in a similar way as the original one, but instead of using the reduceRight() method, it uses a for...of loop to iterate over the path array in reverse order. It also uses another for...of loop to iterate over the elements of the current array and compute the optimal path.

I hope this helps! Let me know if you have any questions."

Yes, that definitely helps! This solution works by storing the last row as `previous` and then looping over the array in reverse order. I didn't know you could do `.entries()` on an array so you can use a `for...loop` and still have an index. 

This version of the function works by iterating over the path array in reverse order using a for...of loop. For each element in the array, it computes the optimal path using another for...of loop and stores it in the optimalPath array. The final result is the first element of the optimalPath array.

