+++
title = "Some Tiny Javascript Optimizations"
categories = ["misc"]
date = "2017-02-12T09:02:20-08:00"

+++

Preface: Optimizations in client-side Javascript is usually not worth sacrificing readability for. If your app is slow, consider whether you can either serve API requests better, do less with the DOM, minimize writing to local storage, or make any other reducuction of side effects. There is almost never enough data held by the client for there to be a need make working synchronous code run faster. I think the following make improvements in readability as well, so if you have both, why not... 

### Recursive tail calls

In the example below, a new stack has to be created each time the function is ran (to preserve the context for the addition). So, passing in `x:5000` creates 5000 stacks.

```js
function foo(x) {
  if (x < 0) {
    return 1;
  }
  return 1 + foo(x-1);
}
```

If we use an accumulator, there's no additional context to preserve so the returning stack can replace the function's current.

```js
function foo(x, accumulated) {
  if (x < 0) {
    return 1;
  }
  return foo(x-1, accumulated + 1);
}
```

### Using logical expressions

Unlike in C, Javascript logical expressions evaluate to an operand instead of `true`. That means you can simplify code like this:

```js
function foo(x) {
  if (!x) {
    return null;
  }
  return x.y;
}
```

A tiny-optimized version.

```js
function foo(x) {
  return x && x.y;
}
```

A similar pattern can be used for the `||` operator.

### Hashing > Iterating

The `switch` statement is making a comeback with Redux's documentation using it in constructing reducers. In general, though, a lookup by hashing and comparing is going to be faster than iterating to a matching element.

```js
function iterating(action) {
  switch (action.type) {
    case 'a':
      // do something with action.data
      ...
      return;
    case ...
    case 'z':
      ...
      return;
    default:
      return;
  }
}
iterating({type: 'z', data: 'asdf'});
```

Iterates 26 elements.

```js
const lookupTable = {
  'a': (data) => {...},
  ...
  'z': (data) => {...}
}

function hashing(action) {
  if (lookupTable[action.type]) {
    return lookupTable[action.type](action.data);
  }
}
```

One comparison and two lookups.

Of course this applies to iterating over any iterable (arrays, strings, etc), but `switch` is the less obvious case. 
