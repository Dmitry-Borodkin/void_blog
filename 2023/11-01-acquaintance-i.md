# Acquaintance I.


### About the author.

Let me introduce myself: Dmitry Borodkin, 58m, professional programmer. Experience +40y...
I have been writing in C/C++ for most of my career.
For the last few years I have been working in the field of visualization of “scientific” data
(Qt/OpenGL, Ultrasonic NDT).

An unaccomplished mathematician. Passionate programming language enthusiast.
I tried a lot of what is compiled/interpreted/etc...
Experimented endlessly with my own "under-language" "under-projects"...

This continued until the fall of 2019.
Then I finally understood exactly how to get the language
and programming system that I needed...


### About the idea.

#### Wishlist.

Here is a list of the desired properties of the language and programming system:

- The language must be low-level enough at its core that the "bare metal" can be easily accessed.
In the best traditions of the C language.

- Provide a truly limitless ability to extend the language regardless of complexity and/or common sense.
So that you can do something like this:

    - Sculpt the language C out of the blue.
    - Extend it to C++.
    - Add Borrow Checker like Rust.
    - Above this, place a thick layer of Haskell-style monads with transformers.
    - And decorate everything with JavaScript libraries on BrainF*ck...

- It is necessary to be able to create "third-party" language extensions.
So that these extensions can be included in "third-party" libraries and applications.
To make it possible to work with language the way mathematicians work
with their notation systems for their theories, proofs and other constructions.

- In the future, it will be necessary to be able to work with mathematics itself,
with the semantics of the language (languages) and “verification-driven development” in general...

- Etc...

This list goes on and on...
It’s easier to formulate something like this:
I need a language which extensibility would be limited only by decidability and imagination.

But how to achieve this?


#### Babylonian tower of metalanguages.

Traditionally, meta-programming capability is added to an existing language in the form of some additional "meta-language".
Like macros, templates, etc. That is, we get:

- language ==> language + meta-language.

But what if I want to do meta-programming for this new, extended language? Extend it further?

- ... ==> language + meta-language + meta-meta-language...

And continue even further? Ad infinitum? Until which one exactly? Omega? Continuum?

Something is wrong here...

Let's take a closer look at "meta-language". What should be at its core? What is its function?
It must provide:

- Model of the "language" structures (CST, AST, types, IR, etc).
- Tools for analysis, transformation, generation of these structures.

But wait a minute. We already have it all. In the compiler.
And we must “drag” this entire “kitchen” from the inside out in the form of a “meta-language”...
Well, okay...

But then for this we will have to build a “Model of the Model of the Language” in the compiler...
And continue this further and further...

No, something is somehow completely wrong here...


#### How is this solved in natural languages?

I am, of course, not a very big expert in linguistics. Let's put it bluntly - an amateur.
But still, “I did my own research.” :wink: And this is what I got:

- In natural language there is something like a "kernel", a kind of "basic vocabulary".
- This "kernel" has nothing to do with reflection.
It contains only the most basic "load-bearing structures" of the language.
- The main “semantic load” is distributed across “subject areas”.
- The language itself “lives” in its specific subject area.
- But as for reflection, this remains unclear to me...

Not much on linguistics, but I still found something interesting...


#### Idea: separate the language from the meta-programming tool...

It turns out that my whole problem arises due to the fact that
I am trying to “stuff” a meta-programming tool into the language itself...

Wonderful! Let's just not do that... And what will we get?

- “Simply” a language that should be able to invoke at least something.
- "Simply" a meta-programming library that is accessible from within the language.

That's all!.. Well, almost...

That is, we can make the language the way we want - minimalistic and low-level.
The main thing is that it must call our meta-programming library.
And this library may be in the compiler.
Even better: the compiler itself can become a library! Everything is already there!
You just need to "inject" a good dose of *Extensibility* there...

All that remains is to figure out how to “technically” pull this all off...


#### Looking for a suitable platform.

In principle, it is clear that almost any existing “virtual machine” of any (if any)
existing developed programming language can be suitable as a platform for such a system:
[JVM](https://en.wikipedia.org/wiki/Java_virtual_machine),
[CLR](https://en.wikipedia.org/wiki/Common_Language_Runtime),
[LLVM](https://en.wikipedia.org/wiki/LLVM), etc.
You can start from some interpreted languages: Python, JavaScript, ~~BrainF*ck~~ ...
You can write your own interpreter. You can even take kinda WASM...

The main thing is that we need to be able to build
“object” code “on the fly” and be able to immediately execute it.
Call functions of the compiler itself (i.e., meta-programming library)
and thus “build” the compiler (and language) to an arbitrarily high level...
Incrementally, layer by layer.

My desire to make the language low-level and as close to the hardware as possible
leads to the rejection of options based on interpretation.

The requirement for a minimalistic language also allows us
to discard options with "over-regulated" memory management...

The minimalism of the language is dictated by the desire to gain maximum freedom for future extensions.
This, in turn, entails transferring the “center of gravity” of the functionality from the language to the compiler
(i.e. "library").


So we need a "rich platform"...


#### Decision-making...

As a result, it turns out that there is not much to choose from. We get LLVM.

With all its shortcomings, at the moment, for my purposes, LLVM has no competitors...

Well... After a couple of months of experimenting,
I seemed to have gotten the hang of it a little
and began to understand how to tame these beasts...
I mean both of them: LLVM and Void.

What a “sweet couple”...


---

2023.10.28 - 2023.11.01

