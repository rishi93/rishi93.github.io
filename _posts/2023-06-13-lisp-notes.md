---
layout: posts
title: "Lisp notes"
date: 2023-06-13
tags: programming lisp
---
> Lisp is the greatest single programming language ever designed - Alan Kay

You can try out clojure at [tryclojure](https://tryclojure.org).

Lisp stands for LISt Processing, and code is written in lists.

The first argument of a list needs to be a function. The rest, are the arguments
to that function. 

Lists are enclosed in parentheses and separated by spaces. `(1 2 3)` is a list.

```lisp
(not true) // not is the negation function and true is the argument
// prints false
```

Mathematical operators are like normal functions.
So instead of `4 + 2`, you will do
```lisp
(+ 4 2)
```


## Clojure syntax
```lisp
(+ 1 2) // this prints 3
```
```lisp
"hello world" // strings are enclosed in ""
```

A list is a collection of items like so: `(1 2 3)`
```lisp
(list 1 2 3) // Create list using the list function
'(1 2 3) // Or by prepending ' operator
```

The `reverse` function reverses a collection.
So if you pass a string, it will use it as a collection of characters.
```lisp
(reverse "hello") // ("h" "e" "l" "l" "o") 

