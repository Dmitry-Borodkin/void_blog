# Acquaintance II.

This post is a continuation of the previous post:

- [Acquaintance I. About the author and idea.](11-01-acquaintance-i.md)


### About the project.


#### General principles.

The `voidc` project involves the creation and gradual joint development of two “components”:

- The "Void" programming language.
- The `voidc` compiler for this language.

That is:

- I don't know what the "ideal" language for such a system should be.
- I don't know what the "ideal" compiler for such a language should be.

I will try to "figure out" these things as I go.

I will need to start with a minimal language and the simplest “unroller” in C++.
Then gradually add extensibility to the compiler and develop the language itself.

At some stage, when the language and architecture of the compiler are “stabilized”,
it will be necessary to make a “bootstrap” and rewrite the “unroller” in Void itself.


Here are some thoughts on my "approaches" in the project:

- I would like the language to be similar to C.
But at the same time, the syntax regarding types should be more “mathematical”.
The example of the Rust language shows that this is quite achievable...

- I consciously try to avoid "homoiconic" solutions. For several reasons:

    - Any such solution has its limitations. And the main one is in your mind -
      “If you have a hammer in your hands, then all problems become like nails”...

    - We just don't need it. We can work with the internal representation of language structures directly,
      without the need to build a special "kitchen" for "reflection"...

- The main emphasis is on the simplest and most understandable solutions:

    - “You can’t optimize something that doesn’t exist.”
    - No "exotic" stuff. Minimum "tricks"...
    - At the beginning, an absolute minimum of diagnostics...

- ...


#### "How it started."

As you can see in the initial commit, everything started out quite modestly:

- The language:

    - No any Unicode.
    - Expressions are NOT recursive.
    - The type system is purely LLVM (v11).

- The compiler:

    - Parser via [PackCC](https://github.com/arithy/packcc).
    - The simplest compilation in the [LLVM Tutorial](https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/index.html) style.
    - The "Main Loop" on OrcJIT...

But at the same time:

- ...




#### "How it's going."

...



---

2023.11.16 - 2023.11.19

