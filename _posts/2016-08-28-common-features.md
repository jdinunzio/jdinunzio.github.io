---
layout: post
category : programming
title: The One Thing Most Influential Programming Languages Have in Common
tagline: (other than C, that is)
tags : [programming]
---

There's a small number of programming languages which have been highly 
influential, some because they were the first ones to implement a new paradigm, 
others because are their best example. In particular, I want to talk
about Smalltalk, Lisp, Haskell and Forth.

These languages have a lot in common. They usually are conceptually simple, 
with clean syntax and a small set of unifying concepts. But, what I find 
most fascinating is not the similitude between their design concepts, 
but that they seem to have similar phylosophy on the way programs are written in them.
They seem to facilitate certain way to achieve answers than other more
common languages seem to lack.

**Smalltalk** is perhaps the best representative of Object Oriented Programming
paradigm. The OO languages that came after it copied and emphasized the
wrong aspects of what OOP is to retrofit a more imperative and
"efficient" implementation. The key realization of Smalltalk is *message
passing*, where you have a network of objects working together and comunicating
by sending message. Every object knows the *protocol* to speak with
the objects it needs (the messages these objects understands) but nothing
else about them. That allows an easy replacement of objects with others which 
understand the same messages. And, since messages are implemented as methods,
to be able to easily write replacement objects, it's convenient for
every method to be as short as possible and delegate as much of its work as possible to
other objects.

**Lisp**, as you may know, is a multiparadigm programming language. You can program
in whatever style you like, imperative, object-oriented, functional, even
declarative. Lisp programmers write indistinctly long or short functions.
But the most distinctive characteristic of Lisp programming is its use of macros
that allow it to "grow" the language and convert it in a Domain Specific 
Language for the problem at hand. This is another form of bottom-up programming,
in my opinion.

**Haskell** is one of the most interesting, purest examples of functional
programming. Not only "everything in Haskell is a function", but it has
*lazy evaluation*, a value is only calculated when it's needed. In Haskell,
like in every functional language, function composition is the cornerstone
of problem solving.

```haskel
-- Given a list, take all but the first three elements that satisfy "p"
-- and return the first five
take 5 . filter p . drop 3
```

This composability, together with the most general abstractions provided
by Cathegory Theory, make Haskell a very expressive language.

Finally, there's **Forth**. This is both a low and high level language of a paradigm 
that is now know as *concatenative programming*. Forth explicitly uses a stack
to pass its arguments and return its values. This fact alone, forces the
practice of writing small functions, since it's very hard to follow the stack
state of a long sequence of instructions. 

But, there's more to the eye in Forth that just stack manipulation. In 1984,
Leo Brodi wrote [Thinking Forth](http://thinking-forth.sourceforge.net/) to 
explain Forth phylosophy, which is based in composability of independent
components (where "independence" doesn't mean "information hidding"). His thesis
is that Forth composability allows a powerful form of expression that permits
to describe programs in a way closer to the *solution space*. You express the
algorithm in its own terms, and not in the language's syntax.

Even if you don't plan to write a line of Forth in your life, I highly recomend to
read "Thinking Forth", it's one of these rare books that will change you percepction
about programming.

As you can see, some of the most influential programming languages, each one
following its own particular philosophy, have come to similar conclusions about
the best program practices:

> You should be able to describe a program in a formal way in the *Solution Space*,
> reducing to a miminum the bookkeeping that every language impose.

