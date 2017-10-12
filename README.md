# The arr.reduce() function

The official syntax description for this function is as follows:

```
arr.reduce(callback, [initialValue])
```

I prefer to think of `reduce` as having the following syntax:

```
arr.reduce(reducer, initialValue)
```

where:

| param | description |
| ----- | ----------- |
| `reducer` | is a function taking up to four arguments, of which I tend to use the first two only, viz. `accumulator` and `elem`,  and which should return (either the same or a new) `accumulator`. (Elsewhere you will often see the word `accumulator` being used instead of `accumulator`.) |
| `initialValue` | is an initial value for the `accumulator` argument of `reducer`, i.e. for the first iteration of `arr.reducer()` |

The `reducer` callback function looks like this:

```
(accumulator, elem) => {
    // do something with accumulator and elem
    return accumulator
}
```

The `arr.reduce()` function iterates over the array `arr` from start to finish and for each iteration calls `reducer`, passing the current iteration element from the array and the `accumulator` value of the previous iteration _or_ the initial value of `accumulator` passed as the second argument to `arr.reduce()` in case of the first iteration.

The value eventually returned by `arr.reduce` is the `accumulator` returned from the last iteration. (_Do not forget to ultimately return the accumulator from the reducer function!_)

The whole process is visualised in Figure 1 below (the term `bucket` was used to represent the `accumulator`).

![buckets](images/reduce.png)
<br>Figure 1. Passing the accumulator as in a conveyor belt

## Example 1: using reduce to filter

Although there is a separate `array.filter()` function, let's try and use `arr.reduce` for this:


```
const arr = [6, 3 , 10, 1];
const evenNumbers = arr.reduce((acc, elem) => {
    if (elem % 2 === 0) {
        acc.push(elem);
    }
    return acc;
}, []);
console.log(evenNumbers);
```

In this example our accumulator is an (initially empty) array. We put elements (in this case integer numbers) in the accumulator only when they are divisible by 2.

## Example 2: use reduce to transform elements (i.e. map)

Again, there is already a separate `array.map()` function that accomplishes this, but for our purposes it is illustrative to implement it using `array.reduce()`. In this example an array of integer numbers is mapped to an array of their squares.

```
const arr = [6, 3 , 10, 1];
const squares = arr.reduce((acc, elem) => {
    acc.push(elem * elem);
    return acc;
}, []);
console.log(squares);
```

## Example 3: use reduce to group an array by a common property

In this example our accumulator is not an array, but an (initially empty) object. It groups the array elements by gender.

```
const arr = [
  { gender: 'F', name: 'Joyce'},
  { gender: 'M', name: 'Jim' },
  { gender: 'F', name: 'Lucy' },
  { gender: 'M', name: 'Ferdinand' }
];
const groupedNames = arr.reduce((acc, elem) => {
  if (acc[elem.gender]) {
    acc[elem.gender].push(elem);
    } else {
      acc[elem.gender] = [elem];
  }
  return acc;
}, {});
console.log(groupedNames);
```

Result:

```
{
  F: [
    { gender: 'F', name: 'Joyce' },
    { gender: 'F', name: 'Lucy' }
  ],
  M: [
    { gender: 'M', name: 'Jim' },
    { gender: 'M', name: 'Ferdinand' }
  ]
}
```

The `arr.reduce()` function might look complex at first but once you get the hang of it can be quite useful and **reduce** :smile: the need for `for` loops in your code.