+++
tags = ["tech"]
categories = [ "post" ]
date = "2015-07-28T17:02:35-04:00"
lastmod = "2019-10-12"
summary = "A tutorial to get beginners to Clojure or web development acquainted with using Compojure and Heroku."
keywords = []
title = "Clojure web server tutorial"
+++
(This tutorial is INCOMPLETE, but was first given in its complete form as a talk at a Michigan Hackers event on 2015-07-23. See my slides [here](https://github.com/vinayh/clojure-web-talk). I have explained the first portion of the content here in more depth.)

I will start by talking about some basics of Clojure, then move onto explaining the fundamental purpose of a backend in web development, and finish with a demo of a simple web app (with complete code) that you can deploy to Heroku with a one-click button or try out on another platform of your choice.

## What is Clojure? (besides the best language ever!)
* A dialect of the Lisp language, which means that the syntax uses elegant S-expressions with the code-is-data philosophy
* Based on the Java Virtual Machine, which means that it is quite fast when compiled to Java bytecode and that you can easily integrate other Java libraries or use your Clojure code in a Java codebase
* Strong emphasis on functional programming, which improves your ability to reason about large projects because state is effectively eliminated

## Tools you will need
* Leiningen (for local Clojure development) - [link](http://leiningen.org)
* Heroku Toolbelt (if you want to deploy to Heroku) - [link](https://toolbelt.heroku.com/)
* PostgreSQL (for a local database) - [link](http://www.postgresql.org/download/)

## Some basics of Clojure
As mentioned earlier, variables are essentially always immutable in Clojure:
```clojure
(def my-name "Vinay")
```
This defines a constant called `my-name` that refers to the string `"Vinay"`. The code-is-data philosophy allows for assigning variable names to functions (among many other things)
``` clojure
(def my-function (fn [x] (+ 5 x)))
```
This creates a function called `my-function` that returns the sum of `5` and a parameter `x`. Note the prefix notation for arithmetic operations here, which maintains consistent function notation. Think of the `+` function as being called with two operands, `5` and `x`, and returning the resulting sum.

## More basics of functions in Clojure
.Let's simplify the function notation above using the `defn` macro to express the same thing:
```clojure
(defn my-function [x] (+ 5 x)
```

Note that to define a function, `defn` is followed by:

  * The function name (eg. `my-function`)
  * The function parameters (eg. `x`)
  * The function implementation (eg `(+ 5 x)`)

You can call the function like so:
```clojure
(my-function 4) ;; Result is 5 + 4 = 9 in this case
```

## Confused by all of the parentheses?
Clojure uses S-expressions to denote nearly every statement as a list. As you probably noticed above, function definitions could be denoted as a list, with the elements: `defn`, the parameters in a vector (i.e. enclosed by `[ ]`), and the implementation. Just about everything in Clojure uses this syntax, and another common example is the `let` form which:
  
  1. Binds variables to values provided as pairs, eg. `[variable_1 value_1 variable_2 value_2]`
  2. Evaluates expressions with this context or scope

      ```clojure
      (let [x 1
            y 2]
          (+ x y))
      ```

The whole `let` expression evaluates to the result of the last expression, which in this case is the sum of `x = 1` and `y = 2`, so `3`

## An arithmetic expression
A benefit of this: no order of operations because the parentheses and prefix notation make things very explicit. Simply evaluate by recursing into every S-expression:
```clojure
(- (+ 1 (/ (* 2 3) 4)) 5)
```

The threading macros in Clojure provides a procedural alternative to this with beautiful syntax. Rewriting, starting at the `2` in the nested expression above:
```clojure
(-> 2
  (* 3)
  (/ 4)
  (+ 1)
  (- 5))
```
The thread-first macro shown above `(->)` inserts the result of each expression as the first argument of the expression below it, then continues the process with the expressions below that one in turn.

## To (maybe) be continued...
The slide deck (HTML) linked above is a more self-contained introduction that includes all of the information in this post.
