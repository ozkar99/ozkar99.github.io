---
layout: post
title:  "Why i have stopped using racket"
comments: true
date:   2015-06-07
---
<p class="intro"><span class="dropcap">S</span>ince the beginning of this school semester, i've made myself use lisp and its variants.</p>

For 2 particular assignments, i have chosen racket as my goto language, mainly because the tooling is crazy good.
DrRacket is great, it even has a graphical debugger and everything.

This post is about the things i don't like about the language and that really shuts me down, and prevents me from using it.

### Strict Typing + Dynamic Typing - Duck Typing = Not so awesome.
Coming from ruby i have come to love the simplicity that duck typing + oop brings to me.

For example, to do a comparison in racket is quite simple: {% highlight lisp %}(= a b) {% endhighlight %}
and if 'b' is the same as  'a', it will be true.

Or so thats what i thought, in reality doing that will only work if both a and b are of the same type, there is no explicit conversion (racket is a strictly typed language after all.)

A more real life example would be something like:

```scheme
(define input (read))
(if (= 3 input)
    #t
    #f)
```

Except that that code would blow up, since read returns a string :/

So the fix would be to convert the input to a number like this:
```scheme
(= 3 (string->number input))
```

This quickly adds up and its just boilerplate... it breaks my heart.

In a language with duck-typing like ruby the comparison implementation for a number could be like this:
```ruby
def == that
  self == that.to_i
end
```

where that is an object that respond ***.to_i*** (to integer) for example.


### Function clutter.

The lack of generic functions that work on a wide-range of data-type primitives quickly snowball into a clutter of small functions where the types are in their names, a couple of examples are:

- string->number
- number->string
- =
- string=?
- number=?
- equal?

Just to name a few.
In my opinion,  the function ***=*** should wrap and do dynamic dispatching depending on the type.

### Anything Else?

I understand this minimalism, and having done everything explicit is from the scheme school of thought, like i said, i chose racket because of its tooling, i guess i am not really tuned to the schemers idiosyncrasy.

One quick example of this is the following scheme vs lisp comparison that really show each branch ideology:

scheme:  
```scheme 
(set x (+ x 1)) 
```

common-lisp:
```scheme
(incf x)
```

Notice how common lisp has a function that increments by one, this really resonates with me, ***incf*** also does the set implicitly which i love

### Whats next?

Doing less racket, doing more common-lisp its not racket fault that it was not the laguage i expected it to be, thats my fault for being in the wrong band-wagon.

I still think racket is the best scheme implementation there is and dr racket is and i repeat myself because there are no other ways to describe it: 'crazy good'.

That being said, i thing ill be more comfortable on common-lisp land than in racket land, and i cant wait to start learning about the crazy stuff cl has like clos.
