---
layout: post
category : haskell
title: A Monad Non-Tutorial
tagline: "...or why you shouldn't ask what a monad is."
tags : [functional, haskell]
---

"What is a monad?" is one of the most common question when you're learning 
Haskell. And it's there when troubles start, because it is the wrong question. 
Ask a wrong question, and you'll get the wrong answer. The only right answer 
to this question is a mathematic definition:

> \[...] A monad is a triple `(M, unitM, bindM)` consisting in a type constructor 
> `M` and a pair of polymorphic functions `unitM :: a -> M a` and 
> `bindM :: M a -> (a -> M b) -> M b`
>
> These functions must satisfy three laws \[...]

{% highlight haskell linenos %}
(unitM a) `bindM` k = k a
m `bindM` unitM = m
m `bindM` (\a -> (k a) `bindM` (\b -> h b)) =
(m `bindM` \a -> (k a)) `bindM` (\b -> h b)
{% endhighlight %}

After a reverential minute of silence, you start to figure it out that maybe 
you shouldn't be asking anyways. Your mind is working fast to come out with a 
phrase to fool your interlocutor into thinking that you have understood. 
But there's nothing to understand.

You'll see, that's the way definitions work. They are used to label objects, 
that's the only thing they are good for. So if the definition of homo sapiens 
is "an animal, mammal, of the order of primates, family hominidae and genus 
homo" and if you know what is an animal, a mammal, a primate... etc. then you 
can put objects into classes: "my friend Ed, homo sapiens; mi dog Buch, not."

## It is why, not what

The least interesting thing about monads is its definition. The real question 
is "why?" You only make a definition if it is useful, if you use it frequently. 
So, let's see how this definition is implemented in Haskell:


{% highlight haskell linenos %}
class Monad m where
    (>>=) :: m a -> (a -> m b) -> m b
    return a :: m a
{% endhighlight %}

So, a monad is an Haskell class. Or, if you are a Java guy (and let's confess 
it, who isn't?), then you can think in a monad as an interface. But, again, 
why? When you see `FileishInterface` with its methods open, close, read, write 
and seek, you say: "Oh, so FileishInterface represents objects that behave like 
a file, I get it". You can think in a FileishInterface instance using a TCP 
connection or a byte array. With monads, there is nothing in its
interface that make you think in a concrete example.

An that is a good thing. After all, FileishInterface is so concrete that it 
could be used only in a very few cases. The interface of Monad is so generic 
that it could be used in a wide range of objects. Paradoxically, that makes 
it hard to think in a use when you are learning the concept.


## When

The other problem when you are learning monad is that you want to use them. 
You open your editor and say "I'll make a program and I'm gonna create my 
own monads and I'll show them in the Haskell Café and I'll become a member of 
the Secret Category Cabal and I'm gonna spend my evenings talking about Kleisi 
arrows and functors and maybe Paul Hudak will friend me in facebook". 
Curiously, you have never say "Mmm, today I feel like I'm gonna make a program
that uses hashtables". You don't try to impose data structures to your 
imperative programming, so why are you trying to force monads in your haskell 
programs? Don't go after monads, let the monads find you.

You'll know when you need them. One day you will make a program and you'll 
write a type constructor:

{% highlight haskell linenos %}
type M a = ...
{% endhighlight %}

(did you notice how your type constructor's name is "M" as in "Monad"? This is 
a clever, subtle way in which the author -- me -- hints that you've started to 
write a monad).

If you use your type constructor `M` to create only one type, say `M Integer`, then 
you don't need monads. But, after a few hours of programming, types start to 
appear and you have `M Char`, `M String`, `M Bool` and others. After a few coups of 
coffee you have functions all over the place with types 
like `Integer -> M Integer`, `Integer -> M Bool`, `Bool -> M Char`.

Then you need to chain these function together, but it's a pain in the 
functional ass. Each time you need to connect two functions, say 
`Integer -> M Bool` and `Bool -> M Char`, you need to invoke the first function, 
extract the value inside `M` and invoke the second one. And it is then --I hope-- 
that the "aha! moment" comes to you. You can write a function that extract the 
`a` from `M a` and use it to invoke a function with type `a -> M b`. You have
invented `>>=`, the **bind** function!

Since you wrote the type constructor `M`, you are the one who knows how to extract 
`a` from `M a`. And you know how to chain that value to `a -> M b`. Each monad has 
its own definition of `>>=`, adapted to its structure.

When is return used? If you see the `>>=` definition, it needs a `M a` to start. If 
you have a value of type `a` instead, you'll need a way to transform it into 
`M a`. That's what **return** does.


## Now what?

Hopefully you know by now that the definition of monads is not the important 
part for a programmer. You only need to focus in the type constructor and the 
bind function. What does the type constructor represent? What is it used for? 
How do they defined bind: how does it extract the value inside one of those new 
types and how does it chain it to the `a -> m b` function?

So, now you can go back to those tutorials that explain what are monads and give 
them a second look. You can review the **do** notation that help you to write your 
long chain of `>>=` expression in a clear way. Good luck!


## The IO Monad

But wait! I can't end this post without talking about IO monads, can I? A monad 
is a monad is a monad. IO monads are not the exception. Since your understanding 
of monad is better now and my hands are tired, I'll cheat a little.

An `IO a` value can be seen as a normal imperative *program* that, when executed,
returns a value of type `a`. For the `IO` monad, the **bind** operator is used to
compose those "programs" with functions that return "programs" into new, more 
elaborated ones. That's why the haskellers can see you right into the eye
and say that Haskell is a pure language: "Of course that putChar is a pure 
function. Giving the same input, it will always return the same C program!".

Thinking in this way is useful to understand why IO is a monad, but it is a waste 
of your attention. In your daily programming, you'll be better thinking that the 
IO values are an imperative language embedded in haskell and `>>=` is its interpreter.

