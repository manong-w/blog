+++
title = "Mistakes in Naming"
categories = ["misc"]
date = "2017-05-19T09:02:20-08:00"

+++

Naming is an important part of programming. Good names help readers (including the author in the future) navigate the code and act as documentation. It shouldn't be an afterthought to go back and give informative names -- use them as placeholders for your thoughts as you code.

A good name for a function should tell you what to expect of the output and any side effects invoked during the execution of the function, given sufficient context. A good name for references should describe the value and/or indicate what it's used for.

----

### Always consider context

A good name allocates just enough characters to add information but doesn't repeat what should already have been derived from the context.

The context includes where the function is being defined, taking into consideration things like scope, outer functions, and the file/class name.

- `get` is mysterious as a global function but makes sense in the context of a `HashTable` class.

- `helper` is appropriate given that it's defined within an outer function like `sorter`.

- In a function called `findClosestNeighbors`, references like `closestNeighbor` and `secondClosestNeighbor` are repeating information. `closest` and `secondClosest` are unambiguous given the context of the outer function.

------

### Always begin function names with a verb

How do you convey output and side effect? A function always *does* soemthing. When it's called, the program is expected to take some action and that action should be captured by the choice of a verb.

A verb with the object is part of the name implies it'll be the ouput.

- `findMedian(listOfNumbers)`
- `calculateWinProbability(cards)`

Mutating side effects can also be conveyed through verbs that imply change to the object.

- `addTodo(list)`
- `withdraw(bankAccount, amount)`

It's sometimes tempting to name things the way you'd say it conversationally.

- `if (paymentDeclined())` - this reads like an English sentence but the clarity declines when used in other contexts. For example, assigning `status = paymentDeclined()`, it can't be assumed the value of `status` is a boolean (it could be the contingency value upon the payment being declined). Stick with starting with a verb: `isPaymentDeclined`.

----

### Don't bind to implementation

There's rarely a good reason for exposing a part of the implementation into the function signature. Functions act as an interface, they should be able to be quickly scanned and allow people to easily find one suited for their use case. Rarely does implementation come into consideration.

- `recursivelyMergeList(listA, listB)`
- `endSessionByDisconnectingSocket(session)` - If there's choices for how to end the session, you can specify the method through parameter `endSession(session, method)`
- `returnBrightest(colors)` - `getBrightest(colors)`
- `isThreadHoldingSemaphore` - `isActiveThread`

----

### Use succinct vocabulary

Words that encapsulate more meaning is usually a better choice than the expansion of it.

- `drawLineInMiddleOfAngles(angleA, angleB)` - `drawBisector(angleA, angleB)`
- `sortTeachersAndPrinciple(comparator)` - `sortFaculty(comparator)`

-------

### Have argument names clarify how to use functions

- `transferFunds(sender, receiver)` is better than `transferFunds(a, b) // Function transfers funds from a to b`

ES6 supports named parameters in objects. Consider using that for functions that accept multiple parameters in an unclear order.

- `placeOrder({paymentMethod, cartId, userId})`
