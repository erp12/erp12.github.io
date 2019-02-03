---
title: Pretending Clojure Has Argument Defaults
date: 2016-11-22
draft: false
---

Although Clojure doesn't have the ability to specify the default value of a function argument, it is common to use multiple function signatures to achieve serve the same purpose. We can also "destructure" a variadic function (function is arbitrary number of arguments) to get default argument values.

### Simple Example

A great example of having default values for function arguments is the `range`. Of course, this function is [built into Clojure](https://clojuredocs.org/clojure.core/range) but it makes a great demonstration of default argument values.

Below is an implementation of a function that has a similar behavior as `range`, except it returns a vector. Let's call our function `my-range` and write it to take the same arguments as `range`.

```clj
(defn my-range
  [start end step]
  (loop [i start
         result []]
    (if (> i end)
      result
      (recur (+ i step)
             (conj result i)))))
```

Let's compare our `my-range` function to Clojure's `range` function.

```clj
(my-range 0 5 1) ; => [0 1 2 3 4]
(range 5)        ; => (0 1 2 3 4)
```

Wouldn't it be nice if we could call `my-range` with only 1 or 2 arguments, just like `range`? The `range` function assumes that `start` is 0 and `step` is 1 unless otherwise specified. Lets implement that:

```clj
(defn my-range-2
  ([end]
   (my-range-2 0 end))
  ([start end]
   (my-range-2 start end 1))
  ([start end step]
   (loop [i start
          result []]
     (if (>= i end)
       result
       (recur (+ i step)
              (conj result i))))))

(my-range 5)    ; => [0 1 2 3 4]
(my-range 5 10) ; => [5 6 7 8 9]
```

When our functions have only 1 signature, we list our arguments in `[]`s directly after the function name. When we want to write a function with multiple signatures we put multiple sets of `[]`s, each in their own `()` expressions. Using this, we can write a single function that has a different behavior based on the number of arguments passed.

Notice how the two branches of `my-range-2` that don't have all 3 arguments simply call `my-range-2` with the default argument values we wanted?

>Stuart Sierra has a great demonstration [here](https://stuartsierra.com/2015/06/01/clojure-donts-optional-arguments-with-varargs) of this concept used more generally in the context of optional arguments.

In other words, when we call `my-range-2` without all three arguments, the `my-range-2` function calls itself with all missing arguments set to their default values.

### Can we do better?

What if we wanted to specify a value for `end` and `step` but use the default value for `start`. Not even *Clojure's* `range` supports this.

We *can* do it. But *should* we? I rarely stop to ask myself that question until its too late, so let's implement it.

```clj
(defn my-range-3
  [& {:keys [start end step] :or {start 0 step 1}}]
  (loop [i start
          result []]
     (if (>= i end)
       result
       (recur (+ i step)
              (conj result i)))))

(my-range-3 :step 2 :end 10) ; => [0 2 4 6 8]
```

This changes the way which we have to call our function, but it works!

In Clojure, we can pack an arbitrary number of arguments into a single collection that is given one name using the `&` symbol. This is called a *variadic function*, apparently. I didn't actually know that until I looked it up to write this post. For example:

```clj
(defn foo
    [x & args]
    (apply str args))
(foo "I" "Have" "So" "Many" "Arguments") ; => "HaveSoManyArguments"
```
Notice that `"I"` is bound to `x` so when we `(apply str args)` it is not included in the result.

We can "destructure" the arbitrarily long collection of arguments and bind each value to a symbol, such as `start`, `end`, or `step`. When destructuring, you can also specify default values to be used when a key isn't present in the collection of arguments.

>For a better explanation of destructuring, see [Clojure's destructuring guide](http://clojure.org/guides/destructuring).

A bonus feature of this approach is that you can set arguments in any order you want. Notice that in our call to `my-range-3` we have set the step before we set the `end`, which is not possible using Clojure `range` function.

Let's revisit the question of *should* you implement your function in the way we implemented `my-range-3`. I suppose this would come down to personal preference, but my recommendation is that you avoid destructuring a variadic function in order to get default values for your function's arguments.

I am going to echo [Stuart Sierra's warning](https://stuartsierra.com/2015/06/01/clojure-donts-optional-arguments-with-varargs) while we consider this example:

```clj
(my-range-3 :step 2 :end 10 :reverse true) ; => [0 2 4 6 8]
```

But our `my-range-3` function doesn't support a `:reverse` argument! Those using our code will be woefully mislead when they accidentally pass the incorrect amount of arguments yet the function doesn't throw an error or change behavior.

I personally find destructuring to be much harder to read, compared to multiple function signatures.

I also personally find the way destructuring changes the function call to be harder to read. Sure, Clojure users are familiar with alternating key-value arguments, so maybe that doesn't bother you.

As far as performance, I have not thoroughly tested the difference between `my-range-2` and `my-range-3`. I will say that both *seemed* to perform similarly when given just the `end` argument, but `my-range-2` started outperforming `my-range-3` once all 3 arguments were specified.

```clj
(time (repeat 10000 (my-range-2 0 100 10)))
; "Elapsed time: 0.047652 msecs"

(time (repeat 10000 (my-range-3 :start 0 :step 10 :end 100)))
; "Elapsed time: 0.083339 msecs"
```

If anyone has any other ways to implement default argument values in Clojure, I look forward to reading about them in the comments! Got a useful improvement to the information here? Be sure to let me know!
